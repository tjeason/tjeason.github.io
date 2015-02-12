---
layout:     post
title:      "Basic Linux Web Server Monitoring"
subtitle:   "Know when your service is down"
date:       2015-02-11 20:00:00
author:     "T.J. Eason"
header-img: "img/python_server_monitoring.jpg"
---

<p>
  So you want to monitor your web application or server?  No problem.  There is a simple way to provide
  monitoring using a Python script and a cronjob on a Linux server.  You can follow the steps below for
  a basic monitoring system, be alerted by e-mail when your app or server goes down.
</p>

<h3>Creating the Server Monitoring Script </h3>

<p>
  First, make sure you have an access to a Linux server with at least execution permissions in a directory you plan
  to a script.  The next step is to create a script that will perform health checks on a single or cluster of
  web applications.  The script written below uses the standard Python 2.x libraries.  The script is named
  <em>server_monitor.py.</em>
</p>

<!-- Full Python Server Monitoring Script -->
<script src="https://gist.github.com/tjeason/05055c7956e7578e3c2f.js"></script>

<p>
  The single Python script uses the system, URL, web socket, and time modules.  The objective is to create a simple
  command line interface program that is reusable for multiple use cases.  The script should be able read the user’s
  commands, make a socket connection request to detect if the web application’s port is being used, and return a health
  check status. E-mail notifications can also be sent if the script returns a specific status.  For example, if a system
  administrator would like to know when an application with an Apache web server used on port 80 is down, then an e-mail
  message will be sent immediately after the script is executed.  Let’s breakdown the server monitoring script to describe
  what is happening in each method.
</p>

<h3>The Main Method</h3>

<p>
  Starting with the main method, this section gets initiated first.  The first line of code in the main method checks the
  required amount of input arguments expected to run when the script executes.  If the user fails to provide the exact number
  of arguments, then the user will receive a help message and directions called in the usage() method.  Otherwise, the number
  of arguments passed is correct and proceed with network health check starting in the server_test() method.  The program is expecting
  to use four arguments: script itself, network test type, hostname with port number, and e-mail address used to send notifications.
</p>

{% highlight python linenos %}

# Main method - Invoke script and parse arguments.
if __name__ == '__main__':
    if len(argv) != 4:
        print('Wrong number of arguments.')
        usage()
    elif not server_test(argv[1], argv[2]):
        send_error(argv[1], argv[2], argv[3])

{% endhighlight %}

<h3>The Usage Method</h3>

<p>
  The usage outputs a summary of information about how to properly run the script.  As you can see, the methods contains only
  print statements to give the user hints.  The first print statement displays the following the arguments types requested.
  The rest of the print statements include short descriptions of each argument type.
</p>

{% highlight python linenos %}

# Help menu.
def usage():
    print('%s <test-type> <server-info> <email-address>\n' % (argv[0]))
    print('\ttest-type    \ttcp or http')
    print('\tserver-info  \thostname:port for tcp')
    print('\t             \thttp://hostname/page for http')
    print('\temail-address\tusername or username@domain.com\n')

{% endhighlight %}

<h3>The Server Test Method</h3>

<p>
  The server_test() method is called in the main function if the number of arguments are correct.  The method requires two
  parameters, network test type and the hostname (IP address) with port number. For this script, the script is looking for
  either HTTP or TCP requests. Using one of the network protocols will determine the next method to be used to handle the
  sent host properly.  Anything parameter that not state either “http” or “tcp” in the command is flagged in the else block
  with a print statement to inform the user about the incorrect test-type.
</p>

{% highlight python linenos %}

# Parse arguments passed from the script, and determine which test was requested.
def server_test(test_type, server_info):
    if test_type.lower() == 'tcp':
        return tcp_test(server_info)
    elif test_type.lower() == 'http':
        return http_test(server_info)
    else:
        print('Invalid test-type given, please use either tcp or http.')
        return True

{% endhighlight %}

<h3>The HTTP Test Method</h3>

<p>
  The http_test() method sends a request to the specified server and port to determine if the HTTP connection is a success.
  The function requires on parameter, which is the server information (host and port).  The value passed should abide by a
  specific expression:
</p>

<blockquote>
  hostname:port (ex. mydomainname.com:80)
</blockquote>

<p>
  The method calls the urlopen() method from the urllib2 module to read the URL with a HTTP response. The urlopen().read() will
  return the HTML content from the URL’s web page.  If the HTTP request was a success, then returning True will not perform anymore
  steps. The web application is running on the server.  Returning false will call the send_error() method to send an e-mail
  notification that the server is down.
</p>

{% highlight python linenos %}

# Establish a HTTP(S) request to a server, and report if attempt was successfull.
def http_test(server_info):
    try:
        data = urlopen(server_info).read()
        return True
    except:
        return False

{% endhighlight %}

<h3>The TCP Test Method</h3>

<p>
  With this Python script, you can also use TCP instead of HTTP for your server health check.  The tcp_test() method uses web sockets
  for network statuses.  The method requires one parameter.  The host and port number is passed as a string.  The server information is
  parsed to operate the port number by finding the colon (:).  For instance, the argument should be the same as mentioned in the HTTP Test
  Method section:
</p>

<blockquote>
  hostname:port
</blockquote>

<p>
  If the validation fails, then an output notification can be written to inform the administrator that the expression is incorrect.
  Passing the validation check will the begin the process of creating a web socket using the imported socket module to perform the TCP connection test.
  If a socket connection is made successfully to the host, then the web application passed the TCP check and returning True as the value.
  Returning false will call the send_error() method to send an e-mail notification that the server is down.
</p>

{% highlight python linenos %}

# Establish a TCP connection to hostname:port, and report if attempt was successfull.
def tcp_test(server_info):
    cpos = server_info.find(':')
    if cpos < 1 or cpos == len(server_info) - 1:
        print('You need to give server info as hostname:port.')
        usage()
        return True
    try:
        sock = socket()
        sock.connect((server_info[:cpos], int(server_info[cpos+1:])))
        sock.close
        return True
    except:
        return False

{% endhighlight %}

<h3>The Send Error Method</h3>

<p>
  The final method being used in this script is the send_error() method.  The primary responsibility for this function is to send an e-mail
  notification to the desired users about either a HTTP or TCP  health check failing.  The method requires the Linux mail command to be
  installed on the server.    A subject and message body is created here.  A Linux system call is used to perform to run the mail command with
  the arguments. The method requires three parameters values: the network test type (HTTP/TCP), host and port number, and the e-mail address
  of the receiver(s).  The network test type is used to inform what kind of health check was executed and time. The server_info parameter provides
  what host (domain name or IP address) and port was tested, and the e-mail address to inform a administrator about the failed status.  The default
  e-mail message is short and direct to notify an administrator with enough information.  You can see an example e-mail below the code section.
</p>

{% highlight python linenos %}

# Create e-mail message, and send notification.
def send_error(test_type, server_info, email_address):
    subject = '%s: %s %s is down' % (asctime(), test_type.upper(), server_info)
    message = 'Server Monitor: Performed a health-check running a %s test against %s. The server is down.' % (test_type.upper(), server_info)
    system('echo "%s" | mail -s "%s" %s' % (message, subject, email_address))

{% endhighlight %}

<h3>Setting up Automated Recurring Health Checks</h3>

<p>
  Once the script is tested and working the way you want, you can setup basic scheduling using a Linux cron job.  In your home directory, you
  can create a crontab.  To edit your crontab file, type the following command in the Linux shell prompt:
</p>

<blockquote>
  $ crontab -e
</blockquote>

<p>
  Next, you need to follow the proper syntax (field description) to have the system run server monitoring script the way you want.  For example,
  you can run the script per minute, hour, day, etc.  The sample below shows running two different types of test.  One job is scheduled to run
  every 10 minutes to perform a TCP health check on web app server running on port 9000.  The other cron job is scheduled to run every 30 minutes
  to do a HTTP health check on the proxy server using port 80.  Failed health checks will send a text-based e-mail to the e-mail address set in
  the cron jobs.
</p>

<blockquote>
  */10 * * * * /home/username/server_monitor.py tcp mydomainname.com:9000 admin-email@email.com

  <br><br>

  */30 * * * * /home/username/server_monitor.py http mydomainname.com:80 admin-email@email.com
</blockquote>

<p>
  Once you save and exit, such as using the VI editor, your crontab is started.
</p>

<p>
  You can learn more in more details about the crontab by going <a href="http://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-linux-or-unix-oses/">here</a>.
</p>

<h3>Conclusion</h3>

<p>
  I this post gives you an idea on how to set your own web application/server monitoring.  It’s been very helpful for me to alert me by e-mail when
  something is down.  The script can be improved.  For instance, You could setup logging using a Python log module to create log file(s) to keep a
  close eye on your monitoring and tracking historical data.  Collected data can come in handy for troubleshooting or even performing extraction to
  visualize the data in a dashboard. I hope this information helps! Happy coding and administrating!
</p>

<hr />

<div id="disqus_thread"></div>
<script type="text/javascript">
/* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
var disqus_shortname = 'tjeasongithubio'; // required: replace example with your forum shortname

/* * * DON'T EDIT BELOW THIS LINE * * */
(function() {
  var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
  dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
  (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
  })();
  </script>
  <noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>

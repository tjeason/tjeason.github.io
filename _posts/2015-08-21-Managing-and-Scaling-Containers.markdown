---
layout:     post
title:      "Managing & Scaling Containers"
subtitle:   "The challenges and potential solutions"
date:       2015-08-21 14:00:00
author:     "T.J. Eason"
header-img: "img/containers_bg.jpg"
---

<p>
In the IT Infrastructure industry, a new technology shift is happening.  A transition just as big as moving from physical (bare-metal) servers to virtual machines just over ten years ago.  Now, with virtualization can easily be done on a single physical computer to cloud, using containers is the next big step to be more efficient.  
</p>

<p>
Even though <a href="https://linuxcontainers.org/" target="_blank">Linux Containers (LXC)</a> has been available to use in a while, the mass market did not even bother on how to figuring out to use them until the companies, Docker and Google, began publishing their work.  The <a href="https://www.docker.com/" target="_blank">Docker Container Runtime Engine</a> was developed as a side project to figure out a simpler way to use Linux containers.  Since Docker’s launch of its tools to simplify distributing and running apps, databases, and back-end API services, usage grew dramatically in the developer and operations community.  Development and operations teams loved the benefits of provisioning containers within seconds compared to minutes on traditional virtual machines, less  CPU usage on the host, and a guarantee an app running inside a container will run in any environment with minimal to none configurations on the host instances.  Even infrastructure architects are seeing the benefits of using containers.  Architects can move away from monolith (or silo) architectures to using a micro-services design. Cross communication can happen between various apps, databases, and web services using RESTful APIs.  A DevOps team can take down or update services without effecting other areas.  In result, a much more loosely-coupled architecture.  The transition from virtualization to containerization has gotten so popular even Microsoft is developing a version of Windows Server to run inside containers and partnering with Docker.  
</p>

<p>
Great! The transition is happening and developers and operation teams are starting to figure out how to use containers to host their apps and services in sandboxes or production. But once the transformation happens, the next challenge is how to handle large scale deployments of containers.  IT Infrastructure Engineer and Operations Teams now have to figure how to orchestrate and manage hundreds to thousands of containers.  Companies and open-source communities are developing a variety of great technologies that will hopefully solve this type of issues.  Two technologies that are widely discussed online are Google’s Kubernetes and Mesosphere.
</p>


<center><img src="/img/kubernetes_logo.png"></center>
<p>
<a href="http://kubernetes.io/" target="_blank">Kubernetes</a> is Google’s open-sourced solution to managing a cluster of Linux containers as a single system to accelerate development and simplify operations.  Google has been using containers for a long time to run services, such as Gmail. Their container management solution has proven to be a great solution to handle usage in a massive environment.  It’s currently only used at the command-line.  One of Kubernetes’s strengths is able schedule large clusters of containers onto nodes to create “labels” and “pods”.  This process makes it easier to manage the grouped containers and discover.  Replication can be done as well.  
</p>

<p>
Another approach is building a “data center operation system” to manage and deploy several containers.  Former Twitter and Airbnb Engineers founded the company and technology called <a href="https://mesosphere.com/" target="_blank">Mesosphere</a>.  Their approach is to organize the entire infrastructure as if it was a single computer. By using the command-line or GUI web app, administrators can launch and manage thousands of containers as a platform compared to just being a tool like Kubernetes.  The technology is built on top of <a href="http://mesos.apache.org/" target="_blank">Apache Mesos</a> to be a kernel for data pooling and handling distributed computing.  Other components to Mesosphere are Marathon for deploying long-running services, Chronos for chron tasks, and graphical dashboard for viewing services. Kubernetes is getting integrated as well.
</p>

<center>
  <img src="/img/mesosphere.png">
  <br>
  <small>Mesosphere - GUI Web App screenshot of managing a cluster of containers.</small>
</center>

<p>
In summary, managing and deployment solutions for containers is still in its infancy. The technologies I mentioned in this article are some of the more interesting and supported tools, which have great potential to be commonly used from startups to enterprises.  There is still plenty of time for companies to decide of running container and micro-services to be a great fit or not.  The tech community recently setup the <a href="https://www.opencontainers.org/" target="_blank">Open Container Initiative</a> to help standardize container formats and runtimes.  The initiative is under the loosely-governed Linux Foundation and supported by companies, such as Amazon, Joyent, Cisco, IBM, Docker, Google, Microsoft, HP, and Oracle. If you have not tried using containers as a developer or system administrator, then now is an exciting time to starting learning before the “big transformation” happens.
</p>

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

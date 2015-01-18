---
layout:     post
title:      "Know When Not to be a Cowboy Coder"
subtitle:   "Choose the right path"
date:       2015-01-17 12:00:00
author:     "T.J. Eason"
header-img: "img/cowboy_banner.jpg"
---

<p>
  When starting new software development projects, there can be an urge to start coding away.
  You know what the outcome should be and have at least a general idea how the software should work.
  Why not begin programming? What are the consequences? These are the type of questions you should be
  asking yourself (and your team) before implementing.
</p>

<p>
  Software development projects typically use the <a href="http://en.wikipedia.org/wiki/Application_lifecycle_management">
  Application Lifecycle Management (ALM)</a>, or can be called Application Development Life-cycle Management (ADLM)
  if DevOps is included.  A common software development practice will involve gathering all the required information
  and resources, planning, design, implementation, testing, and then moving to production.  It is possible that
  developers to skip the planning stage and going straight to writing code and still have a successful release.
  When developers have the freedom to skip the planning stage will lead them to a new, wild adventure.
</p>

<center>
  <img src="http://upload.wikimedia.org/wikipedia/commons/5/5f/Three_software_development_patterns_mashed_together.svg" width="400" height="400">
</center>

<p>
  Skipping requirements and planning stages, and allowing engineer(s) to starting hacking a solution can be considered
  as <a href="http://en.wikipedia.org/wiki/Cowboy_coding">“cowboy coding”</a>.  The software practice is great for learning and prototyping, but I would not recommend developers
  being coder cowboys on highly-visible, critical, or complex projects.  A programmer may receive pressure to hit a short
  deadline, but that does not mean it’s time to put on your cowboy hat and start the design, implementation, and testing phases
  at once.  Let’s look at few of the pros and cons of this practice:
</p>

<h3>Good</h3>

<ol>
  <li>
    Great for learning a new programming language or framework.  Developers can setup a development environment and
    acquire a new skill. Remember, learning how to write software requires “doing”.
  </li>
  <li>
    Creating prototypes with no or soft deadlines. If you have an idea and time for a solution, then writing small
    features or applications is a great way to see if you have a good solution or not.
  </li>
  <li>
    Writing small unit-tests.  After building your logic with your code, it’s good practice to make sure there is no
    flaws and production ready.  Unit tests focusing on a section of your solution can be small enough without having
    to plan and design your unit test framework to see if the tests will pass.
  </li>
</ol>

<h3>Bad</h3>

<ol>
  <li>
    Easier to miss deadlines.  Cowboy coding allows to jump right into the text editor, but with high risks.
    For instance, many managers or customers want to know when development goals can be achieved by a set date and
    time.  By not planning properly, engineers increase their chances of missing deadlines. Missing deadlines can
    impact costs, frustration, stress, and even working longer hours.
  </li>
  <li>
    Reduces team communication.  In software development projects involving teams, no documentation on planning, scope,
    requirements, etc. reduces communication on how a solution is going to work.
  </li>
  <li>
    More difficult to set deadlines.  It can be very hard to set goals to meet at specific timeframes for medium and
    large projects.  Without enough preparation, developers are generating more uncertainty of when to say done!
  </li>
</ol>

<p>
  I have had personal experience dealing with cowboy coding in various ways. From testing and touching code in
  production, it can be risky and not supported.  I now know when I can skip all of the pre-coding steps and the
  consequences.  I recommended software developers think about the pros and cons before jumping straight to writing
  code.  Management and users will have a better appreciation of the work knowing there was proper care placed to
  provide a great solution.
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

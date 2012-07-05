---
layout: post
title: "How We Use Jenkins for Continuous Deployment"
date: 2012-07-06 12:00
author: Louis Chatriot
comments: true
categories: continuous deployment, server setup
---


As all teams working on a web application, we had to setup a [continuous
integration](http://en.wikipedia.org/wiki/Continuous_integration) server. This is 
a fairly well-known engineering practice, where you want your code to be built and tested 
regularly on the server so that you spot problems as soon as they arrive and not when
you push three months' worth of work to production. Also, it makes deployment a much less 
stressful experience, because you know it will work. You avoid getting in the situation 
of a friend of mine who could not have coffee with me because he was deploying and didn't want 
to mess it up!  

We also wanted to take it one
step further, and use continuous deployment. As the name implies, this
means that any new code pushed to the server will automatically be
built, tested and then go live and be used by our users right away. It
turns out that is fairly easy to set up -except for a few caveats-, and
I will describe our setup. This by no means a step-by-step tutorial but
rather a "here is what you can do" article.


<a href="http://jenkins-ci.org/" target="_blank"><img alt="Jenkins CI" src="http://jenkins-ci.org/sites/default/files/jenkins_logo.png"></a>


## Our Tech Stack - The Part Of It That Matters For This Article
Our server runs on Ubuntu 12.04 LTS, our version control system is Git
(we use [Github flow](http://scottchacon.com/2011/08/31/github-flow.html)) and our
code is hosted on Github. We chose to use [Jenkins](http://jenkins-ci.org/) as 
the CI server. Even though it is poorly documented and has a definite
'so nineties' feel, it is very easy to deploy and intuitive. Basically, you use it 
to set up jobs that are run when you launch them manually or when an external event, such 
as a code push, is triggered. These jobs can be anything that's executable, I usually use 
simple bash scripts but you can use any kind of script, makefiles and so on.


## Install And Keep Jenkins Up And Running At All Times
Jenkins is contained in a WAR file, so you can simply download and execute it with
java (command `java -jar /path/to/jenkins.war`). Of course, this is only
suitable for testing purposes. In production, you will want to use an 
[Upstart](http://upstart.ubuntu.com/) script so that Jenkins respawns if
it crashes unexpectedly and starts on boot.  

When Jenkins is up, you have
access to a web interface to manage it (you can configure the port on which
it listens). Make sure to enable security -I recommend matrix based security- unless you want anyone to be
able to take control of your servers! One thing to watch out for is that
you need to enable the "overall read" permission for the anonymous user,
or else Jenkins won't be able to communicate with third-party services - in our case Github and Hipchat. Pretty stupid
behaviour I know, but at least it doesn't create security holes.


## Build On Push To Master
We have the Git and Github plugins enabled. The [Github plugin](https://wiki.jenkins-ci.org/display/JENKINS/Github+Plugin) 
enables us to connect Jenkins to Github's post-receive hook. That way,
whenever someone pushes to master (usually by merging a feature branch),
our Jenkins "build" job pulls the new version of the code, builds it and
test it. For us, that's really a two-line script, as we [check all
modules in to Git](http://www.mikealrogers.com/posts/nodemodules-in-git.html).  

If everything was successful, the "deploy" job is now called. It pulls
the new version of the code in the live repository, then restarts the
node server (since we use nodejs, we can overwrite the javascript files
when the server is running without impacting it). Node starts pretty
fast, so we only have ~0.1s downtime (no load balancer to achieve 0s downtime - for now!). And since we use an external store
to manage sessions, nobody gets logged out.


## Be Notified
The last part of the puzzle is how we get notified of the build result.
We use [Hipchat](https://www.hipchat.com/), so we installed the Hipchat
plugin that notifies us of build results in a dedicated chat room. Most
of the time it just says "Build successful". But no sweat if a build
fails, since the "deploy" job was not called our users don't see
anything wrong!


## And That's All There's To It!
Yup. It is indeed simple enough to get a robust continuous deployment
system up and running, that most of the time safely deploys your code
through a simple `git push` on your part. I'm interested in anything you
would have to say on this setup or any tip you'd like to share!

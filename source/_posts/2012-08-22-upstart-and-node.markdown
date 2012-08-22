---
layout: post
title: "What To Use Upstart (And Other Supervisors) For"
date: 2012-08-22 12:00
author: Louis Chatriot
comments: true
categories: server setup, process supervisor
---


_tl;dr version:  

* We should stop having software handle system tasks like daemonization
  or privilege-dropping
* That's a job for a supervisor. For example we use [Upstart](http://upstart.ubuntu.com/)
* Nodejs plays nicely with Upstart, for example see [this Upstart
  script for launching a nodejs application](https://gist.github.com/3385102)
_


We are currently building a webapp with the following stack: nodejs,
mongoDB, nginx and redis. We also use a certain number of services,
like [Jenkins](http://needforair.com/blog/2012/07/09/jenkins-in-production/) and Graphite.
To use these softwares in production, we need to be able to daemonize
them, run them as specific users (usually with low privileges), and have
tham restart automatically if case of an unexpected crash.


<a href="http://upstart.ubuntu.com/" target="_blank"><img alt="Jenkins CI" src="http://upstart.ubuntu.com/img/upstart80.png"></a>


These are quite common needs. But people are often confused as to what
part of the system is responsible for what task. I often see software
handling the privilege drop (for example the otherwise excellent
[Graphite](http://graphite.wikidot.com/)), or the daemonization (for
example [gunicorn](http://gunicorn.org/) - which is also great otherwise).


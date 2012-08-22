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
  script for launching a nodejs application](https://gist.github.com/3385102)_



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
example [gunicorn](http://gunicorn.org/) - which is also great
otherwise). It's bad, because as a result, some of these softwares don't
play nicely with supervisors like Upstart. People tend to resort to
strange and not well understood scripts (I even saw an Upstart script
that called the _start stop daemon_ to launch a gunicorn process which
daemonized itself!).


This should be a simple problem. The rule to follow is that
*softwares should not daemonize or handle privilege drop*. These are
jobs for the supervisor. That's all, separation of concerns, folks. For
example, we use Upstart to launch our processes as services. That way,
we get added benefits: they get respawned in case they crash, they
restart automatically if we need to reboot the server, and we can
specify rules such as "service A should not start before service B has started
and is ready". I see no reason not to it that way!


An example of software that respects these rules is nodejs. Here is [the
Upstart script](https://gist.github.com/3385102) we use with it. It enables us to start our app as any
user, log all output and get the added benefits I was referring to
earlier. It really is all you need with nodejs and most software you
will use. You have no excuse not to use it now :)

---
layout: post
title: "What To Use Upstart (And Other Supervisors) For"
date: 2012-08-22 12:00
author: Louis Chatriot
comments: true
categories: server setup, process supervisor
---


<blockquote class="tldr-embed-widget" data-align="center">      <p>      <a href="http://tldr.io/tldrs/504def5fe1eaddb23a00002c/what-to-use-upstart-and-other-supervisors-for-" class="link-to-tldr-page" target="_blank">Summary of "What To Use Upstart (And Other Supervisors) For -"</a> (via <a href="http://tldr.io" target="_blank">tldr.io</a>)      <ul>          <li style="margin-bottom: 10px; line-height: 130%;">We should stop having software handle system tasks like daemonization or privilege-dropping</li>          <li style="margin-bottom: 10px; line-height: 130%;">That’s a job for a supervisor. For example we use Upstart</li>          <li style="margin-bottom: 10px; line-height: 130%;">Nodejs plays nicely with Upstart, article links to an Upstart script for launching a nodejs application</li>      </ul>      </p></blockquote><script async src="//tldr.io/embed/widget-embed.js" charset="utf-8"></script>


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
that called the _start\_stop\_daemon_ to launch a gunicorn process which
daemonized itself!).


There is a simple rule to follow to avoid this confusion:
**softwares should not daemonize or handle privilege drop**. These are
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
will use. This is not an Upstart tutorial ([their doc](http://upstart.ubuntu.com/cookbook/) is pretty good), 
but let me explain the most important parts:  

* The privilege drop is done through the `su -s /bin/sh -c 'exec "$0" "$@"' $RUN_AS -- command` part. 
This line executes `command` as user `$RUN_AS`. Unfortunately, we have to use this method because the standard
`sudo -u` doesn't work from within an Upstart script.
* The log file needs to be `touch`ed in case it doesn't exist, then `chown`ed to user `$RUN_AS`, because otherwise 
it will belong to user root (which the user executing the Upstart script)
* The rest is pretty standard

Of course, this script doesn't work just for nodejs! So you have no excuse not to use it now :)

---
layout: post
title: ""
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


<a href="http://upstart.ubuntu.com/" target="_blank"><img alt="Jenkins CI" src="http://upstart.ubuntu.com/img/upstart80.png"></a>



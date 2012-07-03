---
layout: post
title: "How We Use Jenkins for Continuous Deployment"
date: 2012-07-06 12:00
author: Louis Chatriot
comments: true
categories: continuous deployment, server setup
---


As all team working on a web application, we had to setup a [continuous
integration](http://en.wikipedia.org/wiki/Continuous_integration) server. This is 
a fairly well-known engineering practice, but we wanted to take it one
step further, and use continuous deployment. As the name implies, this
means that any new code pushed to the server will automatically be
built, tested and then go live and be used by our users right away. It
turns out that is fairly easy to set up, except for a few caveats.

// PHOTO



---
layout: post
title: "Tutorial: Domain Name System for Beginners"
date: 2012-04-22 08:30
author: Louis Chatriot
comments: true
categories: [tutorial, internet]
---


If you build a website, you will want your own domain name, which means
you will need to manage your Domain Name System (DNS). While there is nothing very
complicated here, the info is scattered across the Internet, so I decided
to write a "cheat sheet"-like tutorial which will hopefully save you some time!  


[![Code](http://farm1.staticflickr.com/122/277341190_3f098a08a4_n.jpg)](http://www.flickr.com/photos/49502986585@N01/277341190/)
*by [Ezu](http://www.flickr.com/photos/ezu/ "Author")*  


## What is the DNS?
The DNS is a system that helps your computer translate human-readable **domain names**
into computer-usable **IP addresses** (4 numbers between 0 and 255
identifying a server).  

A domain name must have two parts, separated by a dot: the **actual domain** (e.g. *google*) and the
**top-level domain** (e.g. *com*). In this example, the domain name is
*google.com*. Optionally, you can also use different **subdomains**, so
that users typing *subdomain1.domain.tld* and *subdomain2.domain.tld*
arrive on two different pages. The well-known *www* is a subdomain of
*google.com* in the address *www.google.com*.  

To use your own custom domain name, you need
to buy one from a **registrar** (such as [Gandi](https://www.gandi.net/)).  


## How to manage it: the zone file
Most registrars will let you use Web forwardings, which are simple
redirections (what happens when the webpage you are looking at automatically changes to another page). However, you should
avoid those for the following reasons:

* Simple redirections will replace your domain name by your IP address,
  which is the best way to scare off users
* Masked redirections will not scare off users, but they will give you a
  very poor search-engine rating

You will need to use the **zone file**. It is a simple text file
describing the structure of your DNS zone (usually your domain) to the global DNS 
(i.e. the world). Let's take a look at a sample zone file (the zone file on 
Gandi for our blog [needforair.com](http://needforair.com).

These are the DNS rules for our domain `needforair.com`:

    @ 3600 IN A 184.106.20.102
    www 3600 IN CNAME needforair.com.
    wik 3600 IN CNAME fr.wikipedia.org.
    
You usually also find lines similar to the following in a zone file.
They manage the email server your registrar provides you, and shouldn't
touch them:

    @ 3600 IN MX 10 spool.mail.gandi.net.
    @ 3600 IN MX 50 fb.mail.gandi.net.
    smtp 3600 IN CNAME relay.mail.gandi.net.
    webmail 3600 IN CNAME agent.mail.gandi.net.
    pop 3600 IN CNAME access.mail.gandi.net.
    imap 3600 IN CNAME access.mail.gandi.net.

The most important points here are:

* Each line corresponds to one rule (a record)
* The `@` character represents your bare domain (needforair.com in our example) as opposed
to subdomains such as `test.needforair.com` or `www.needforair.com`
* An `A` record points to an IP address, usually your server. In our example, `@ 3600 IN A 184.106.20.102`
indicates that a user typing 'needforair.com' in his address bar will request content from the IP address 184.106.20.102. 
The `3600` number is the [Time To Live (TTL)](http://en.wikipedia.org/wiki/Time_to_live#DNS_records) of the record, 
the time (in seconds) during which a nameserver will cache the record and not ask the authoritative server (Gandi here) 
if queried again
* A `CNAME` record points to any domain or subdomain, either in your zone or outside. 
In our example, `www 3600 IN CNAME needforair.com.` means that a user typing `www.needforair.com` will request content 
from the same IP address as needforair.com (that's not the case by default). You can of course use a CNAME to any other 
domain, in our example `wik 3600 IN CNAME fr.wikipedia.org.` which will make users typing "wik.needforair.com" in their 
address bar go to the French version of Wikipedia instead (don't try it
we removed it!)
* **Don't forget the period ('.') at the end of your CNAMEs !!!** For example, use "needforair.com.", and NOT "needforair.com". 
If you forget the period, the record simply won't work, and you will
spend an awful lot of time to figure out why  

There are lots of other types of rules, but this should be enough to set up your DNS.  

Last note: keep in mind that **changes to the zone file take some time**,
usually a few hours, to propagate through the Internet. So don't make a
mistake or they will take some time to spot and correct!


## Discover more

* [A great primer on DNS for the non-technical](http://continuations.com/post/16405180072/tech-tuesday-dns)
* [Wikipedia's article on the Zone file](http://en.wikipedia.org/wiki/Zone_file)
* [Wikipedia's article on the DNS](http://en.wikipedia.org/wiki/Domain_Name_System). 

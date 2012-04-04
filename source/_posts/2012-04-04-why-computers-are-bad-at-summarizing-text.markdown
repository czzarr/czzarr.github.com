---
layout: post
title: "Why Computers are Still Bad at Summarizing Text"
date: 2012-04-04 08:30
author: Louis Chatriot
comments: true
categories: language, processing, computer, summarizing
---

As we explained in [this previous post](http://needforair.com/blog/2012/03/09/how-to-keep-up-with-the-information-overload/ "Information Overload"), we believe we are dealing with an information overload problem. The Internet produces so much information in one day that we need a lot of time (for me 2 to 3 hours) to go through all those information streams (blogs, information websites, twitter, aggregators and so on). This even more true for the tech community which traditionally produces (and consumes) more than the "general population".  

## Solving information overload through summarization
The natural solution is text summarization: if I had access to summaries of the resources I read, I reckon it would only take me 20 minutes to go through it. I would then have time to dive deeper in the most interesting articles, and I would have more time to code. Being a developper, I naturally asked myself if there was a way to sum text up algorithmically. It turns out that **computers are still bad at summarizing text**. I will explain why.  

## Keyphrase extraction
The simplest summary is one that consists of a few bullet points, the
keyphrases. It turns out computers are able to extract keyphrases from a
text, but not that well.

### Keyphrase extraction as supervised learning
One way to perform this task is described by Peter Turney in [this paper](http://webdocs.cs.ualberta.ca/~lindek/650/papers/turney.pdf). He found that using a custom-designed algorithm that incorporates specialized domain knowledge on a text generates yields fairly good results: 80% of keyphrases are deemed acceptable by human readers. The problem with this approach is that it requires the computer to know the field the text is about beforehand. It is possible to use a prior first step to determine what this field is, but this is difficult and the results are terrible if we get the field wrong.  

### Unsupervised keyphrase extraction 
This approaches enables us to extract keyphrases from a text without any
prior knowledge. The TextRank algorithm was developped by Rada Mihalcea and Paul Tarau in
[this paper](http://acl.ldc.upenn.edu/acl2004/emnlp/pdf/Mihalcea.pdf) to
perform this task, and uses Google's PageRank. It treats a text as a graph, where
the text units (words or phrases depending on your need) are the
vertices which are connected when they appear close to one another in
the text. Running PageRank on this graph ... TODO

## Abstract summarizing
Keyphrase is "copy paste", no deep understanding of text -> need
abstract understanding for real summaries (humans).




ABSTRACT







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

## Finding the mpst relevant content: keyphrase extraction
The simplest summary is one that consists of a few bullet points, the
keyphrases. It turns out computers are able to extract keyphrases from a
text, but not that well.

### When you know the context: extraction as supervised learning
One way to perform this task is described by Peter Turney in [this paper](http://webdocs.cs.ualberta.ca/~lindek/650/papers/turney.pdf). He found that using a custom-designed algorithm that incorporates specialized domain knowledge on a text generates yields fairly good results: 80% of keyphrases are deemed acceptable by human readers. At the time the paper was written, this was the highest success-rate ever observed. The problem with this approach is that it requires the computer to know the field the text is about beforehand. It is possible to use a prior first step to determine what this field is, but this is difficult and the results are terrible if we get the field wrong.  

### Applicable to any text: unsupervised keyphrase extraction
This approaches enables us to extract keyphrases from a text without any
prior knowledge, which is a tremendous improvement upon the previous method. The TextRank algorithm, developped by Rada Mihalcea and Paul Tarau in
[this paper](http://acl.ldc.upenn.edu/acl2004/emnlp/pdf/Mihalcea.pdf),
performs this task using Google's PageRank. The key idea is that a text
unit (word or phrase depending on the need) that is often referred to by
other text units is likely a keyphrase. Here, "*a* refers to *b*" means
"*a* and *b* are close within the text". TextRank transforms the text in
a graph where text units (equivalent to webpages in PageRank) are the
vertices and there is an edge between two text units when they are close
to one another (equivalent to hyperlinks in PageRank). It works fairly well, its main author was even rewarded by Google for her work.  


## The best: abstract summarizing
Keyphrase extraction, though useful, is still far from actual summarization.
There is a notable qualitative difference between a list of phrases
directly extracted from a text and a coherent summary. Moreover,
keyphrase extraction doesn't work well with complicated or technical
articles, which typically use several different text units to talk about
the same thing. What we actually need is simply a way to produce summaries as
if they had been written by a human.  
To that algorithmically, we need two things:  

* A way to understand the meaning and most important
  points of a text
* A way to use this understanding to write the summary

### Understanding what a text means is hard
Although Natual Language Processing is progressing, computers still have
a real hard time to understand what a text means. In particular, they
have trouble with:  

* **Coreferences**. For example, who does 'he'
  references in the sentence 'Paul told John he should play squash'?
* **Word sense disambiguation**. In 'I need batteries for
  my mouse', are we talking about the animal or the device?
* **Sentiment analysis**. Understanding the mood of the writer is often
  key to understanding a text, all the more when there is sarcasm.


### Writing "intelligent text" is not easy either
When you know the context, and provide the understanding of a text to a computer
in a form it can understand, it is possible to write fairly well. [This start-up](http://www.yseop.com/EN/home.html), for example, does a nice job. But summaries written this way are still far from their human-produced counterparts, and as I explained, we still lack a way to make a computer understand a text!


## Computers can still help humans
So, computers are still bad at summarizing text. An algorithmically
produced summary can't be compared to its counterpart written by a
human. But that doesn't mean computers have no role to play. First,
there is still a lot of research going on, and a breakthrough may occur
one day. Second, keyphrase extraction could be used to produce a
"pre-summary" which would save time to a human writing the actual
summary. We are working on these kind of ideas currently!  


*Note: some examples were taken from [Stanford's Natural Language
Processing online class](https://class.coursera.org/nlp/). If you couldn't take
it this quarter, look for it, it is a terrific place to learn about this
topic*





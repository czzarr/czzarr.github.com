---
layout: post
title: "The Role of Computers in Text Summarization"
date: 2012-04-06 08:30
author: Louis Chatriot
comments: true
categories: [tldr, NLP, algorithmy,  summary]
---

As Stanislas explained in [this previous post](http://needforair.com/blog/2012/03/09/how-to-keep-up-with-the-information-overload/ "Information Overload"), we believe we are dealing with too much information to keep up on the Internet. The natural solution is text summarization, and computers can help us there.

[![Summary](http://farm4.staticflickr.com/3376/3558255554_9f9c480132_n.jpg)](http://www.flickr.com/photos/xeeliz/3558255554/sizes/l/in/photostream/)
*by [Xeeiliz](http://www.flickr.com/photos/xeeliz/ "Author")*  

There are roughly two kinds of summaries: the ones that consist of a few of
the most important sentences extracted from the text, the keyphrases, and the ones that
are written from scratch without necessarily using the same phrasing, called abstract summaries.


## Finding the most relevant content: keyphrase extraction
The big advantage of keyphrase-based summaries is that there is no risk
of miscomprehension since they are composed of unrephrased pieces of the
text. Moreover, computers are able to compute them and get decent
results. There are two main ways to do it: supervised and unsupervised
extraction.


### When you know the context: extraction as supervised learning
One way to perform this task is described by Peter Turney in [this paper](http://webdocs.cs.ualberta.ca/~lindek/650/papers/turney.pdf). He found that using a custom-designed algorithm that incorporates specialized domain knowledge on a text generates yields very good results: 80% of keyphrases are deemed acceptable by human readers. The problem with this approach is that it requires the computer to know the topic beforehand. It is possible to use a prior first step to determine what it is, but this is difficult and the results are terrible if we get it wrong.  

### Applicable to any text: unsupervised keyphrase extraction
This approaches enables us to extract keyphrases from a text without any
prior knowledge, which is a tremendous improvement upon the previous method. There a several algorithms, and one notable current example is [Summly](http://www.summly.com/). I will explain one of those, named TextRank, to give you a feeling of the kind of techniques used in this field.  

The TextRank algorithm was developped by Rada Mihalcea and Paul Tarau in
[this paper](http://acl.ldc.upenn.edu/acl2004/emnlp/pdf/Mihalcea.pdf). The key idea is that a text
unit (word or phrase depending on the need) that is close to a lot of other important text units is likely a keyphrase. Using the parallel between the "close to" relation and the "referred to by" relation of hyperlinks, TextRank uses Google's [PageRank](http://en.wikipedia.org/wiki/PageRank) to determine which text units are the most important. It works well, its main author was even awarded a $100k grant by Google for her work on keyphrase extraction.  


## The best: abstract summarization
Even though they can be very useful, keyphrase-based summaries are still
rudimentary compared to abstract summaries, because they are much nicer
to read, and we are sure no important information was "left behind" (i.e a keyphrase was not extracted). Also, keyphrase extraction algorithms still find it hard to
summarize technical articles.  

Abstract summarization is the limit of what computers can do today. The
main reason for this is that they struggle with the following tasks:   

* **Coreferences understanding**. For example, who does 'he'
  references in the sentence 'Paul told John he should play squash'?
* **Word sense disambiguation**. In 'I need batteries for
  my mouse', are we talking about the animal or the device?
* **Sentiment analysis**. Understanding the mood of the writer is often
  key to understanding a text, all the more when there is sarcasm.


## Computers can help humans summarize a text
To sum it up, I believe that humans still cannot be replaced by
algorithms if we want to produce quality summaries. Nonetheless, a
computer really can help a us by providing a keyphrase-based summary,
so that we spend less time scanning the text for them. In addition, if
we were able to train algorithms by comparing their output with a human
summary a lot of times for articles on a certain topic, I think it is possible we see significant improvements.  


*Note: some examples were taken from [Stanford's Natural Language
Processing online class](https://class.coursera.org/nlp/). If you couldn't take
it this quarter, look for it, it is a terrific place to learn about this
topic*





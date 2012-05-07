---
layout: post
title: "Behind Opinion polls"
date: 2012-04-29 20:12
comments: true
author: Charles Miglietti
categories: 
---

We are in an election time in France. Everyday news is rythmed by
polls. _Nicolas Sarkozy is falling from 2% in today's poll from IPSOS_,
_Francois Hollande slipped below 33% after the crisis_ etc... Everybody
seems to give a lot of credit to these polls forgeting most of the time
that they are just statistical estimations of the reality. But what is their realibility ?  

## Organize the Opinion Poll

Opinion polls tend to estimate the percentage of people who will vote
for a given candidate. Polling Institutes want to produce accurate polls
but they can't poll a too big sample as it would be time consuming and
expensive.

They choose a [representative sample](http://en.wikipedia.org/wiki/Sampling_(statistics\)) 
of the population and ask them for whom they plan to vote. The responses
of the polled people are assumed [independant](http://en.wikipedia.org/wiki/Independence_(probability_theory\)).

Once collected, how do we build an estimate of the votes and what is the
precision of this estimate? 

The _natural and logical_ estimate is to take the 
[sample mean](http://en.wikipedia.org/wiki/Sample_mean) as an estimator. How do we
justify such an estimate and what is its precision? We'll see that what
sounds common sense has strong mathematics justification.


## Building the Estimate

We are in a situation where we have _n_ people 

<div markdown="0">
\[ X_1, X_2,...X_n\]
</div>

who can choose between _k_ candidates. We define the variable

<div markdown="0">
\[ \delta_{i,j} \]
</div>

which is equal to _1_ if person _i_ votes for candidate
_j_ and _0_ else.

The real vote intentions we want
to estimate are noted :

<div markdown="0">
\[ \pi_1, \pi_2,...\pi_k\]
</div>

with the constraint:
<div markdown="0">
\[ \sum_j^k \pi_j = 1 \]
</div>

The probability to observe the result of the polling is:

<div markdown="0">
\[ p_{\pi}(X_1, X_2,...,X_n) = \prod_i^n p_{\pi} (X_i)\]
</div>

as the _n_ people are chosen independantly.

<div markdown="0">
\[ \prod_i^n p_{\pi} (X_i) = \prod_j^k \prod_i^n \pi_j^{\delta_{i,j}} =
  \prod_j^k \pi_j^{n_j}\]
</div>

where 

<div markdown="0">
\[ n_j = \sum_i^n \delta_{i,j}\]
</div>


The strategy we adopt is, given the _n_ people, to maximize this
propability of observing the output. Maximizing this propability is the
same as minimizing the _-log_ of the probability as _-log_ is a convex
function.


<div markdown="0">
\[ max[p_{\pi}(X_1, X_2,...,X_n)] = min[(-log(p_{\pi}(X_1,
        X_2,...,X_n))] = min[ \sum_j^k n_j log(\pi_j) ]\]
</div>

To find the minimum with the above constraint we can use the 
[Lagrangian
multiplier](http://en.wikipedia.org/wiki/Lagrange_multiplier) and just
minimize:

<div markdown="0">
\[ \sum_j^k n_j log(\pi_j) + \lambda(\sum_j^k \pi_j -1)\]
</div>

which gives us the estimates which minimize the above function

<div markdown="0">
\[ \hat{\pi_1}, \hat{\pi_2},...\hat{\pi_k}\]
</div>

<div markdown="0">
\[ \hat{\pi_j} = \frac{n_j}{n} \]
</div>

We just justified the use of the empirical mean as an estimate of the true
mean given a set of observations using [maximum a posteriori
estimation](http://en.wikipedia.org/wiki/Maximum_a_posteriori_estimation)


## Reliability



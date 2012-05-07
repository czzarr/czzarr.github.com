---
layout: post
title: "Behind Opinion polls"
date: 2012-05-10 20:12
comments: true
author: Charles Miglietti
categories: [math]
---

We are in an election time in France. Everyday news is rythmed by
polls. _Nicolas Sarkozy is falling from 2% in today's poll from IPSOS_,
_Francois Hollande slipped below 33% after the crisis_ etc... Everybody
seems to give a lot of credit to these polls forgeting most of the time
that they are just statistical estimations of the reality. But what is their realibility ?  

## Organizing the Opinion Poll

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
function. With this transformation, the computation of the extremum is
much easier as the product becomes a sum.


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

Given the estimator, which level of trust can we give it? How near is
the estimated value of the true one. The [law of large numbers](http://en.wikipedia.org/wiki/Law_of_large_numbers) 
tells us that when the size of the sample goes to infinity the estimated
value tends to the true value. But in reality there is not an infinity
of people who get polled. Usually just a few thousands are. From one
sample to the other the estimator won't be the same. So how confident are the polling
institutes in their estimations? The reliability of the estimator
depends on the statistical fluctuations of the estimate around the true
value.


The [central limit theorem](http://en.wikipedia.org/wiki/Central_limit_theorem) indicates us that 
the random variables (_m_ is the true mean and _sigma_ the variance): 
<div markdown="0">
\[ \frac{\hat{\pi_j} -nm}{\sigma \sqrt{n}} \] 
</div>

[converge in distribution](http://en.wikipedia.org/wiki/Convergence_in_distribution#Convergence_in_distribution)
to a normal random variable with mean 0 and variance 1. From here we
have the approximation: 


<div markdown="0">
\[ P(|\hat{\pi_j} - \pi_j| \leq \epsilon) = P(\frac{\sqrt{n}|\hat{\pi_j} - \pi_j|}{\sqrt{p(1-p)}} \leq \frac{\sqrt{n}\epsilon}{\sqrt{p(1-p)}}) \approx \int_{-a}^{a} g(x)dx\]
</div>

where _g_ is the density of the normal distribution and 

<div markdown="0">
\[ a = \frac{\sqrt{n}\epsilon}{\sqrt{p(1-p)}} \]
</div>

from which we can write 
<div markdown="0">
\[ \epsilon = \frac{a\sqrt{p(1-p)}}{\sqrt{n}} \]
</div>

So given a threshold of confidence _t_  we can build a interval of
confidence where we can state with _t%_ of confidence that the true
value is within this interval:

<div markdown="0">
\[ I_t = [\hat{\pi}_{j,n} - \frac{a_t}{2\sqrt{n}}, \hat{\pi}_{j,n} + \frac{a_t}{2\sqrt{n}}] \]
</div>

where 

<div markdown="0">
\[ \int_{-a_t}^{a_t} g(x)dx = t\]
</div>

The value for _a_ can be found in a computation table. For _t_ equal to
_95%_ the value of _a_ is _1.96_ which gives us for the interval where there
is _95%_ of chance that the true value is:

<div markdown="0">
\[ I_t = [\hat{\pi}_{j,n} - \frac{1}{\sqrt{n}}, \hat{\pi}_{j,n} + \frac{1}{\sqrt{n}}] \]
</div>

A quick numeric application gives us the result that if we have an
opinion poll realized on 1020 people which gives _33%_ as an estimation
for the vote intention for one candidate there is _95%_ of chance that the
true vote intention is between _30%_ and _36%_.

This quick reminder on opinion polls should remind us that opinion polls
can be quite far for the reality. And there is one variable that I
didn't take into account: there are people who don't dare
the name the true candidate they plan to vote for. This is especially
true for extremist votes and explains [why we can experience such a
discrepancy between the estimations and the true results of one
election](http://www.guardian.co.uk/world/2012/apr/22/marine-le-pen-french-election).





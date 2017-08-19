---
author: tr8dr
comments: true
date: 2009-12-18 02:23:12+00:00
layout: post
link: https://tr8dr.wordpress.com/?p=390
published: false
slug: portfolio-followups
title: Portfolio Followups
wordpress_id: 390
categories:
- strategies
---

**Pattern Based**
Finding a prior expert based on pattern and blending based on the performance of the weights used in the forward period may have merit.   We can chose weights, in fact that are both positive and negative in such a scenario.   In addition to various degrees of fuzziness in matching, we may also want to consider matching on the sequence of cumulative returns over the past period.

In the simplest case the symbols for returns X are:

    
       S(X[t-1]), S(X[t-2]), S(X[t-3]), ...


The other case is to perform some smoothing, as in:

    
       S(cumsum(X[t-1:t-k]), S(cumsum(X[t-k+1:t-2k])), S(cumsum(X[t-2k+1:t-3k])), ...


This will be less noisey and be more representative of the patterns we want to find.

**Mean-Reversion**
The ANTICOR portfolio did much better than the CRP in some cases, but worse in others.   Perhaps there was some additional innovation done by the authors not present in the paper.

There are a number of improvements that could be done, but ultimately this is a long-only algorithm.   One such improvement, though would be to estimate the relative gain of CLAIMij using the difference between expected returns:

    
      E[Xi | i->j] - E[Xi | i->i]


as a function of volatilities and correlations.

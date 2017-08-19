---
author: tr8dr
comments: true
date: 2009-11-28 02:04:27+00:00
layout: post
link: https://tr8dr.wordpress.com/?p=327
published: false
slug: variance-estimation-mean-process
title: Variance Estimation & Mean Process
wordpress_id: 327
categories:
- volatility
---

**Duration Process Estimation**
Instead of using the rather primitive ACD models, use one of the learning regression models such as KRLS to model durations over time.   Provide as input:



	
  1. prior N durations (lagged)

	
  2. realized variance up to prior observation

	
  3. actual duration


KRLS will provide a prediction.  This can be converted into a probability using a counting distribution, Weibull, or other distributions.

The expectation can then be evaluated to give us a local volatility measure.

**Mean Process**
The generalized gaussian markov process is key to our mean estimation.   With a good estimate of the variance process we can generate an appropriate corridor for the mean-reversion component.   We can compose with:



	
  1. a process for variance σ(t)
See above and older posts for details

	
  2. a mean-reverting process κ(t) for the MR coefficient
dκ(t) = γ(κ∝ - κ(t))dt + ε

	
  3. an AR process for the mean
μ[t] = α μ(t-1) + βε[t-1] +ε[t] or a general ARMA(p,q)


We first need to develop the KRLS regressor.

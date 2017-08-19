---
author: tr8dr
comments: true
date: 2009-11-14 20:04:42+00:00
layout: post
link: https://tr8dr.wordpress.com/?p=227
published: false
slug: mean-reversion-sde-revisited
title: Mean Reversion SDE revisited
wordpress_id: 227
categories:
- strategies
---

I've spent a bit of time in the past few posts thinking about the "mean" in the context of mean reversion, vacillating between stochastic models and projections based on the decomposition of the a-posteriori price series.   There is no question in my mind that probabilistic models are the way to go, the question has been more about how to formulate in a way that allows for explicit control of the level of MR I'm looking to observe.

**Filtering Approach**
One thing that came to mind was, supposing we extended the Ornstein-Uhlenbeck process as:

![Picture 5](http://tr8dr.files.wordpress.com/2009/11/picture-5.png)

Such that σ(t) is a heteroscedastic process with reversion to some mean stdev (our average level of innovation), and κ(t), likewise allows for a non-constant degree of mean-reversion but also reverts to its mean (our desired average level of MR).   We could also impose that μ(t) is an AR(p) process of some sort.   Note that process describing σ(t) is one of the most complex topics in financial engineering;  I will probably not be implementing a duration based variance estimate for this, but may do so in the future.

Discretizing this, we could use a particle filer to estimate the parameter constants of the MR, volatility process, and mean process by optimizing for maximum likelihood.    Parameterizing the volatility mean and innovation (error) would allow us to specify the "force" pulling the price process away from the mean.    This will be quite expensive to calibrate, but certainly doable.

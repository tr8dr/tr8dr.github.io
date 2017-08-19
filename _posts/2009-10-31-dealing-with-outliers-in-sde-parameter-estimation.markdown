---
author: tr8dr
comments: true
date: 2009-10-31 15:20:17+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/10/31/dealing-with-outliers-in-sde-parameter-estimation/
slug: dealing-with-outliers-in-sde-parameter-estimation
title: Dealing with Outliers in a markovian SDE
wordpress_id: 56
categories:
- state-space-models
- statistics
- stochatistic
---

I was developing a state model similar to the ADS (_Arouba_-_Diebold_-_Scotti_ index) to provide a daily indication of economic cycle for the Canadian market.   Economic data sourced from news can be particularly noisy (ie dataentry errors, etc).   I had encountered this in the state system:

![Picture 3](../files/2009/10/picture-31.png)

The instability was due to bad data points in the series.  Cleaning the bad data solved the problem.  However, detecting unintended outliers at runtime is especially important for real-time strategies.

A markovian state system, where each state depends on the previous, is particularly sensitive to data issues.   Such a system is typically set up as:


![Picture 1](http://tr8dr.files.wordpress.com/2009/10/picture-12.png?w=300)


In particle filtering or kalman filtering one is estimating hidden state based on observations and using the hidden state to make statements about the market.   An outlier is such that:


![Picture 4](http://tr8dr.files.wordpress.com/2009/10/picture-4.png?w=150)


In such a situation a kalman filtered state system will collapse and one will get a huge spike in the state system as an oscillating function as it attempts to minimize the disturbance in subsequent iterations.   Assuming the "outlier" is in fact correct data, our state system suffers from one of a number of possible problems:



	
  1. Distribution does not have proper tails

	
  2. Covariance of innovation is too small, causing system to be numerically unstable in the presence of near zero probability observation.


There are approaches for dealing with this such as adopting a weighted least squares approach or bayesian weighting of observations (see [Learning an Outlier -Robust Kalman Filter](http://www.google.com/url?sa=t&source=web&ct=res&cd=1&ved=0CA0QFjAA&url=http%3A%2F%2Fwww-clmc.usc.edu%2Fpublications%2F%2FT%2FTR-CLMC-2007-1.pdf&ei=rF7sSqvuI4eslAf7rdX_BA&usg=AFQjCNEVtP3NqiOxmV4FbciooUsMFR8UnQ&sig2=heofR9OhJFqzTDVtOgj-Jg)).

In our case, however, the outliers are data errors.   For non-realtime systems, the data can be cleaned.  In the case of a real-time strategy we don't have the option of manually cleaning data, so we may need to resort to a heuristic to determine whether the data is an "outlier" or not.

One heuristic approach is to determine:


![Picture 5](http://tr8dr.files.wordpress.com/2009/10/picture-5.png?w=150)


and then evaluate the probability of this observation Yt given Xt.   This can be accomplished by evaluating the pdf on the residual of the projected versus observed:


![Picture 6](http://tr8dr.files.wordpress.com/2009/10/picture-6.png?w=150)


If the probability is 0 or near 0 then the data is clearly an outlier.  The estimation of the expected value of Xt can be determined by estimating the distribution of state.

How does one evaluate E(X[t] | X[t-1])?   A simple (though expensive) way to evaluate this is to draw noise samples from the innovation (error) distribution, evaluate Fx(X[t-1],) + Ex, and use the robust mean of those samples weighted by the probability of the draw.   The number of samples needed can be reduced by using a kernel estimator when determining the expectation.

Finally if a outlier is detected, we can treat it as a missing value.  Information has been lost, but at least we have preserved the stability of our state system.   This is a reasonable approach, I think, for the occassional outlier.

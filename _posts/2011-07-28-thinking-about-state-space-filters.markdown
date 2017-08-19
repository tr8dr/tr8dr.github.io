---
author: tr8dr
comments: true
date: 2011-07-28 01:09:06+00:00
layout: post
link: https://tr8dr.wordpress.com/2011/07/27/thinking-about-state-space-filters/
slug: thinking-about-state-space-filters
title: Thinking About State Space Filters
wordpress_id: 889
categories:
- strategies
---

I have not used stochastic state based systems for a couple of years, but have decided to revisit.   I had previously implemented a number of systems with both the EKF and 3 variants of particle filter, but encountered various issues.

In particular, found the parameterization of the evolution via the covariance matrices to be opaque and problematic for some systems.    Also both the EKF and particle filters were subject to numerical instability if the mean of the state distribution or observed samples shifted significantly with likelihoods approaching 0.

I've decided to revisit state space filters but with a different filtering approach, for the purposes of coming up with a different approach to multi-regime pricing.

I'll start with an introduction.  The traditional state space model is a discrete (or discretized continuous) system represented by a measurement equation and state evolution equation:


[![](http://tr8dr.files.wordpress.com/2011/07/screen-shot-2011-07-27-at-7-01-57-pm.png)](http://tr8dr.files.wordpress.com/2011/07/screen-shot-2011-07-27-at-7-01-57-pm.png)


Where:



	
  1. X is a n dimensional vector representing the state of the system on timestep t

	
  2. Y is a m dimensional vector  representing the measurement on timestep t (for example a price in our price series)

	
  3. f(.) is our state evolution function mapping the t-1 th state to the t th state

	
  4. g(.) is our state to measurement mapping

	
  5. ε_x our state noise / error ~ N (0, Σx)

	
  6. ε_y our observation noise / error ~ N(0,Σy)


**Kalman Family of Filters
**The kalman family of filters uses a bayesian logic to implement a linear error correction model, such that errors in the estimation of Yt propagate back to the evolving state Xt (or more precisely, an evolving distribution of Xt).

The traditional Kalman filter assumes a linear function f(.) and g(.), usually expressed as matrices:


[![](http://tr8dr.files.wordpress.com/2011/07/screen-shot-2011-07-27-at-7-30-55-pm.png)](http://tr8dr.files.wordpress.com/2011/07/screen-shot-2011-07-27-at-7-30-55-pm.png)


We are interested in estimating the state Xt given the noisy observed data Yt, hence are interested in the pdf **p(x[t] | y[1:t])**.  Given that we will have a lagged (t-1) view on this pdf via the recursion inherent in filtering, we first need to be able to compute **p(x[t] | y[1:t-1])** and then using that **p(x[t] | y[1:t])**:


[![](http://tr8dr.files.wordpress.com/2011/07/screen-shot-2011-07-27-at-8-28-48-pm.png)](http://tr8dr.files.wordpress.com/2011/07/screen-shot-2011-07-27-at-8-28-48-pm.png)


Hence as pointed out in (Hartikainen 2008) the above linear state equations map to distributions:


[![](http://tr8dr.files.wordpress.com/2011/07/screen-shot-2011-07-27-at-8-37-51-pm.png)](http://tr8dr.files.wordpress.com/2011/07/screen-shot-2011-07-27-at-8-37-51-pm.png)


I won't go into the math for the Kalman filter, as would like to discuss its issues and move on to more sophisticated approaches.  The 3 main issues I have with the Kalman Filter:



	
  1. can only express the evolution of linear state functions

	
  2. hard to calibrate degree of fit via state and measurement noise covariance matrices

	
  3. can become numerically unstable with unexpected noise (probabilities go to 0 or may oscillate wildly)


The **Extended Kalman Filter** partially solves one of these issues (that of non-linear state functions) by adjusting the kalman distribution estimation to use 1 or 2 terms of the taylor series expansion of the state and observation functions.   This works well for functions completely described by 1st and 2nd derivatives, although has similar drawbacks in terms of calibration and stability.

Short of using a numerically expensive **particle filter**, it seems that a variant the **Unscented Kalman Filter** (UKF) presents the best choice for the potential state systems I will be using.    The UKF using a deterministic sampling approach mapped through the non-linear functions, provides multiple moments for the underlying distribution.   The distribution can then be described with greater accuracy than afforded through the EKF's taylor expansion.

**Topics to be Discussed**:






	
  1. Unscented Transform

	
  2. Smoothing

	
  3. Need for a Multiple Model approach with Markovian switching

	
  4. Filter approach for multiple models

	
  5. Possible Models




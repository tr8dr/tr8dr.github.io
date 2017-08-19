---
author: tr8dr
comments: true
date: 2010-02-08 01:11:23+00:00
layout: post
link: https://tr8dr.wordpress.com/2010/02/07/adaptive-regressor/
slug: adaptive-regressor
title: Adaptive Regressor
wordpress_id: 590
categories:
- strategies
---

Regression is an important tool in trading (witness the number of traders that rely on moving averages of various sorts).    I don't directly use regressors to generate trading signals, but I do find them useful in denoising signal output.

Aside from the obvious about past predicting the future, there are other issues with regressors:



	
  1. lag: denoising necessarily involves averaging of some sort, resulting in lag relative to the underlier

	
  2. parameterization:  what parameter settings bring out the features of interest


The simplest regressors are ARMA based FIR or IIR filters.   Lag is easy to quantify as phase delay in those systems and harder in others.   Rather than focusing on lag, I want to consider the parameterization.

**Parameterization**
To illustrate the problem of parameterization, consider a simple exponential MA in two market scenarios:



	
  1. **market with strong trends**
Long windows mask tradeable market movements.   A shorter window (or "tau") is needed to capture market movements of interest.

	
  2. **market trading sideways**
Short windowed MA oscillates on small movements.   Long window needed to reduce or eliminate noise that is not tradeable.


While I don't use MAs for trade entry, the general problem of adapting a regressor to features of interest is important.

**Penalized Least Squares**
The penalized least-squares spline is known to be  the "best linear unbiased predictor"  for series that can be modeled by:


[![](http://tr8dr.files.wordpress.com/2010/02/fa.png)](http://tr8dr.files.wordpress.com/2010/02/fa.png)


Where, f(x) is typically a polynomial based function (typically a high dimensional basis function).   Characteristic of the penalized family of splines is the balance between least-squares fit and curvature penalty:


[![](http://tr8dr.files.wordpress.com/2010/02/fb.png)](http://tr8dr.files.wordpress.com/2010/02/fb.png)


This minimization can be constructed into a matrix based system using the basis design matrix.  I'm not going to go into this here, but you can find many papers on this.  The formulation is straightforward, but it is very easy to run into numerical instabilities with straightforward solutions (trust me I've tried), so best bet is to use one of the tried and tested implementations (such as DeBoor's).

Ok, the problem with the above is that the parameter λ is a free variable (i.e. an input into the minimization).   λ allows us to control the degree of curvature or oscillating behavior.   Here is the same series with 4 different levels of λ (underlier in black):

[![](http://tr8dr.files.wordpress.com/2010/02/demo.png)](http://tr8dr.files.wordpress.com/2010/02/demo.png)

Flexibility is great.  Now how do I choose λ appropriately?   And how do I define appropriate?

**Criteria**
As mentioned above, with the incorrect choice of regression parameters result in  regressor that is either too noisy or misses features.

Now before I explain the criteria (heuristics really) that I came up with, let me point to some literature tackling the general concept.   Tatyana Krivobokova, Ciprian M. Crainiceanu, and Goran Kauermann, "[Fast Adaptive Penalized Splines](http://www.bepress.com/jhubiostat/paper100/)" (2007).   Their approach produces an evolving λ, one for each of the truncated basis functions through time, chosen such as to reduce the local error, but keeping enough error to be optimally cross-validated.

Though the above is interesting, and indeed produces some amazing results for certain data sets, the "smoothness criteria" are fundamentally different from what I am looking for.

I decided that my criteria is as follows:



	
  1. the amplitudes between min/maxima in the spline must meet some minimum amplitude-time

	
  2. the energy of the spline must be "close" to the energy of the original series


The rationale for the 1st point is that we do not want small oscillations in the spline (signifying that we need to tune for less noise).   The second point tunes in the other direction, that is, if the spline is too stiff, missing many features, the energy of the spline will be too low relative to the original series.

**Algorithm**
The two above criteria break down into:



	
  1. the integral between a maximum and minimum ≥ threshold

	
  2. the integral of f(x)^2, where f(x) is the spline


As I did not see an easy way of building into a system of equations took the "poor mans algorithm" approach, namely:

	
  1. binary-style search between low and high values for λ

	
  2. if amplitude/area < threshold choose higher lambda else lower

	
  3. repeat until some granularity


Works well!

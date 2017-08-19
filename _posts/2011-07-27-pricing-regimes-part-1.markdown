---
author: tr8dr
comments: true
date: 2011-07-27 21:24:44+00:00
layout: post
link: https://tr8dr.wordpress.com/2011/07/27/pricing-regimes-part-1/
slug: pricing-regimes-part-1
title: Pricing & Regime (part 1)
wordpress_id: 873
categories:
- strategies
---

High frequency trading tends to fall into the following categories:



	
  1. some form of prop market making (probably largest %)

	
  2. gaming other players or microstructure

	
  3. short-period arbitrage

	
  4. execution algos




One might also run medium frequency strategies that pursue:





	
  1. trend (if there is sufficient momentum)

	
  2. cycles (follow the pivots for high amplitude price cycles)

	
  3. longer-period arbitrage




Focusing on market making and trend/cycle following, understanding price regime (the gross characteristics of price movement) and pricing function (a view on expected forward price) is important.


**


**Regime**




We might like to have an indication of regime to determine whether we should be market making, following a trend, following cycles, etc.


  



In particular, with market making, there will be periods in the market where market making is "dangerous" (i.e. is almost guaranteed to lose money).   Aside from the obvious danger in gapping prices around news or other events, the main danger is in offering during periods of strong momentum.


  



There will be a strong order selection bias such that when the market is going up (down) you receive buying (selling) orderflow almost exclusively, and end up with short inventory against an appreciating (depreciating) price.


  



Hence, there will be periods during a trading day that are "good" for market making (i.e. when there is balanced order flow and very gradual price drift) and periods which are better suited for momentum or high-amplitude cycle following strategies.





  



**Pricing**




Pricing provides us with an expected mean and its derivatives over some forward period, as well as an estimate of noise and MR activity.   This information is invaluable in determining appropriate market making offering prices and in following high amplitude price activity.


  



**Goals**




We would like to have the following:








	
  1. A pricing function with (often) good accuracy over some forward period, tailored to the requirements of the regime

	
  2. A notion of price regime

	
  3. Other metrics such as upper and lower noise around the price function (mean), period and amplitudes of monotonic segments, etc.




In the context of market making we are looking to have a monotonic and almost linear price function that represents a mean or a mode through price noise.   Calibrated in this fashion one has a convenient view on the forward price function and the upper and lower bounds on prices we are willing to offer on.   This combined with inventory can be used to dynamically bias the offerings.


  



In the context of momentum or cycling we are looking for a pricing function that follows the momentum or cycles with appropriate curvature and is reasonably predictive (or retrospective with low-lag) with respect to pivot points.


  



**Example**




Here is an example of a pricing function (the green line) during a momentum period (over the course of some hours), encountering a disturbance and change in regime to a cycling mode thereafter:


  



 [![](http://tr8dr.files.wordpress.com/2011/07/screen-shot-2011-07-27-at-2-39-48-pm1.png)](http://tr8dr.files.wordpress.com/2011/07/screen-shot-2011-07-27-at-2-39-48-pm1.png)


  



Here is the continuation within the series with the pricing function in a momentum / cycling mode:




[![](http://tr8dr.files.wordpress.com/2011/07/screen-shot-2011-07-27-at-4-36-10-pm.png)](http://tr8dr.files.wordpress.com/2011/07/screen-shot-2011-07-27-at-4-36-10-pm.png)


  



Note that the above is smoothed a-posteri, so the trajectory of the filter has more noise as it progresses tick to tick.


  



**History Of Approaches**




I've used many approaches in the past in modeling the price function:








	
  1. variety of filters from the signal processing space

	
  2. signal decompositions and partial reconstitutions

	
  3. a number of different stochastic state systems with an EKF (extended kalman filter)

	
  4. direct calibration of least squares splines with various heuristic rules




My current approach is the #4, but would like to find a good solution using a SDE in a bayesian framework, as is more elegant and flexible.    My past experience with EKFs was that they are hard to parameterize (via the state and measurement covariances) and can easily become unstable in the presence of unexpected noise.





  



A few months ago Max Dama posted a link to a [paper](http://arxiv.org/abs/0802.0220) describing a TV-VAR model, but more of interest, an alternate approach to describing the kalman filter evolution rate with discount factors.   The discounting approach solves one of the problems in parameterizing Kalman filters.


  



That and in discussion with some other quants (hi Sasha) recently motivated me to reexamine the SDE approach.   I'll be discussing an approach in some detail in the next posts.

---
author: tr8dr
comments: true
date: 2007-12-09 14:35:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2007/12/09/price-path-probability/
slug: price-path-probability
title: Price Path Probability
wordpress_id: 16
categories:
- prediction
- probability
- statistics
---

What is the probable path of a security over the next 1 second, 5 seconds, 30 seconds?  
  
I attended a quantitative algorithmic trading seminar 3 weeks ago where one presenter was discussing fill probability (in general terms).   The presenter claimed that their model predicts the price path over the next few minutes to determine how best to read a VWAP strategy.  
  
While I don't believe it is possible to predict a specific price path, it is possible to determine the probability of any given price path.   If we can determine the probability of any given path through time from the current price to some final price in N seconds or minutes, we can compute the expected probability of going through a price level within some period of time.  
  
The expected probability through a node at time Tn at price level Pa on a multinomial tree will simply be the sum of the probability of all sub-paths from Ts to Tn going through Pa.   That part may be simple, but accurately determining the likely paths / probabilities is a hard research problem.  
  
Given that the number of paths is exponential with time, the farther out we look the more time it takes to compute a precise expectation.  We must use a monte carlo analysis, sampling a calibrated timeseries equation, to approximate the expectation function.  
  
Determining the timeseries function that accurately reflects the market is a very hard research problem.  Alas, if I told you my approach would have to kill you ;)

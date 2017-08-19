---
author: tr8dr
comments: true
date: 2009-07-31 20:47:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/07/31/price-path-probability-again/
slug: price-path-probability-again
title: Price Path Probability (Again)
wordpress_id: 27
categories:
- prediction
- probability
- statistics
---

So I completed a model and calibration which determines the probability of a price going through a level within a given time period.   If one can arrive at a high confidence level, this is incredibly useful for multi-leg execution and as a prop strategy in its own right.  
  
The model uses a SDE with mean reversion, a trend component, and an evolving distribution to determine the price across time.   The SDE is evaluated as a monte carlo simulation on a grid.  We determine the conditional probability of going from one price level to the next for a given (small) time interval.   The sum of the product of the probabilities along the price path represents the posterior probability of being at the given price node at a given time.  
  
With the grid in hand, one can query the grid to determine the probability of a price being above or below a level within a given time, etc.   For some markets, we are seeing a 75% confidence level, meaning we are right 3/4 of the time.   There were some markets where there was no distinct edge in the approach, I have ideas on how to adjust this, but have not had the time to revisit.   
  
The evolution of the distribution was the most complex to model.  Unlike idealized option models, where the distribution is stationary and generally gaussian, the observed intra-day distribution over short periods is neither gaussian nor stationary.   We noted that the first 3 or 4 moments have dynamics which can be modelled and fitted on top of an empirical distribution.  
  
The trending and mean reversion functions were fitted using a maximum likelihood estimate, which was easily obtained from the distribution for each time step under given assumptions.   The parameters were evolved with a GA to maximize the likelihood.

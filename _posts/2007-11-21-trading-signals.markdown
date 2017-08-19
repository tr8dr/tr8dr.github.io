---
author: tr8dr
comments: true
date: 2007-11-21 21:02:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2007/11/21/trading-signals/
slug: trading-signals
title: Trading Signals
wordpress_id: 15
categories:
- bayesian networks
- genetic algorithms
- indicators
---

I'm putting together a framework to evolve, test, and optimize signals using a genetic algorithm approach.   Signals with statistically significant results will be further combined into bayseian networks and fed back into this testing framework.   
  
Determining the conditionality of one signal against another requires insight and guesswork, or evaluating permutations of networks.   With a GA optimizer and enough computing power should be able to determine networks that successfully amplify the combination of signals to one that is correct more often than not (or at least is more successful in the profitable situations  than the losing ones).  
  
The universe of events and indicators that have potential to be significant is large, as are their parameterizations.   Choosing the search space will be a challenge, as is sometimes having access to the required data.  
  
I plan to overlay our tick UI with trading signal indicators indicating a probability weighting when a signal reaches a non-neutral threshold.  Should be interesting to visualize the results.

---
author: tr8dr
comments: true
date: 2010-03-24 19:12:05+00:00
layout: post
link: https://tr8dr.wordpress.com/2010/03/24/market-regime/
slug: market-regime
title: Market Regime
wordpress_id: 637
categories:
- strategies
---

I tend to design strategies that are largely market agnostic, however, I do have a strategy that has behavior conditioned on, what I will loosely call,"market regime".  I am not using "regime" as the basis of a strategy, rather a means to distinguish modality in the distributions used in the strategy.  By market regime may include the following:



	
  1. medium - strong trend upward

	
  2. medium - strong trend downward

	
  3. consolidating / range bound

	
  4. gapping


I'm starting to dust off some work I had done a couple of years ago in detecting these sort of things.  The trick, as always, is in detecting with a minimum of lag, determining the window of analysis, and technique.

**Timeseries Approaches
I've implemented or considered a number of approaches to detecting the above:**



	
  1. **signal decomposition**
The signal is decomposed into wavelet-like bases.   The power of the bases is evaluated to determine which base carries the most power.   The most powerful base is the most representative of the market direction and mode.   This approach is sensitive to window.  May work well with volatility inverse scaled window size.

	
  2. **filter bank**
A bank of filters targeting periods of 2^n are evaluated against the series (ewma's for example).   Based on whether series above or below # of filters determines degree of trend.   Oscillation and period can also be measured.

	
  3. **technical indicators?**
Haven't done much here, perhaps some readers have some ideas.   Observing the average width and direction of a bolinger band can give indications about consolidation, etc.   How does one adjust or choose the MA window?


**Statistical Approach
Now the above is an explicit "technicals" view on market modes.   The idea in the technicals approach is that each of the states implies a different distribution, so depending on what mode you are in, one selects the appropriate distribution (or a blend thereof for a fuzzy approach). **

**It occurred to me that a more statistical approach requiring no prior assumptions would be to **use an algorithm to determine the modes (or categories) within the multivariate distribution**. **

I need to do this on high-dimensional multivariate distributions.  If the distribution is dense or continuous this is relatively straightforward to do.   For a dense empirical distribution one could project a smoothing kernel and solve for maximum points by setting the derivative to 0.

**Simple 1D case**
On a 1 dimensional distribution, this is easy to see:

[![](http://tr8dr.files.wordpress.com/2010/03/1d.png)](http://tr8dr.files.wordpress.com/2010/03/1d.png)

Even in the simple case, determining the degree of smoothing and hence the number of local maxima requires either a judgement call and/or cross validation against model performance.

**High Dimensional Sparse Case**
Attempting to determine the modes in a high dimensional multivariate distribution is a much more difficult.   It occurred to me that one of the machine-learning clustering algorithms may be the best approach here.   K-means is one such algorithm.   I have not yet investigated the efficacy of the approach for my requirements, will post more later when I circle back on this one.

**Tentative Conclusion
**I believe the statistical approach may be superior, particularly as it can capture modes one may not even be aware of.   The statistical approach, however, does not attach any meaning to the modes.   In other words is an opaque model.   Perhaps a mix of determining what factors give rise to the modes via a clustering approach followed by codifying makes more sense.

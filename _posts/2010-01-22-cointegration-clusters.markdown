---
author: tr8dr
comments: true
date: 2010-01-22 22:25:46+00:00
layout: post
link: https://tr8dr.wordpress.com/2010/01/22/cointegration-clusters/
slug: cointegration-clusters
title: Cointegration Clusters
wordpress_id: 554
categories:
- strategies
---

Previously I had written a simple clustering algorithm using correlations to look at rough "relationships" between equities, whether real or "accidental".    I had evaluated this on the S&P 500.

I decided to do a much wider evaluation on all US equities with average volume > 200K and market cap > 500M + the ETFs.   This subset of equities results in about ~2800 assets or 3,780,000 pairs to be evaluated.    Evaluating this in R was impractical, so evaluated in Java.    The evaluation time was in seconds rather than the days R might have required.

To discover stronger relationships evaluated both the correlation and Augmented Dickey-Fuller test on all pairs,  keeping pairs with adf p-value < 0.05.   Additionally did cross-validation against prior years, throwing away pairs that did not fit in the cross-validation period.

I then used my clustering algorithm (outlined in a previous post) to determine networks of maximally related assets.

This resulted in ~1000 pairs (of the original ~4 million pairs).    Some portion of these pairs "make sense" and others are complete surprises.  That said, given the large number of series it would not be a surprise to find "unrelated" assets that are relatively cointegrated over the test period.

It generated ~50 clusters, here is one at random:

[![](http://tr8dr.files.wordpress.com/2010/01/screen-shot-2010-01-22-at-10-25-54-pm.png)](http://tr8dr.files.wordpress.com/2010/01/screen-shot-2010-01-22-at-10-25-54-pm.png)

**Trading The Pairs
T**he beauty of cointegrated series is that they are much easier to model then series with heteroscedasticity or trending mean.    There are a number of approaches to trading pairs (or larger cointegrated portfolios).    Before getting to this, first want to illustrate a non-cointegrating spread:

**[![](http://tr8dr.files.wordpress.com/2010/01/non-coint.png)](http://tr8dr.files.wordpress.com/2010/01/non-coint.png)**

We can see that the spread between NI and ACG has a spread that is growing with time from the axis.  The ideal spread is one that oscillates around a constant mean (generally 0).   The above can be traded, but would involve, first a view that the there is a long run beta differential of ~0.25 and weighing the basket appropriately.

Many look at the relative "beta" (i.e. the slope of the long run cumulative returns) for each asset and determine weights based on a linear regression.    That approach works well if trends follow a near linear path over the observation period.

A better approach is to find the weights such that the spread "spends as much time above the origin as below the origin" (ok, it's a rough heuristic I came up with).   This can be expressed as:


[![](http://tr8dr.files.wordpress.com/2010/01/screen-shot-2010-01-22-at-4-51-49-pm.png)](http://tr8dr.files.wordpress.com/2010/01/screen-shot-2010-01-22-at-4-51-49-pm.png)





Basically the above is "saying": find the integral of some weighting of the spread function such that the area is as close as possible to 0 (i.e. we have balanced sweep above and below the origin).   The constraints make sure that the weights don't go to 0.

**If it is Cointegrated
In theory this is a much simpler scenario where we can chose equal and offset weights (-1, 1) and then analyse the resulting spread for entry and exit (technically one may still adjust the weights to adjust for drift depending on MR period one is focusing on). **

**Next, we want to look for mean-reversion patterns or at least identify levels likely to mean revert conditioned on the past.  Here is a pair that is cointegrated for this period:**

**[![](http://tr8dr.files.wordpress.com/2010/01/coint2.png)](http://tr8dr.files.wordpress.com/2010/01/coint2.png)**

**The typical approach is to normalize the spread to standard deviations and enter reverting trade when 2 SD or another suitable threshold is realized.   Some basic observations of momentum and vol can be used to decide precisely when to enter.**

Another approach used is to calibrate some descendent of the Ornstein-Uhlbleck MR model to the desired level of MR and use as a driver for entry.     I'm not trading pairs at this time, so I'm not sure whether it is worth adopting a MR model.   From past experience with these models, they are hard to calibrate and require significant modification to match empirical behavior, even loosely.

**Beyond Pairs**
We use pairs to provide a more desirable process statistically, more amenable to MR analysis.   There is a much wider universe of possibilities present in "spread baskets".    By "spread baskets" am refering to collections of more than 2 assets that are fractionally long or short, producing a tightly cointegrated return.

Determining such baskets is very complex for a number of reasons:



	
  1. size of search grows at roughly O(N^k), where k is the size of basket and N the number of assets

	
  2. one needs to determine optimal weights (expensive NLP)

	
  3. optimal weights need to be tested in cross-validation


Mitigating the worst case is:

	
  1. can throw away assets with low correlations


To give an example, if we consider the 3-asset case on 2000 stocks, the worst case search would involve 2.6 billion combinations to check.    The correlation matrix may well make this viable however.

**
**

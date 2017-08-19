---
author: tr8dr
comments: true
date: 2010-08-31 17:53:12+00:00
layout: post
link: https://tr8dr.wordpress.com/?p=712
published: false
slug: dependency-sets-implementation
title: Assets Sets
wordpress_id: 712
categories:
- strategies
---

I'm interested in finding the top asset groups of size N for with lookback period P which carry the most predictive power for next period returns.    We might also want to allow the inclusion of other assets in the group of independent variables.   Let our return vector for assets {a,b,c}:


[![](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-31-at-12-42-36-pm.png)](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-31-at-12-42-36-pm.png)


We want to predict the above vector with lagged vectors of the above, augmented (perhaps) with additional assets.   Using lag operator L to indicate 1 or more lags of period P, we are looking to maximize the following:


[![](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-31-at-12-57-58-pm.png)](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-31-at-12-57-58-pm.png)





The TE needs to be normalized so that the baseline entropy of the lagged conditionals is removed to avoid sampling effects.   To do this a baseline TE is calculated with a randomly shuffled LX.   Finally to normalize it into a % of new information explained by the dependent variables can express as:


[![](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-31-at-1-32-00-pm.png)](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-31-at-1-32-00-pm.png)


The next aspect of this is selecting an appropriate symbol set.   Other experiments have been done showing that breaking into an odd number of symbols {3, 5, etc} is more effective than an even one.   In reality very small movements near zero should be treated as a neutral symbol, whether positive or negative.

Choosing 5 symbols, 1 centered around 0 and the other 4 representing 2 ranges on the +/- sides of the distribution, the subdivisions provide the following frequencies of occurance:

[![](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-31-at-2-52-09-pm.png)](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-31-at-2-52-09-pm.png)

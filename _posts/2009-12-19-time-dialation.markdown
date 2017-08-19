---
author: tr8dr
comments: true
date: 2009-12-19 01:56:48+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/12/18/time-dialation/
slug: time-dialation
title: Time Dilation
wordpress_id: 394
categories:
- technical-analysis
- volatility
---

Many measures work best in a homoscedastic volatility regime.   This is not a big secret.    Most regressors, the simplest of which are the ever popular moving averages, are especially biased in the context of a heteroscedastic series.

Probably the best way of normalizing a heteroscedastic series into one with near constant variance is to observe the following.   If we assume our process is roughly a SDE with normally distributed innovations (or alternatively a Hurst constant close to 1/2), we know that:


[![](http://tr8dr.files.wordpress.com/2009/12/picture-14.png)](http://tr8dr.files.wordpress.com/2009/12/picture-14.png)





As a rough measure, we can remove much of the vol of vol by scaling our time axis in proportion to the variance.   I use a duration based local volatility measure with smoothing or alternatively for daily data an EWMA based evaluation of:


[![](http://tr8dr.files.wordpress.com/2009/12/picture-23.png)](http://tr8dr.files.wordpress.com/2009/12/picture-23.png)





We can then change measure:


[![](http://tr8dr.files.wordpress.com/2009/12/picture-33.png)](http://tr8dr.files.wordpress.com/2009/12/picture-33.png)


where ψ(t) is a smoothing / scaling function.   An example of such a scaling (with the red curve in the upper pane indicating the degree of scale from the baseline):

[![](http://tr8dr.files.wordpress.com/2009/12/picture-62.png)](http://tr8dr.files.wordpress.com/2009/12/picture-62.png)

---
author: tr8dr
comments: true
date: 2009-11-27 16:36:19+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/11/27/anatomy-of-a-spike/
slug: anatomy-of-a-spike
title: Anatomy of a Spike
wordpress_id: 298
categories:
- statistics
- volatility
---

Spikes manifest in intra-day markets frequently.   These are often short-lived and associated with buying/selling programs more often than change in fundamental factors, particularly in low-liquidity periods.   In evaluating duration and variance measures was trying to determine reasonable jump thresholds.

Below is a price series demonstrating a variety of spiking behavior:

[![](http://tr8dr.files.wordpress.com/2009/11/picture-104.png)](http://tr8dr.files.wordpress.com/2009/11/picture-104.png)

Taking a look at the region around the jump at high frequency, we note that the jump did not occur with one trade rather with multiple within a short space of time:

[![](http://tr8dr.files.wordpress.com/2009/11/picture-114.png)](http://tr8dr.files.wordpress.com/2009/11/picture-114.png)

From a duration perspective, if we want to capture the spike as one event of a given magnitude we either need to consider the cumulative return over a given window or sample with a longer period.   Here is the same with a 2 second sample:

[![](http://tr8dr.files.wordpress.com/2009/11/picture-121.png)](http://tr8dr.files.wordpress.com/2009/11/picture-121.png)

Here is the 2 second sampling of the original range.   With the longer sample period, the spikes in return are more evident (compare to the first graph in the post):

[![](http://tr8dr.files.wordpress.com/2009/11/picture-131.png)](http://tr8dr.files.wordpress.com/2009/11/picture-131.png)











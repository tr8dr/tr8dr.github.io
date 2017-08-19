---
author: tr8dr
comments: true
date: 2009-11-27 18:55:24+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/11/27/durations-on-intraday-price-series/
slug: durations-on-intraday-price-series
title: Durations on Intraday Price Series
wordpress_id: 305
categories:
- statistics
- volatility
---

As mentioned in a [previous post](http://tr8dr.wordpress.com/2009/11/26/rethinking-variance/), I intend to model quadratic variation in terms of multiple pairings of intensity (duration) and return level processes.   At a minimum want a pairing for "non-jump" related returns and a pairing for "jump" related returns.

To do this it is necessary to partition returns into the categories based on threshold.   We may further want to disregard price movements below a certain level unless they cumulatively add up to a return with significance within a period.   Towards this end my duration measurement function uses a threshold to determine whether a return is to be considered as an event or not.  In pseudocode:

    
    r ← {0} ∪ diff(log(series))
    t ← times (series)
    durations ← {}
    <strong>for</strong> (i in 2:length(r))
    {
        <span style="color:#008000;"># determine cumulative return since last acceptance</span>
        cumr ← <em><cummulative return since last event or max cum window></em>
    
        <span style="color:#008000;"># determine whether qualifying event has occurred</span>
        <strong>if</strong> (|cumr| ≥ threshold <strong>or</strong> |r[i]| ≥ threshold)
            durations ← durations ∪ {t[i] - <em><Tlastevent></em>}
    }
    


For the diffusion portion of the process, in this 2 second sampled data set (EUR/USD low-liquidity period), a threshold of 3e-5 (equivalent of about 1/2 pip), seems to work well:

[![](http://tr8dr.files.wordpress.com/2009/11/picture-115.png)](http://tr8dr.files.wordpress.com/2009/11/picture-115.png)

The jump portion of the process should be set so as to capture desired jump features and not much more, here I show with a threshold of 2e-4 (equivalent to about 3 pips):

[![](http://tr8dr.files.wordpress.com/2009/11/picture-215.png)](http://tr8dr.files.wordpress.com/2009/11/picture-215.png)

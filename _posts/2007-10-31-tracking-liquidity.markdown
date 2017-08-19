---
author: tr8dr
comments: true
date: 2007-10-31 22:10:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2007/10/31/tracking-liquidity/
slug: tracking-liquidity
title: Tracking Liquidity
wordpress_id: 10
---

Encountered a problem in trading where we did not properly adjust the liquidity (depth) level coming back from the exchange.  Basically when we push down an order would decrement available liquidity.  The next tick from the exchange would reflect the new available liquidity (or so we naively thought).  
  
The reality is that one or more ticks can be received before a order (which was filled) is reflected in the depth level.  Given this, the affect of the order will not be represented in the depth.  
  
To fix this, two different approaches could be taken:  


  1. Estimate latency in hitting the depth x 2  
Hold the order impact across any ticks within this period  
  

  2. Hold the order impact across ticks until trade confirm comes back  
This is the most conservative approach.  On some venues trade confirms are sent back with lower priority, so can be an overly conservative method of measurement.  

The net affect of not handling this properly was that we saw more depth than was actually there.   This meant that for aggresive algos trying to pull some part of advertised depth, ended up with partial fills based on a flawed liquidity tracking model.  
  
Have taken the conservative approach at this point.

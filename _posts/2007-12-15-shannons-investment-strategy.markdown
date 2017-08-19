---
author: tr8dr
comments: true
date: 2007-12-15 22:16:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2007/12/15/shannons-investment-strategy/
slug: shannons-investment-strategy
title: Shannon's Investment Strategy
wordpress_id: 17
categories:
- portfolio
---

Was reading the book 'Fortune's Formula', which I highly recommend.   [Claude Shannon](http://en.wikipedia.org/wiki/Claude_Shannon), the genius of Information theory fame, came up with an approach to investing in the market using a interesting variant of Kelly's betting approach.  
  
Assuming a market with constant mean (no drift / trend over time):  


  1. Invest 1/2 of your capital in an asset
  2. Periodically rebalance
  3. If the market went up, sell enough units of the asset to have exactly 1/2 of your capital invested
  4. If the market goes down, buy enough units of the asset to maintain 1/2 investment
This is an effective scheme (assuming no transaction costs).   Why?  


  1. rebalancing implicitly executes a mean reversion strategy  

  2. losses reduce the capital in the market
  3. wins increase the capital in the market  

In effect, this is a ratcheting investment approach.  As was pointed out, most assets are not constant mean over time.  This would imply a strategy that trades mean reversion around a longer term drift in the mean.   How might such a strategy work?  


  1. since drift might be upwards or downwards, fundamental position should be long or short
  2. rebalancing should take into account the expected movement of the mean so that the ratio of cash to position will depend on this
This is referred to as a Constant Rebalanced Portfolio (CRP).   Thomas Covers, later extended on this concept with non-even distribution of allocations with his Constant Universal Portfolio (CUP).

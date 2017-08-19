---
author: tr8dr
comments: true
date: 2008-01-20 08:41:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2008/01/20/evolution-of-distribution/
slug: evolution-of-distribution
title: Evolution of Distribution
wordpress_id: 20
categories:
- genetic algorithms
- pricing
- statistics
---

For a number of problems, understanding the evolution of the distribution over time is important.  The distribution tends to be stable over longer periods and unstable over shorter periods.  
  
The distribution is going to be measured from a sample over some time period.  One may want to take a blend of distributions measured over different time periods, combined as basis functions with weights summing to 1.  
  
The interesting bit is predicting the distribution forward with some statistical accuracy.  The order book and momentum indicators should tell us something about how the distribution is going to transform over the next period or based on when a certain price level is achieved.  
  
We are going to use a GA to calibrate the transformation function against historical data.   There are many different functions we could use, so we use a GP approach to play with the permutations.

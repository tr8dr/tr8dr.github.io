---
author: tr8dr
comments: true
date: 2009-09-17 23:01:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/09/17/momentum-strong-indicator-in-daily-returns/
slug: momentum-strong-indicator-in-daily-returns
title: Momentum strong indicator in daily returns
wordpress_id: 30
---

On the side have been working with someone who is looking for long term strategies in the fixed income space.  My strategies focus on intra-day trading primarily, but have found the start of a number of very attractive longer term (low frequency) strategies.  
  
In particular, we are building a multi-factor model to predict market movements for Canadian bonds.   Alternatively, we are also looking at cointegration models that would be implemented as long/short baskets of securities.  
  
Sometimes the simplest ideas work best.  I decided to look at a function of momentum over a period as a predictor of return over the following period.   Did not expect to have such strong results.  Here is the average return predicted by momentums at various standard deviations from parity:  
[![](http://tr8dr.files.wordpress.com/2009/09/picture1.png?w=300)](http://tr8dr.files.wordpress.com/2009/09/picture1.png)  
An alternate graph of this showing standard deviation bands for returns against momentum levels:  
﻿[![](http://tr8dr.files.wordpress.com/2009/09/return.gif?w=300)](http://tr8dr.files.wordpress.com/2009/09/return.gif)  
There is certainly more work to be done to understand maximum drawdown and optimal money management.   
  
Next Steps  
Beyond momentum, we are also looking at building a continuous economic index (much like the [Aruoba-Diebold-Scotto Business Conditions Index](http://www.philadelphiafed.org/research-and-data/real-time-center/business-conditions-index/)).   This provides a continuous forecast of economic variables based on a stochastic state space model.   Will discuss further in the next post.

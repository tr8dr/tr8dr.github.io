---
author: tr8dr
comments: true
date: 2010-09-05 22:10:12+00:00
layout: post
link: https://tr8dr.wordpress.com/2010/09/05/mean-reverting-strategies/
slug: mean-reverting-strategies
title: Mean Reverting Strategies
wordpress_id: 731
categories:
- strategies
---

Recently there have been some discussions on one of the quant groups, where someone indicated "mean reverting strategies no longer work".    I don't agree with this at all.    However, there has been a lot of risk aversion and the market movements of the last few months (well since Feb or March) have been quite different then those preceding.    The market of the last few months has been ideal for market making given the tight bands and much less favorable for strategies that rely on trends or big swings.

One of my strategies is quite simple and depends on mean-reverting behavior across a large portfolio.   When MR behavior in the market is "cranked back" it makes sense to filter the portfolio, increasing the chance of successful trades and reducing possible losses from assets without the desired footprint.

I decided to do some tests on the biggest winners and losers by asset to confirm my belief that wining assets would exhibit strong negative autocorrelation and losing assets would likely exhibit positive or statistically not-significant levels of autocorrelation (+/-).  Indeed one of the biggest losing assets had the following profile:

[![](http://tr8dr.files.wordpress.com/2010/09/screen-shot-2010-09-05-at-5-32-48-pm.png)](http://tr8dr.files.wordpress.com/2010/09/screen-shot-2010-09-05-at-5-32-48-pm.png)

The above asset spends most of the time in positive auto-corr territory and does not tend to mean-revert over the window of interest.    The asset below spend most of its time in negative auto-corr territory and hence is very strongly mean-reverting:

[![](http://tr8dr.files.wordpress.com/2010/09/screen-shot-2010-09-05-at-5-27-27-pm.png)](http://tr8dr.files.wordpress.com/2010/09/screen-shot-2010-09-05-at-5-27-27-pm.png)

Of course measures like this have lag associated with them, but if chosen carefully can be used to effectively filter assets on the run.   Here is the test code in R:

[![](http://tr8dr.files.wordpress.com/2010/09/screen-shot-2010-09-05-at-9-18-26-pm.png)](http://tr8dr.files.wordpress.com/2010/09/screen-shot-2010-09-05-at-9-18-26-pm.png)

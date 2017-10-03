---
author: tr8dr
comments: true
date: 2017-10-02 17:00:00+00:00
layout: post
title: Roll Your Own RoboAdvisor [1]
categories:
- strategies
---

I have two pools of capital, one for active trading and another for long-term investment / lower-risk capital preservation.  I go through phases of actively managing investment capital and then phases where become too busy to do so properly.  It would be convenient to hand off the management to one or more funds, invest and forget, but given the market uncertainties and what I know about wall street, trust is hard to come by.   Indeed since the financial crisis, Hedge funds have been gradually falling out of favor, while investments into ETFs have skyrocketed.

Perhaps as a response to the financial crisis and a certan cynicism regarding traditional financial services, robo-advisors have become popular.  I have looked at Betterment and a variety of other robo-advisors.  While they have slick reporting and have done reasonably well performance wise (albiet in a bull market), they do not offer the sort of flexibility, constraints, or downside protection I need.

I have not done much with investment portfolio design, but figure since have quite a bit resting on a sound strategy, worth the effort.  At the same time know that this is a black hole of sorts, so will try to start with some relatively simple strategies and optimisation approaches.   Some obvious dimensions:

- **asset or strategy universe**
- **constraints around risk and/or expected return**
- **optimization to some measure of risk/return**
- **downside protection**

## Asset / Strategy Universe
In the spirit of keeping things "simple", I want to look at:

- **CRP (constant rebalanced portfolio) strategy** for specific names or ETFs with mean-reverting behavior
  * this will require some measure of expected forward MR, as CRP is a losing strategy otherwise.
- **4 and 6 factor French / Fama model**, utilizing factor replicating ETFs
  * the 6 factor strategy appears to describe most price variation, and potentially can be used as a toolset to achieve certain risk / return criteria
  * the reality is that this can be quite complex; for example, some factor ETFs actually combine multiple factors, so cannot be composed orthogonally.
- **Top Quintile vs bottom quintile assets, based on vol skew**
  * yet to be determined in terms of how effective, pending ongoing vol surface analysis

## Utility Functions & Optimisation
Will evaluate a number of different optimisation measures:

- **minimum expected shortfall**
  * (basically the lower 2nd moment)
- **constant relative risk aversion**
  * Some discussion of it [here](https://en.wikipedia.org/wiki/Risk_aversion)
- other

## Downside Protection
The fourth element, in particular, is missing in the robo advisor world.  Depending on market, may decide to hedge for a K% downside, by buying OTM puts on ETFs that are highly correlated with the underliers.  One can partially or fully finance the puts by selling OTM calls.

Roughly speaking, we could determine the ideal hedging ETFs (assuming that do not hedge with options on the portfolio assets) by either:

- determining the covariance of potential hedging ETFs with the portfolio and determining the principal components
- optimizing the portfolio with the potential hedge options, allowing for weights = 0 on the hedges, so as to cause redundant hedging assets to fall away.

## Final Notes
The above text is a high-level plan of work to be done.  Will explore this in detail and post the process and results to the blog as I work through it.   For now have a month or so of work to do in investigating the [Information In Volatility Structure](https://tr8dr.github.io/Volatility-Surfaces/).


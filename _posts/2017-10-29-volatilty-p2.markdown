---
author: tr8dr
comments: true
date: 2017-10-29 17:00:00+00:00
layout: post
title: Information In Volatility Structure [2]
categories:
- data
- strategies
---

In the prior post [Information In Volatility Structure [1]](/Volatility-Surfaces/) applied the SABR model to fit noisy raw option price data of approximatelty 700 million prices across a 10 year history of 2700 stocks.  The point was to examine a hypothesis:

* does supply / demand imbalance in the options market express in terms of abnormal vol skew?
* can abnormal vol skew point to forward market behavior?

## First Application
I started by observing both put/call skew and skew within the put or call curves independently.  Observed the following:

* abnormal spikes in vol skew often marking apparent pivot tops or bottoms
* low level or almost no abnormal skew activity during sideways or trending moves

Here is an example, showing a 6 months of MSFT activity and corresponding put and call skews:

![MSFT](/assets/2017-10-29/MSFT.png)

In the above example MSFT has large 10% cycles, where abnormal skew marked turning points in direction.  Here is another example with SPY:

![SPY](/assets/2017-10-29/SPY.png)

Observed this behavior across quite a few stocks across the historical period, however, its usefulness appears to be constrained to situations where there are large pronounces price cycles (for example cycles with a high amplitude relative to vol).   In the case of scenarios with a long-running trend or low-amplitude mean-reversion did not see pronounced skew signals.

Given the current market conditions, where dips are being bought by algos, stock buybacks, and otherwise, the approach may have limited usefulness.  In current market conditions, while it cannot identify direction, may be useful in pointing to a top, where a spike in skew(s) provides some investor concensus.

## Second Application
I also took a look at a rank-based portfolio of stocks for a 1 month holding period, based on stock skew rank:

1. select the top K stocks on a given observation date by skew
2. observe the forward return (1day, 1wk, 1mo) for the selected stocks (with equal weights)
3. look at the cumulative return across the time period

Without any directional guidance (in terms of deciding on long or short), going long on the top K stocks by skew yielded an annual return of 15%.  Given that the skew spikes tend to occur around cycles (though with more positive than negative forward return bias), it should be possible to also determine likely direction based on whether the skew spike occurred at a cycle bottom or top.   The rule might then be as follows:

1. given some metric, determine whether near bottom or top of a cycle:
   * if near bottom go long
   * if near top go short
2. if no detectable cycle of some minimum amplitude then assume is a long signal OR ignore the signal entirely

I did not pursue this refinement in identifying likely direction.  Anecdotally, based on some investigation, expect it could improve return by 5% to 10%.

## Conclusions
I am going to put this aside for a while.  While I think there could be a strategy here, have some lower-hanging fruit that want to pursue before looping back to this.

Also note that I performed this analysis on a 2002 - 2012 data set.  The market evolves, so is possible that this effect has become muted over time.   As of 2012, the effect was still viable.  When I pick this up again will purchase another 5 years of data and close the gap with the current regime.






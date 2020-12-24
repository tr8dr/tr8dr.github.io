---
author: tr8dr
comments: true
date: 2020-12-24 11:00:00+00:00
layout: post
title: New Equities Strategy (p2)
subtitle: 
categories:
- strategies
---
In the prior post I showed results for a new equities strategy which uses a combination of signals to
create and risk manage a high-momentum portfolio.  Further investigation revealed that I had neglected
on a couple of fronts:

- failed to account for dividends (which are substantial)
- some data issues
- improper sharpe calculation

The good news is that solving these issues substantially improved the profitability of the strategy (which was
already high return for a low-freq strategy).

<img src="/assets/2020-12-24/strat2.png" width="800" height="600" />

The strategy invests in a portfolio of stocks from period to period.  Some of these stocks will underperform
or experience unrealized drawdowns during the holding period.  I spent some time trying to determine if there was a way to detect a 
losing asset and cut it earlier to reduce impact on the portfolio return.  Many profitable
stocks see a small negative unrealized drawdown during the holding period:

| unrealized drawdown | %       | % with positive outcomes |
|---------------------|---------|--------------------------|
| all                 | 100%    | 65%                      |
| touched < 0         | 72%     | 52%                      |
| touched < -2%       | 55%     | 41%                      |
| touched < -5%       | 33%     | 22%                      |
| touched < -10%      | 14%     | 4%                       |

Note that overall, in spite of 72% of assets touching a negative unrealized drawdown during the holding period, the
combined return of the portfolio over each period was __positive 81.6% of the time__.  Looking at the above table one
might decide to __cut assets that touch the -5% level__, however this __produces lower P&L__ for the following reasons:

- mean reversion of price during holding period (to more positive outcome)
- loss of right tail on the 22%

A __money-management approach that is effective__, is to condition on:

- negative return (-5% for example)
- buy/sell imbalance as a measure of negative momentum
- target mean-reversion of at least 50% of the unrealized loss

However, this __only improved the strategy by 48%__ over the 9.75 yr period, and as I was concerned with avoiding data-mining bias, 
so did not include in this round.  One reason __why leaving drawdowns unadjusted__ is favorable is that this strategy has
very strong right tails:

<img src="/assets/2020-12-24/asset-dist.png" width="600" height="400" />

The above is the realized return distribution for individual assets during their holding periods. 


## Next Steps
At this point have solved some important issues with the strategy and backtest.  I am starting to trade this manually,
however, I need to get this up and running systematically.  Status of work to be done:

1. __Strategy__ (complete for backtesting)
   * May need some adjustments for live execution
   * Strategy and strategy framework written in Kotlin, research done in Python (and in the past R). 
2. __Data Feed__ (only historical currently, need live)    
   * I am curently using Polygon.io for historical data, though have had to mix in data from other sources due
     to a few stocks (out of 1700) missing back history or with data issues
   * Splits & Dividend information have been a combination of [Polygon.io](https://polygon.io/), Yahoo, and other
     sources.  I run a reconciliation to arrive at correct information.  In some cases I have to manually edit my meta 
     data.  Hopefully find a reliable single source for this.  Polygon is working on improving their equities meta
     data, but is a work in progress.
3. __Execution__ (partially implemented)
   * I am not running this strategy professionally, rather for my own money.  Hence the easiest options available 
     would be brokers that service the retail community.  [Alpaca] (https://alpaca.markets/algotrading) has the best
     API of the lot.  Other alternatives would be TD or IB.
4. __Monitoring / UI__ (not started)
   * I am quite familiar with R/shiny as a way to quickly put up a UI for monitoring & controls.  However I prefer 
     Python over R.  Dash is far less mature, but may attempt to put something together with it.


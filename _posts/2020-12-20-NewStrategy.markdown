---
author: tr8dr
comments: true
date: 2020-12-20 11:00:00+00:00
layout: post
title: New Equities Strategy
subtitle: 
categories:
- strategies
---
Given that I am busy with higher-frequency strategies generally, thought should find a longer-term portfolio strategy
that could apply systematically at low-frequency for my passive funds.  Momentum seems to have been a dominant force
in the equities market for the last 20 years or so, with relatively short bear or drawdown interludes.  I have no illusions
that market behavior will remain the same, however the following continue to weigh in favor of momentum
strategies in equities, IMO:

- money searching for yield
  * corporate bond yields are at all time lows relative to prior years 
- dollar devaluation
  * this may benefit certain companies, as well as make their asset valuations in $ terms higher
- herding behavior of investors
- behavior of mutual funds and retirement funds

and many other factors.  For reference this is where corporate bond yields are currently:

<img src="/assets/2020-12-20/yields.png" width="800" height="300" />

Towards this end have come up with a strategy that make use of the following:

- buy/sell imbalance
- use signals from VIX ETFs & derivatives for risk management
- measures of momentum
- measures of risk of MR  
- portfolio selection across the top 1700 highest volume stocks (no ETFs)

The holding period ranges from 10 days to 30 days, depending on parameterization and selects a portfolio of 10 or more
stocks expected to be optimal for the forward period.  The strategy makes trading decisions
at 3:40pm (which can be randomized without much impact) and trades either MOC or LOC in the closing auction.

Anyway, here is one parameterization of the strategy:

<img src="/assets/2020-12-20/S1.png" width="800" height="600" />

As I want to get more granular information re: strategy performance for risk / money management, working on a variation
that will stagger entry into a number of portfolios.  For example use 1/10th of the capital to enter on day 1, the
next 10th 2 days forward with a new optimisation, etc.

## Improvements (TBD)
1. __Data sample__
   * I am not entirely comfortable with the # of data points available to backtesting.  For example, 
     my risk management depends on some derivative signals on VIXY (and later VXX), however these ETFs did not exist during
     the 2008 period.  Given that I need volume information, I would not be able to use the VIX index to create synthetic 
     back history for the ETFs.  Instead I would need to create an equivalent of the ETF with VIX futures.
2. __Risk Management__     
   * On the upside, out-of-sample, the risk signal took the strategy out of market during the March 2020 market drop and
     put us back in the market near the low.  It does not seem likely would be as lucky in all such drawdowns.
   * I think can cut short on drawdowns during risk-on periods as there seems to be auto-correlation between
     returns (i.e. a 3rd or 4th loss will follow 2 consecutive losses).
3. __More granularity__
   * As mentioned above would like to get a more granular view on returns so can determine when the market is not 
     conducive to the strategy.

I will explore some of the above before launching, however my inclination is to start trading it soon and get experience 
with the forward behavior of the strategy, since live and out-of-sample can differ.  I don't anticipate any issues with execution, 
at least for my initial size.  If scale up later may have to make more allowances for not being able to fully execute at the close 
or having impact on the close in some stocks.
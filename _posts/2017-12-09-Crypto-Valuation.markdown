---
author: tr8dr
comments: true
date: 2017-12-09 17:00:00+00:00
layout: post
title: Bitcoin Valuation Fundamentals
categories:
- data
- strategies
---

Bitcoin has entered the mainstream, though not in a way that is particularly useful.  Many, including myself, are calling a bubble in Bitcoin.  As with many bubbles when the "mom and pops" and non-professional investors get into a buying frenzy, historically this has been associated with the last stages of a bubble.  Momentum from individual buying may persist for some time, potentially in cycles of buying dips, so would not want to get short, without a strong view on sentiment (the only fundamental for bitcoin right now in my opinion).

All that aside, I want to discuss bitcoin and other crypto currencies from a **fundamentals-based valuation perspective** in this article.  Bear with me while I get to my point towards the end.

## Bitcoin Problems
With the seminal Satoshi Nakamoto paper describing bitcoin, the view was towards creating a currency unbeholden to governments and suitable for a wide range of transactions, including micro-payments, making use of a distributed ledger.  While Bitcoin has succeeded in creating a currency where there is no centralized party controlling money supply or acting as a gatekeeper for transactions, the current implementation has failed in terms of presenting a viable currency for transactions.   Key problems are:

1. **Time required to process a transaction**
   * Roughly 10 minutes per block, and with another ~40 minutes required to determine whether the transaction is part of the longest chain (4 - 5 blocks later)
2. **Transaction cost**
   * The current mining reward is 12.5 bitcoin which translates to about $200K at today's bitcoin valuation, plus additional transaction fees "demanded" by miners in order to include a transaction in the block.   This is **insanely** expensive.
3. **Limited block size**
   * Most agree that the block size needs to increase under the proof-of-work regime, though there has been no agreement on implementation.  That said, while larger blocks can address volume (how many transactions can be processed in a block) they do not address latency (the amount of time it takes to clear a transaction).

The above problems are a significant impediment in using bitcoin as an everyday currency.  In addition, proof-of-work is enormously expense and wasteful.  Clearly bitcoin will need to make some significant adjustments to its technology, which will get into later in this article.

## Asset Valuation
Before we discuss a framework for crypto valuation, is useful to explore characteristics of other assets with respect to valuation from an investment perspective (at a high level):

1. **FX**: valued on a relative basis to other currencies in terms of
   * relative PPP (purchasing power parity)
   * relative interest that can be achieved by depositing money in one currency versus another in a term deposit
   * fungibility & liquidity
   * growth or contraction of money supply, relative inflation, jobs, and a whole set of other macro factors
2. **Equities**:
   * valuation of company in terms of assets less liabilities and expected growth in revenues
   * dividends
   * liquidity (can I easily buy or sell in exchange for cash)
3. **Commodities**:
   * demand vs supply for a given commodity
   * demand based on use in production of manufactured goods or consumption
   * supply based on mining, drilling, farming, etc.
4. **Bonds**:
   * schedule of coupons paid to bond holder over the term of the bond; this provides a very clear investment and valuation approach.  If I buy a 10 year bond for $100 with a 2% annual coupon I can determine the overall value accrued to me in the coupon payments
   * bond yield relative to treasuries or libor curve
   * credit risk as determined by market and ratings agencies

All of the above asset classes provide clear underlying factors and reasonable forward price valuation models.  An investor can determine why the price moved historically and have a reasonable view on forward price valuation with respect to the underlying factors.  Each of these assets has an expected rate of return either contractually as with a bond, FX deposit, or equity dividend or is subject to fairly well understood supply and demand cycles with many observable supply and demand factors. 

## Bitcoin Valuation
What fundamentals can bitcoin draw on?

1. cost of mining
   * approximately $19,000 per block according to some sources, putting a break-even bitcoin valuation at $1520 (this is determined based on the current hashing power and estimated cost to run versus the 12.5 BTC / block reward, ignoring embedded transaction fees in the block, which would reduce the break-even price even further)
2. limited supply
   * if bitcoin was to become a basis for financial transactions, the limited supply would drive the price of bitcoin upwards
   * However, as have tried to demonstrate above, Bitcoin in its current form, is not suitable as an alternative to fiat.
3. supply & demand
   * this is clearly what is driving the price now; a buying frenzy with significant assymetry between the number of buyers and sellers.  However, once the buyers and sellers reach equilibrium there will be nothing to drive the price higher; this would tend to be followed by selling once is clear that momentum has turned.

I don't see a concrete basis for valuation here.  In the next section, lay out how we can get there in the crypto community. 

## Providing a Fundamental Basis (through proof-of-stake)
There has been much discussion regarding moving away from **proof-of-work** which is not only hugely wasteful resource-wise but also hampers the transaction processing time and imposes a high cost of transaction.  The current view is that **proof-of-stake** solves a number of these issues in that:

* blocks can be processed much more quickly given that there is no cryptographic proof-of-work puzzle to be solved
* less wasteful in terms of resources

The general idea with proof-of-stake is that mining power (and reward) is attributed in proportion to the coins held by the miner.  A miner with a 100x more coins than another participant will receive 100x more reward than the 1x participant.  The "miners" basically posit their coins as a stake in the block validation process to receive these rewards (or if you like, "dividends").  I won't go into the details of how this works or the research on the relative security to proof-of-work, as want to focus on another point here.

Given the proportional reward payout to coin-holdings, proof-of-stake enables an **interest rate mechanism for crypto!**  Holding coins in a proof-of-stake coin and "depositing" for use as a stake and participant in the block-clearing function provides a continual stream of return.  For example if the proof-of-stake reward per coin per year was 1/4 coin per staked coin, would provide a effective 25% annual return in addition to price appreciation (or depreciation) of the underlying coin price.

With multiple proof-of-stake coins one can now perform a relative-value comparison of two or more crypto coins.  In addition the return and volatility can be compared to more traditional securities such as corporate bonds, equities, etc.  Here is a hypothetical way to look at it:

| Coin      | reward / staked coin / yr | return | price volatility |
| --------- |:-------------------------:|:------:| ----------------:|
| Foo Coin  | 1/2                       | 50%    | 125%             |
| Bar Coin  | 1/4                       | 25%    | 43%              |

Based on the above, one might choose to invest in the "Bar" coin in order to obtain a 25% return, but with lower underlying price volatility.  Alternatively, one might opt for the higher risk, but higher return "Foo" coin with a 50% annual return, though with high price volatility and higher depreciation risk.  One could add high yield bonds and other return generating instruments as a point of comparison.

## Conclusion
I have sketched an approach towards evaluating crypto fundamentals.  The crypto space needs to integrate with real-world assets and/or provide returns so that can be better assessed from a valuation perspective.  In addition, proof-of-stake is the appropriate direction for crypto, in that solves both a transactional problem and provides a basis for fundamental valuation.

I would also like to note that the emergent CME, CBOE, and BitMEX futures add some term structure and implied rate of return to Bitcoin, however less directly and less effectively than proof-of-stake would enable.  



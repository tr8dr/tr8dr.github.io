---
author: tr8dr
comments: true
date: 2015-01-28 00:55:21+00:00
layout: post
link: https://tr8dr.wordpress.com/2015/01/27/consolidated-source-of-data-for-bitcoin/
slug: consolidated-source-of-data-for-bitcoin
title: Consolidated Source of Data for Bitcoin
wordpress_id: 1042
categories:
- strategies
---

It seems like every other month there is a new bitcoin exchange.  For the purposes of trading research & backtesting it is important to have historical data across the most liquid exchanges.  My minimal list is:

	
1. **BTC/USD**
   1. bitfinex (15%)	
   2. bitstamp (5%)
   3. coinbase (new, but likely to garner market share)
2. **BTC/CNY**
   1. okcoin (28%)
   2. btcn (44%)


(percentage volume sourced from [http://bitcoincharts.com/charts/volumepie/](http://bitcoincharts.com/charts/volumepie/)).   Each of these exchanges not only has a unique protocol but also unique semantics that need to be normalized.

For example, bitstamp produces the following sequence of transactions for a partial sweep of the orderbook.  For example, here is a partial sweep, where a BUY 14 @ 250.20 was placed, crossing 4 orders on the sell side of the book:

![Orderbook sweep](/assets/2015-01-28/screen-shot-2015-01-27-at-7-22-17-pm.png)

In Bitstamp, would see the following transactions:
	
1. **NEW BUY 14 @ 250.20, id: 43**
2. DEL  SELL 1.2 @ 250.05, id: 23
3. **UPDATE BUY 12.8 @ 250.20: id, 43**  (updating the size of the aggressing order)
4. TRADED 1.2 @ 250.05
5. DEL SELL 0.3 @ 250.10, id: 24
6. **UPDATE BUY 12.5 @ 250.20: id, 43**  (updating the size of the aggressing order)
7. TRADED 0.3 @ 250.10
8. ...
9. TRADED 8 @ 250.20
10. DEL BUY 0 @ 250.20, id: 43

The oddity here is that many market data streams & orderbook implementations will just transact the crossing in 1 go, so one will usually only see:  DEL, TRADE, DEL, TRADE, DEL TRADE (and deletes may not be sequenced between the trades either).  Where it gets odd is in replaying this data in that a typical OB implementation will sweep the book on seeing the order right away without intermediate UPDATE states.   In such an implementation, seeing UPDATE to non-0 size after crossing and deleting the order completely might be seen as an error or a missed NEW, since the order is no longer on record in the OB.

Another note is that Bitstamp does not indicate the side of the trade (i.e. which side aggressed), though this is uncommon in markets such as equities or FX, Bitcoin exchanges do provide this.   Fortunately because the initial crossing order is provided can use a bipartite graph (in the presence of multiple crossing orders) to determine the most likely aggressing order and therefore the trade sign.

## Clearing House for Data

I would like to build and/or participate in the following:

1. build robust normalized L3 or L2 -> L3 (implied) orderbook live feeds
   - used to collect data into a simple binary tickdb format
   - also can be reused as connectivity handlers for live trading
2. normalize transaction stream (such as issues in the example above)
3. identify buy/sell designation on trades based on exchange specific semantics
4. in addition to exchange specific tick streams / dbs, create a consolidated OB stream:
   - synchronized market state to nearest ms
   - normalized orderid space so that order ids do not collide and can identify order source
5. simple means to generate bars or filter for trades from the L3 data


It takes some amount of time to develop & fairly small amount of money to run in terms of hosting.   Assuming there are not EULA issues in doing so, could perhaps provide data as a non-profit sort of arrangement.   Not looking to build a for-profit company around this rather a collective where can give something back to the community and perhaps be able to make use of donated resources and/or data.

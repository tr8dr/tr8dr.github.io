---
author: tr8dr
comments: true
date: 2015-03-14 18:46:54+00:00
layout: post
link: https://tr8dr.wordpress.com/2015/03/14/musings-on-hft-in-bitcoin/
slug: musings-on-hft-in-bitcoin
title: Musings on HFT in Bitcoin
wordpress_id: 1104
categories:
- strategies
---

I have 4 Bitcoin L3 exchange feeds running smoothly out of a data center in California (which is slightly closer to Asian exchanges and Coinbase than the east coast).  It took a bit of error handling and exponential back-off, to handle the unreliability of connectivity with these exchanges, where connections can intermittently be overwhelmed (returning 502 / 503 errors due to the poor choice of a REST-based API).

I am thinking to add Bitstamp and Kraken to the mix, even though they are smaller.   Bitstamp seems to have recovered somewhat since its security breach and Kraken is unique due to its EUR denominated trading.

## HFT Opportunity

Bitcoin trading & order volume is quite far from the hyper-fast moving equities, FX, or treasuries markets.   That said, it has significant potential for market makers and short-term prop-trading given the greater transparency of microstructure in this market.    The caveat to this is that the transaction costs on many exchanges are enormous (10+bp or 20+bp round-trip), though for BTCChina is surprisingly free (fees are on withdrawal).

I am really at the beginning of collecting HFT-style data for the main exchanges, so while I have tested a couple of signals on very small samples, want to collect a larger data set for backtest and fine-tuning.   The 2 signals have tested are around short-term momentum detection, so can either use to follow momentum or use as a risk-avoidance in a market making engine.  In the later case would remove my offer from one side of the market if detect 1-sided direction, avoiding adverse selection.

## Restrictions

Some marketplaces look to put in measures to keep their marketplace sane for lower-freq / non-HFT specialized traders.   In looking at the Kraken API today, noticed the following:


> We have safeguards in place to protect against abuse/DoS attacks as well as **order book manipulation** caused by the rapid placing and canceling of orders ...
>
> The user's counter is reduced every couple of seconds, but if the counter exceeds 10 the user's API access is suspended for 15 minutes. The rate at which a users counter is reduced depends on various factors, but the most strict limit reduces the count by 1 every 5 seconds.


So effectively Kraken will allow for a small burst of order adjustments or placements, but only allows an average of 1 adjustment / 5 sec.   For a market maker 1 every 5 seconds (or 2 sides every 10) is too limiting.   This can become a problem for both market makers and trend / momentum followers if the market is moving.   Perhaps this limit should scale with respect to price movement or be combined with a higher limit + minimum TTL (time to live).

I am actually **for** certain restrictions in the market.  The equities market, in particular, needs to be cleaned up, not with new taxes or ill-conceived regulation (reg-NMS for example), but with:

	
  1. time-based priority / price-level  (believe it or not, there are order types that allow HFT to jump the queue)
  2. minimum TTL (time-to-live)
  3. some reasonable maximum # of orders / unit time on a security


#2 and #3 would remove many of the games on the exchange that do not serve the market.





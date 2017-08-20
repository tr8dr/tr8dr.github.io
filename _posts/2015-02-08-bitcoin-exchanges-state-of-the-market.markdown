---
author: tr8dr
comments: true
date: 2015-02-08 13:50:01+00:00
layout: post
link: https://tr8dr.wordpress.com/2015/02/08/bitcoin-exchanges-state-of-the-market/
slug: bitcoin-exchanges-state-of-the-market
title: 'Bitcoin Exchanges: State of the Market'
wordpress_id: 1061
categories:
- strategies
---

In the previous post outlined intention to put together high quality L2/L3 feeds for the top 4-5 bitcoin exchanges, collect L3 data, and provide a consolidated live orderbook for trading.   So far have implemented OKCoin and been experimenting with the others to determine their API capabilities.

With the exception of OKCoin, what I've found so far is not good.  Here is a summary of the top-4 exchanges w/ respect to market data APIs (I also included Coinbase with the notion will become a top player):

![Status](/assets/2015-02-08/screen-shot-2015-02-08-at-9-33-28-am.png)

**BTCChina** is, by far, the largest exchange, however appears to have shoddy technology (at least on the market data side).  They implemented FIX 4.4 last Nov, however is broken in that requests for full OB fail to work as documented (if anyone has had any success with this, please let me know).   BTCChina has an alternative public WebSocket/JSON API which provides just 5 orderbook levels.  However, there appears to be a "secret" API which provides full depth-of-book, as real-time UIs ( such as bitcoinwisdom) show activity beyond 5 levels (if anyone knows how this works, please let me know).  **Addendum**: I have reached out to BTCChina, hopefully they will address the FIX depth-of-book subscription issue and not treat it as a feature.

**OKCoin** is the 2nd largest exchange by volume and appears to have the most active orderbook of the lot.  The good news is that its FIX API works as documented.   Activity-wise, have been receiving 20+ transactions every 100-300ms or 15ms between transactions on average.  This degree of activity rivals more traditional markets.   That said, have suspicions that the exchange or a market-making partner (with 0 transaction cost) is constantly pumping both sides of the market in the 1st 3-5 levels of the orderbook.   The pattern I saw was very shallow orders (1/10th BTC) on the 1st 3-5 levels of both sides of the market getting swept on either side alternatively.

**Bitfinex** seems to be the most popular BTC/USD exchange, and hence cannot be ignored, even though has a ill-advised market data API.   They are beta-testing AlphaPoint's "modern" exchange implementation.  Once that hits production should expect both FIX and binary streaming feeds.   I don't know whether these will provide a L2 (depth of book) or L3 (order transactions) view of the exchange.  Either would be very welcome.

**BitStamp** has fallen dramatically in terms of its share of BTC/USD volume.  Given its troubles, I wonder whether it will survive.   Though it has a "secret" API providing L3 data, the technology backing the exchange is suspect.  In particular, its matching engine has both [improper matching semantics](http://www.reddit.com/r/Bitcoin/comments/1r4d6t/bitstamps_streaming_api_and_exploitation/) (documented on reddit) and is **very slow**, where sweeping a few levels can take seconds to execute.   Short of Bitstamp reinventing itself both in terms of technology and trust, the question is then, who will replace them as the #2 presence in the BTC/USD market?

**Coinbase** or the yet-to-be launched Gemini, may be the successor to Bitstamp in terms of market position (or perhaps even take on Bitfinex).  With respect to market data, it provides a complete list of orders in the orderbook. **Update**: Coinbase has a streaming API & with level 3 data, my mistake.  <del>However this must be queried by polling their REST/json API.   As you can imagine, this is not a scalable approach - the # of orders will grow over time to the point where the message size will be overwhelming on a periodic frequency.   The right solution is a streaming API with order transaction "deltas" {new order, del order, update order, etc}.    The most basic design, scale, & accessibility mistakes have been repeated, and on a high profile exchange launch (regulated, NYSE-backed, etc).</del>

## Chinese Exchanges

It is hard to know what is real with respect to volume & dealability on the chinese exchanges.  I do not have any trading experience with either OKCoin or BTCChina, however, the following has been noted from multiple sources ([for example](http://www.reddit.com/r/BitcoinMarkets/comments/2swttr/okcoin_unusable_during_this_drop/)):


  * OKCoin: exchange not responsive to crossing orders during larger movements (DDOS on price movements)	
  * OKCoin & BTCChina: massive wash trading and spam orders (obviously house trading bot given not economical otherwise).  This resonates with what I've seen in the market data on OKCoin.


At least as a data source, I think these exchanges provide value.   More ideal would be to find a way to use for trading, and avoid situations where unlikely to be able to execute.   With respect to unresponsive crosses in larger price movements, the question is whether this is due to poor matching engine technology (where the OB may be overwhelmed)[1] or whether is being done intentionally "for the house"[2].   It seems very likely that the exchanges are trading on their own behalf in addition to providing exchange services.

Today there was also bad news, where a HK based exchange (MaiCoin) [disappeared](http://headlines.yahoo.co.jp/hl?a=20150208-00000055-jij-cn) with $3B HKD worth of deposits from customers.   I think there is enough fraud and poor market practices in the bitcoin space, that a certain level of regulation and regulated exchanges will be welcomed.

## Conclusions (technology)

My overall impression with (most of) the exchange Bitcoin technology is that it has been designed by the typical full-stack web app developer, taking few learnings from traditional markets or applying common sense with respect to scale.   Polling with REST/JSON is one of the dumbest ideas that is pervasive in Bitcoin exchanges (though for some uses is fine).   With this sort of exterior interface / design decision, one has to wonder what other marvels are present behind the scenes.

I am sure that these will mature, and via competition, will converge towards more sophistication and sensible design.   I would be happy to consult with these exchanges to move their APIs and implementations towards the state of the art, if for no other reason then to make these better venues for trading.

**Notes**

[1] Given the uninformed / ill-considered implementations have seen in the BTC space, would not be a surprise if most exchanges have deficient matching engines, with respect to order volume scaling.   Certainly Bitstamp does.

[2] The "for the house" scam would be to intentionally delay and front-run: in a market upswing, delay buying flow, buy in front of the buyer flow and sell to the latent buyers at a higher price.

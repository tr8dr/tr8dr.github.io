---
author: tr8dr
comments: true
date: 2015-02-11 23:59:43+00:00
layout: post
link: https://tr8dr.wordpress.com/2015/02/11/bitcoin-needs-cross-exchange-prime-brokerage/
slug: bitcoin-needs-cross-exchange-prime-brokerage
title: 'Bitcoin: Needs Cross-Exchange "Prime Brokerage"'
wordpress_id: 1094
categories:
- strategies
---

Ok, what I am going to say here is probably Bitcoin heresy, in that I am going to advocate more centralized clearing and management of assets wrt exchange trading.

I want to be able to scale trading in bitcoin and execute across multiple exchanges.  However have the following problems



	
  * lack of trust in (most) of the bitcoin exchanges	
    * security of the exchange against attackers
    * degree of trust in the ownership re: my assets on deposit	
  * inability to trade across a variety of exchanges on a net / credit basis
    * require depositing cash (fiat) and/or bitcoin on each exchange


## Solution

I want to keep my "working" fiat & bitcoin assets for trading with a trusted agent (a "Prime Broker" in the traditional trading space) who will grant me credit across a fairly wide range of exchanges.   To completely isolate both the trader and the prime broker from exchange risk:

  * exchanges should not hold trader's bitcoin or fiat
  * exchanges send/receive net fiat funds + cryptocurrency to/from Prime Broker based on net P&L + fees
  * reconciliation between exchange and PB done as a secure transaction between the two.  Could probably devise a protocol based on asymmetrical trust, which is near-transactional.  The element which is least transactional is the wiring of funds (amusingly).
  * much of the reconciliation can be done intra-PB or between PBs


Case in point, exchanges such as BTC-e trade BTC/USD at a 100-150bp discount to bitfinex.  Why?   I think this due to the perceived credit risk of depositing funds with BTC-e and possibly issues in moving assets to net.

Perhaps there is an even more interesting angle, PrimeBrokerage V2, using the bitcoin ledger, ascertaining trust, etc.    But for the moment, taking exchange risk out of the equation would open up the market further.

## Advantages

Above and beyond solving the trust issue, more centralization or a way to coordinate amongst exchanges could help with:

	
  * "locate": the process of finding cryptocurrency that can be borrowed for shorting
  * potentially offer a meta-exchange, where participants can interact with a cross-exchange orderbook, data, etc.


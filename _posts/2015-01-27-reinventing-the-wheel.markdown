---
author: tr8dr
comments: true
date: 2015-01-27 20:39:58+00:00
layout: post
link: https://tr8dr.wordpress.com/2015/01/27/reinventing-the-wheel/
slug: reinventing-the-wheel
title: Reinventing the Wheel
wordpress_id: 1030
categories:
- strategies
tags:
- bitcoin
- exchange
---

I guess I am old enough to have seen the wheel reinvented a number of times in both software infrastructure and financial technology spaces.  Unfortunately the next generation's version of the wheel is not always better, and with youthful exuberance often ignores the lessons of prior "wheels".   That said, we all know that sometimes innovation is a process of 3 steps forward and 2 steps backward.

Two examples that have impacted me recently.

**HDFS**

An example of a poorly reinvented wheel is Hadoop / HDFS in the technology space, where Amdahl's law has been ignored completely (i.e. keep the data near the computation to achieve parallelism).   When one writes to HDFS, HDFS distributes blocks of data across nodes, but with no explicit way to control of data / node affinity.   HDFS is only workable in scenarios where the data to be applied in a computation is <= the block size and packaged as a unit.

"Everybody and his brother" are touting big data on HDFS, S3, or something equivalent as the data solution for distributed computation, in many cases, without thinking deeply about it.   I was involved a company that was dealing with a big data problem in the Ad space recently.  I needed to track & model 400M+ users browsing behavior (url visits) based on ~4B Ad auctions daily, relate each URL to a content categories (via classification & taxonomy of concepts), and for each user, then determine a feature vector for each user.

I argued that using HDFS for this data would be a failure with respect to scaling.  The problem was that auction data, 4B records of <timestamp,user,url, ...>, was distributed across N nodes uniformly, rather than on userid MOD N.  HDFS does not allow one to control data / node affinity.  My computation was some function over all events seen in a historical period for each user.  This function:



	
  * F( [<timestamp,user1,urlA, ...>, <timestamp,user1,urlG, ...>, ...])


needed to be evaluated for each user, across all events for that user and could not be decomposed into sub-functions on individual events reasonably.    The technology I had to use was Spark over HDFS distributed timeseries.  I argued that to evaluate this function for each user, on average, (N-1)/N of the requested data with need to be transported from other nodes (as only 1/N of the data was likely to be local in a uniformly distributed data-block scheme).   The (lack of) performance did not "disappoint", instead a linear speedup (N x faster), produced a linear slowdown (approaching N x slower due to the dominance of communication).

**Bitcoin Exchanges**

Bitcoin popularized a number of excellent ideas around decentralized clearing, accounting, and trust via the blockchain ledger.  The core technology behind bitcoin is very innovative and is being closely watched by traditional financial institutions.   The exchanges, on the other hand (with some exceptions), seem to have been built with little clue as to what preceded them in the financial space.



	
  1. JSON / REST is popular as a web transport, but is not an efficient or precise transport for market data.

	
    1. clue: provide a Nasdaq ITCH like binary stream-based feed for efficiency and precision OR FIX if you insist.




	
  2. Some exchanges (like Bitstamp) have a super-slow matching engine

	
    1. sweeping the book takes multiple seconds to transact




	
  3. Provide orderbook updates as transactions and not top-K levels

	
    1. transactions (new order, delete order, update order, traded) are more compact, provide more information, & are timely.




	
  4. Provide keyframes

	
    1. i.e. provide an enumeration of orders in the orderbook on connection and perhaps periodically in the stream to bootstrap the subscribers view of the orderbook and validate later in the stream.






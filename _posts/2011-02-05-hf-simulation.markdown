---
author: tr8dr
comments: true
date: 2011-02-05 18:44:32+00:00
layout: post
link: https://tr8dr.wordpress.com/2011/02/05/hf-simulation/
slug: hf-simulation
title: HF Simulation
wordpress_id: 824
categories:
- strategies
---

Simulation for high-frequency strategies is, at best, a decent way to take one's strategy through its paces, perhaps giving a view on profitability.   In no way can it present an accurate view on profitability, rather at best can look at the strategy's performance under various synthetic assumptions.

Naturally our simulator must reflect:



	
  1. typical order event latency (perhaps with some jitter)

	
  2. typical market event latency

	
  3. accurate orderbook

	
  4. view on trade fills

	
  5. view on impact (icing on the cake, perhaps not necessary)




## The Data


There is just not enough information presented in market data to model with complete accuracy nor can we predict with certainty, the effects of our own trades on subsequent market events.   With access to a HF feed, we receive events such as:

[![](http://tr8dr.files.wordpress.com/2011/02/screen-shot-2011-02-05-at-11-40-51-am.png)](http://tr8dr.files.wordpress.com/2011/02/screen-shot-2011-02-05-at-11-40-51-am.png)

We are able to see most individual order activity.  Primarily, the operations are New, Update, and Delete order.    The exceptions are:



	
  1. hidden orders

	
  2. orders that immediately cross

	
  3. IOC orders (immediate or cancel)


What is shown or not shown may vary from venue to venue.   With the above we can reconstruct the orderbook as a set of price levels on the bid and another set on the ask.  Each price-level represents a queue of orders against which executions will occur on a FIFO basis.

The orderbook (abbreviated) might look something like this:


[![](http://tr8dr.files.wordpress.com/2011/02/screen-shot-2011-02-05-at-12-29-37-pm.png)](http://tr8dr.files.wordpress.com/2011/02/screen-shot-2011-02-05-at-12-29-37-pm.png)


A simulator using historical data needs to reconstruct the orderbook, maintaining proper FIFO behavior.   The two biggest problems in simulation are:



	
  1. determining when / if a trade happened

	
  2. determining market impact of strategy trades on historical data


With the various FX venues trades are shown selectively, and at that, with significant lag to the actual event.   Order (New or Update) events that would have crossed are also not shown.   This leaves us to guess when a trade is likely to have happened.   We can execute our strategy under various scenarios of trade fill.


## Possible Trade Events


Let's look at some possible signals that a trade may have occurred:



	
  1. all orders on our level are removed

	
    1. due to trade or cancellation?




	
  2. inside order level(s) removed

	
    1. due to trade(s) or cancellation?




	
  3. one of the above + our level has moved within epsilon of crossing

	
  4. our level is now crossing


None of these except (4) can indicate a trade with certainty, however without some assumptions about the first three scenarios would only leave us with aggressive crossing or waiting for price levels to collide.


## All orders on our level removed


Consider the above orderbook and the transformed below (where the removed orders are in red):

[![](http://tr8dr.files.wordpress.com/2011/02/screen-shot-2011-02-05-at-1-06-37-pm.png)](http://tr8dr.files.wordpress.com/2011/02/screen-shot-2011-02-05-at-1-06-37-pm.png)

All orders on our orderlevel of 82.709 on the ask have been removed in the historical events (except our synthetic order from the strategy we are testing).   The likelihood that a trade happened is higher if the # of orders on the level and/or the maximum size historically for the life of the level is high.   With one order on the level removed it may well have been a cancellation.

In this particular case we see the order on our level and the first order on the next level removed.   This strengthens a view that a trade may have occurred.

We can assign a probability of trade based on evidence like this (though if using a random / probability approach, ones results will differ with every run).


## Deeper Inside Level removed


Consider the orderbook below:

[![](http://tr8dr.files.wordpress.com/2011/02/screen-shot-2011-02-05-at-1-17-00-pm.png)](http://tr8dr.files.wordpress.com/2011/02/screen-shot-2011-02-05-at-1-17-00-pm.png)

Our order is on the inside level and the level(s) above it is either cancelled or taken out.  This is an even stronger indication of trade if more than one level taken out.   Levels can disappear quickly from a number of possible stimuli:



	
  1. traded: large buying algorithm buys in size (this happens quite often during the day)

	
  2. moved: level primarily lead by market makers and activity elsewhere demands changing the offering

	
  3. risk aversion: pending event or volatility cause pullback


#2 is harder to gauge perhaps, but #1 and #3 are relatively easy to see in retrospect.


## Order Moves Close to Crossing


We may have a passive order that drifts towards crossing (or is placed very close to crossing):

[![](http://tr8dr.files.wordpress.com/2011/02/screen-shot-2011-02-05-at-1-28-03-pm.png)](http://tr8dr.files.wordpress.com/2011/02/screen-shot-2011-02-05-at-1-28-03-pm.png)

We see that our order is 2/10ths pips away from the cross.  On some venues there may be inside-of-inside hidden orders (for venues that don't require partial exposure of size).  In those cases, a move that tight is likely to get executed right away.   Market makers with inventory may find a cross of 2/10ths appealing as well.


## Other Trade Signals?


We did not look at what was happening on the other side of the book, but could possibly correlate activity on the other side and activity in the above scenarios to determine a stronger likelihood of a trade.   I'd appreciate ideas.

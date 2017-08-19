---
author: tr8dr
comments: true
date: 2011-03-04 23:58:50+00:00
layout: post
link: https://tr8dr.wordpress.com/2011/03/04/detecting-price-direction-in-order-flow/
slug: detecting-price-direction-in-order-flow
title: Detecting Price Direction in Order Flow
wordpress_id: 862
categories:
- strategies
---

As indicated in a previous post, high-freq market data is typically in the form of order updates, for example:

[![](http://tr8dr.files.wordpress.com/2011/02/screen-shot-2011-02-05-at-11-40-51-am.png)](http://tr8dr.files.wordpress.com/2011/02/screen-shot-2011-02-05-at-11-40-51-am.png)

This can be used to reconstruct the orderbook at any given instant, but can also be used to analyze movements within the orderbook.   The most primitive approaches look at the incidence of size being removed or hitting a given price-level.    More sophisticated approaches try to determine order streams to see where an order is being moved to in the book.

Given that we can produce a variety of measures that can add information to our trading decisions.   Here is an example of a USD/JPY on a downward move of 10 pips with strong momentum.   The mid price is in the upper pane and the cumulative bid and ask movements (derived from order data) are in the lower pane:

[![](http://tr8dr.files.wordpress.com/2011/03/screen-shot-2011-03-04-at-6-20-00-pm.png)](http://tr8dr.files.wordpress.com/2011/03/screen-shot-2011-03-04-at-6-20-00-pm.png)

One will note that the ask flows are more aggressive than the bid flows.   The above is not yet terribly useful, as the cumulative flows look to follow the price path more or less during a period of momentum.

**Momentum**
More interesting is to look at what is happening in the book:

[![](http://tr8dr.files.wordpress.com/2011/03/screen-shot-2011-03-04-at-6-23-40-pm.png)](http://tr8dr.files.wordpress.com/2011/03/screen-shot-2011-03-04-at-6-23-40-pm.png)

The reds represent areas of high cancellation and blues high insertion (I amplify this by scaling by the size of movement from one region to another).    You'll notice the following:



	
  1. **Seller Book
**There is high movement into the inside (or more aggressive) areas of the book, in-line with the fact that the price has downward momentum.   There are vertically "trailing" deletions from the outside price levels as traders move their prices en-mass to the inside.

	
  2. **Buyer Book
**Buyers / market makers are risk adverse and are moving their orders deeper into the book away from the inside.  Again we see a trail of deletions in the inside-most levels and strong movements in a couple of deeper bands.


**Sideways Movement
**What does "sideways" or non-directional movement look like then?

[![](http://tr8dr.files.wordpress.com/2011/03/screen-shot-2011-03-04-at-6-44-01-pm.png)](http://tr8dr.files.wordpress.com/2011/03/screen-shot-2011-03-04-at-6-44-01-pm.png)

[](http://tr8dr.files.wordpress.com/2011/03/screen-shot-2011-03-04-at-6-44-01-pm.png)It looks a lot more random, with some upward and downward directional patterns in sequence.

**Cleaning It Up**
The activity can be denoised to produce a signal that is much easier to read:

[![](http://tr8dr.files.wordpress.com/2011/03/screen-shot-2011-03-04-at-6-52-58-pm.png)](http://tr8dr.files.wordpress.com/2011/03/screen-shot-2011-03-04-at-6-52-58-pm.png)



**Summary**
The approach is particularly useful for understanding movements over a short to medium timeframe.   However long-running price movements often have periods of "indecision" where the flow and price flounders for a period.   Ultimately this sort of approach needs to be evaluated on multiple windows to accomodate various timeframes.

(sorry this is mostly pretty pictures without much explanation.  Should provide some ideas though).



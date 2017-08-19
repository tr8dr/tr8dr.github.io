---
author: tr8dr
comments: true
date: 2009-09-30 10:48:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/09/30/determining-whether-a-movement-is-mean-reverting/
slug: determining-whether-a-movement-is-mean-reverting
title: Movement is mean reverting?
wordpress_id: 33
---

Can look at the statistics of past reverting cycles in the market to determine an average behavior of mean reversion?   Simply characterizing cycle amplitudes and durations is not enough to make a successful strategy.

Formalizing in a SDE is a step in the right direction, but ignores additional information available.   Of course, even modeling as a SDE is a challenge.   Mean-reversion is surely not a constant.

When is a movement mean reverting and when is it a movement in the direction of a new price level?   When has the movement exhausted its direction?

I have not fully studied this, but would look at the following:



	
  * changes in the intensity of the buy and sell processes

	
  * changes in order book complexion (skew, size, etc)

	
  * lack or presence of news event

	
  * speed of ascent / descent (how do we distinguish period aggressive execution from sustained)


I will revisit this topic soon.


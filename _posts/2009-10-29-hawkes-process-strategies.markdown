---
author: tr8dr
comments: true
date: 2009-10-29 22:33:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/10/29/hawkes-process-strategies/
slug: hawkes-process-strategies
title: Hawkes Process & Strategies
wordpress_id: 35
categories:
- point processes
- statistics
- strategies
- volatility
---

Call me unread, but I had not encountered the Hawkes process before today.   The Hawkes process is a "point process" modeling event intensity incorporating empirical event occurrence.

The discrete form of the process is:
[![](http://tr8dr.files.wordpress.com/2009/10/picture1.png?w=240)](http://tr8dr.files.wordpress.com/2009/10/picture1.png)
where ti is the ith occurrence at time ti < t for some t.   The form of the function is typically an exponential, but can be any function that models decay as a counting process:
[![](http://tr8dr.files.wordpress.com/2009/10/picture2.png?w=202)](http://tr8dr.files.wordpress.com/2009/10/picture2.png)
Ok, that's great but what are the applications in strategies research?

Intra-day Stochastic Volatility Prediction
The recent theme in the literature has been to replace the quadratic-variance approach with a time-based approach.   The degree of movement within an interval of time is equivalent in measure to the amount of time required for a given movement, and can be interchanged easily as  Andersen, Dobrev, and Schaumburg have shown in "Duration-Based Volatility Estimation".

Cai, Kim, and Leduc in "A model for intraday volatility" approached the problem by combining an Autoregressive Conditional Duration process and a Hawkes process to model decay, showing that:
[![](http://tr8dr.files.wordpress.com/2009/10/picture4.png?w=300)](http://tr8dr.files.wordpress.com/2009/10/picture4.png)
and then equivalently expressed in terms of intensity (where N represents the number of events of size dY):
[![](http://tr8dr.files.wordpress.com/2009/10/picture5.png?w=300)](http://tr8dr.files.wordpress.com/2009/10/picture5.png)

relating back to volatility measure as:
[![](http://tr8dr.files.wordpress.com/2009/10/picture6.png?w=152)](http://tr8dr.files.wordpress.com/2009/10/picture6.png)
The intensity process is comprised of an ACD part and a Hawkes part:

![Picture 1](http://tr8dr.files.wordpress.com/2009/10/picture-1.png)
[![](http://tr8dr.files.wordpress.com/2009/10/picture21.png?w=300)](http://tr8dr.files.wordpress.com/2009/10/picture21.png)

They claim to model the intra-day volatility closely and propose a long/short straddle strategy to take advantage of the predictive ability.

High Frequency Order Prediction Strategy
The literature suggests the use of Hawkes processes to model the buying and selling processes of market participants.

John Carlsson in "Modeling Stock Orders Using Hawkes's Self-Exciting Process", suggests a strategy where if the Hawkes predicted ratio of buy/sell intensity exceeds a threshold (say 5) buy (sell) and exit position within N seconds (he used 10).

This plays on the significant autocorrelation (ie non-zero decay time) of the intensity back to the mean.  A skewed ratio of buy vs sell orders will surely influence the market in the direction of order skew.

The strategy can be enhanced to include information about volume, trade size, etc.   We can also look at the buy/sell intensity of highly correlated assets and use to enhance the signal.

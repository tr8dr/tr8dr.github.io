---
author: tr8dr
comments: true
date: 2010-02-11 22:20:22+00:00
layout: post
link: https://tr8dr.wordpress.com/?p=596
published: false
slug: co-what-amongst-assets
title: Co-"what" amongst assets
wordpress_id: 596
categories:
- strategies
---

I've been thinking about the relationships amongst a network of assets.   Supposing I have a network of hundreds of assets, what sort of measurements can be made that allow for statements about the future state with a measurable degree of confidence.

[![](http://tr8dr.files.wordpress.com/2010/01/group1.png)](http://tr8dr.files.wordpress.com/2010/01/group1.png)

Here are some "standard" approaches to looking at the relationship between assets:



	
  1. **Covariance**
Covariance is a linear measure, the normalization of which is literally the slope of a least-squares regression line through paired data.  Has issues with lagged series and assumes linearity, also only uniquely specifies elliptical distributions.

	
  2. **Cointegration
Cointegration is a measure of relationship between series using an autoregressive error correction model.   It avoids many of the issues of Covariance, however, like covariance is not sufficient to uniquely specify the joint distribution.   The VECM model and Johansen method give robust estimates of this.
**

	
  3. **Distribution Estimating Models
Models that estimate the distribution (i.e. provide the most probable price movement in the next sample period based on an estimated high-dimensional distribution); works well on certain portfolios (I'm sorry but I cannot discuss this further).
**

	
  4. ****SDEs
SDEs that impose a structure / mechanism of price movement, implying future price movements.   These models often combine points 1 and 3.   I like most other people in this space have a collection of these, some better than others. ****


I am interested in a new model based on cointegration -- not cointegration between pairs, but using the strong error-correcting relationships in a network of assets to determine likely next period moves.

**The Parts**
Much in the way one can determine the behavior of 1 asset with respect to another in an ECM with impulse response analysis, can made use of the derivatives to determine the rippling effect of movements within the network.    With an ECM we get quite a bit of information at each edge:



	
  1. average period of mean reversion (from the eigenvalues)

	
  2. dP1/dP2 from the eigenvectors

	
  3. measurement error from evaluating the variance of dP1/dP2 over time

	
  4. degree of confidence in cointegration

	
  5. positive or negative feedback from other edges


**Algorithm: Initialization**
The approach is as follows:



	
  1. Find all pairs with various degrees of cointegrating relationships

	
  2. Observe variance of dP1/dP2 over time

	
  3. Observe past optimal SD and cumulative-time bands

	
  4. Set up network of relationships


**Algorithm: Metrics**
The following metrics are observed / updated period to period:



	
  1. average period of mean-reversion cycle

	
  2. average period above noise band

	
  3. dP1/dP2 measurement noise

	
  4. variance of mean-reversion time period


**Algorithm: Determining what to trade**
The approach is as follows:



	
  1. Observe all pairs that are trading outside of a low noise band

	
  2. Calculate dS/dt + [-2sd, 2sd] based on distance from 0 and expected time until 0 crossing

	
  3. determine the largest dS/dt pairs subject to variance

	
  4. determine trades based on network analysis


**Network Analysis
Let's evaluate some networks to get an intuitive feel for what is happening:**


** **




[![](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-17-at-5-32-10-pm.png)](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-17-at-5-32-10-pm.png)


In the network above it is highly likely that A1 is the mover.   A3 is unlikely to move because is "locked" in tight relationships with {A4,A5,A6}, all of which are showing very little flexibility and {A2,A1} has flexibility to move.


[![](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-17-at-5-49-03-pm.png)](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-17-at-5-49-03-pm.png)


In the network below we can see that {A7,A4} and {A1,A3} are the largest pairs.   These present flexibility in the graph such that the subgraphs {A7,A2,A1} or {A4,A3,A5,A6} could end up moving, largely as a whole.   It is hard to predict how the subgraphs will move or which one of them will.   In this scenario, trading the pairs is more prudent.

**Network Algorithm**
Given the above, here is an algorithm:



	
  1. node flexibility = mean of |dS/dt| of edges incident on a node

	
  2. decide on pair or individual node based on disparity or similarity of flexibility


Alternate algorithm:

	
  1. take all nodes in largest pairs into a VECM model

	
  2. determine eigenvectors / values to determine strongest components




** **


**[](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-16-at-11-34-06-pm.png) **

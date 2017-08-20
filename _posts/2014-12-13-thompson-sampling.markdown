---
author: tr8dr
comments: true
date: 2014-12-13 17:47:13+00:00
layout: post
link: https://tr8dr.wordpress.com/2014/12/13/thompson-sampling/
slug: thompson-sampling
title: Thompson Sampling
wordpress_id: 986
categories:
- strategies
---

I recently attended a talk by David Simchi-Levi of MIT, where he discussed an approach to online price discovery for inventory, to maximize some objective, such as profit.   The scenario was where one could observe whether inventory was sold or not-sold to for each potential buyer in the marketplace, giving an empirical view of demand-behavior as a function of price.   The optimal setting in selling the inventory is one that  maximizes price x liquidation probability.

When we have no knowledge about the true demand for inventory as a function of price, we must start with some prior estimate in the form of a sold/unsold distribution (the demand function in terms of price) and modify this online during the selling period.   The setup of the problem is then:


  * have a fixed period in which can sell the inventory
  * can observe whether a prospective buyer bought at offer price or rejected the price as too high
  * determine a number of different selling price levels & some prior on their respective probability of sale at each price, starting with a uniform distribution.


This can be modeled as the 1-armed bandit problem, where we decide amongst n bandits  (slot machines), determining which slot machine gives the highest payout through iteratively adjusting our model based on observations.

In determining the optimal price, can formulate  as a sequence of bandits: a sequence of prices ranging from low to high.  Associated with each price is a distribution which represents our view on the demand associated with this price (i.e. the probability of sale).  With no prior, can start with an equi-probable view on demand across prices, starting with an initial Beta distribution on at each price level of B(α=1,β=1).

Rather than provide the formal setup for Thompson sampling, will illustrate its use for the above problem.  The sold/unsold distributions & selection of price can then be evolved as follows:

	
  * take a sample from each distribution associated with a price (representing the probability of sale at a each given price)
  * choose the ith price (1-armed bandit) that maximizes the objective: p(sale[i]) * price[i], where p(sale) is the sampled probability from each demand/price distribution
  * offer the ith price to the marketplace
  * the unit of inventory is observed to be sold or unsold
  * adjust the beta distribution associated with price[i] (the price used in the offer) as follows:
    * if sold:  B' = B (α+1, β)
    * if unsold: B = B(α, β+1)


The net effect over time is that the distributions will converge such that the optimal bandit (associated with optimal price) will offer the highest probability in sampling or at least the highest expectation (price x p(sold)).  The approach (Thompson Sampling) has many applications in finance for online optimisation.  Definitely going to be part of my toolset going forward.

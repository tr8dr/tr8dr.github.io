---
author: tr8dr
comments: true
date: 2008-01-20 08:19:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2008/01/20/gp-for-option-pricing/
slug: gp-for-option-pricing
title: GP for option pricing
wordpress_id: 19
categories:
- genetic algorithms
- pricing
- statistics
---

As you probably know GP (Genetic Programming) is an extension of GA which rearranges algebraic or functional instruction trees to fit to a solution.  
  
[![](http://upload.wikimedia.org/wikipedia/en/7/77/Genetic_Program_Tree.png)](http://upload.wikimedia.org/wikipedia/en/7/77/Genetic_Program_Tree.png)  
I had not thought of it previously, but could use such an approach with the right set of functional constructors to converge on an option pricing GP.   Now if all we were trying to do was to replicate the Black / Scholes, CEV, or other gaussian distribution based model, would not be very interesting.  
  
We know that the actual distribution are often non-gaussian.  Could we produce a more accurate approximation of the hedging cost against a non-gaussian distribution (implying the true risk free price of the option) with GP?  
  
Interestingly, Neural Networks are just special cases of a GP tree, so in the end GP is the most general approach to non-linear regression.

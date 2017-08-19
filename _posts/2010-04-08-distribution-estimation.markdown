---
author: tr8dr
comments: true
date: 2010-04-08 01:49:29+00:00
layout: post
link: https://tr8dr.wordpress.com/2010/04/07/distribution-estimation/
slug: distribution-estimation
title: Distribution Estimation
wordpress_id: 640
categories:
- strategies
---

I've been travelling for the last 3 weeks so have not had much time to post.   During the trip, I've been thinking further about the following problem:



	
  1. We are interested in determining a representative distribution for some financial factor

	
  2. suppose we start with start with a "universal" high-dimensional joint distribution of all random variables that might possibly have relevance to the probability of some financial factor

	
  3. suppose that the number of variables in the universal joint distribution is high enough that the distribution is sparse relative to the sampled data.  We need to reduce the number of degrees of freedom.

	
  4. Is there a smaller marginal distribution (a subset of variables) that provides representative modes and distribution shape?

	
  5. How do we determine it?


Some observations:

	
  1. We should expect clustering around 1 or more modes

	
  2. Variables with little impact will not introduce new modes or substantially alter the shape of the distribution

	
  3. Adding a new low-impact variable (dimension) should just stretch the existing modes uniformly along the new dimension


This brings to mind a brute-force approach that would involve:

	
  1. observing all possible marginal distributions

	
  2. applying a measure to each, determining the degree of information within, select one that maximizes information and penalizes for number of variables


The first step can possibly be shortcut by reducing incrementally, but may not find the global optimum.   The notion of "information" also needs to be defined.   I'll post more later on this.

Another approach would be to formulate as an expectation maximisation problem, but I have not worked out how this would be done.

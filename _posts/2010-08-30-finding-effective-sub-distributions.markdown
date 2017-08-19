---
author: tr8dr
comments: true
date: 2010-08-30 00:34:16+00:00
layout: post
link: https://tr8dr.wordpress.com/2010/08/29/finding-effective-sub-distributions/
slug: finding-effective-sub-distributions
title: Finding Effective Sub-distributions
wordpress_id: 697
categories:
- strategies
---

I didn't get around to talking about dimensionality reduction in the previous post.   I have a number of related problems I am interested in solving.    Setting up the problem, let us say that we have a distribution **p (E | C)**, where both **E** and **C** are sets of the same dimension (d):


E = {e1, e2, e3, ... e[d]}
C = {c1, c2, c3, ... c[d]}


**d** is a high dimension >> 1000.

**Problem 1: Sub-distributions with C and E of same dimension
**In the case where many of the variables in the dimension are independent, the joint probability of  a specific 1000 variable state, will often be much less than the probability of a lower dimensional state variable (of say 20 variables with covariance).

The question is then, what dimensional subsets of this distribution are the most predictive.   Another way to state this is what subsets of **C** provide the most information transfer into  **E**.

It would be then of interest to look at the top N subsets of the distribution that carry the most information from **C** → **E**.

**Problem 2: Sub-distributions with C and E of different dimension
**As in problem 1, we look for subsets of **C** that carry the most predictive information for subsets of **E**.   In this case, though, C and E need not be of the same dimension.  Combinatorially this is a harder problem than problem #1, however the math is the same.

**Outline of Approach**

** **



	
  1. spanning graph structure is determined between variables to avoid astronomical combinations

	
  2. transfer entropy analysis is done on edges in spanning structure

	
  3. subgraphs of depth 1, 2, 3, ... are evaluated from a transfer entropy point of view to determine the power of each subgraph

	
  4. subgraphs are sorted by transfer entropy 


**Alternatives
**There are undoubtedly many approaches to finding factors, particularly with classification problems where E is of small dimension (often 1 dimension).   I'd be grateful for alternative suggestions.

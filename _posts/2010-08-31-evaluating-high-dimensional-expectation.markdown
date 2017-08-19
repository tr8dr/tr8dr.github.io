---
author: tr8dr
comments: true
date: 2010-08-31 23:45:34+00:00
layout: post
link: https://tr8dr.wordpress.com/2010/08/31/evaluating-high-dimensional-expectation/
slug: evaluating-high-dimensional-expectation
title: Evaluating High Dimensional Expectation
wordpress_id: 723
categories:
- strategies
---

Here I am going to outline a simple approach to evaluating a high dimensional discrete expectation on empirical data.   For example, if one considers the evaluating an expectation like:


[![](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-31-at-5-33-37-pm.png)](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-31-at-5-33-37-pm.png)


Let's say X is of dimension 10 and we have a 10000 x 10 matrix of observations.   Furthermore each of the 10 variables can take 5 possible states:


X0 = { 1,4,4,1,1,1,1,3,3,2 }
X1 = { 1,2,2,2,1,1,1,5,5,2 }
....


The total number of possible distinct state vectors is 5^10 or over 9 million.   A naive approach to the above integral is to consider all possible sequences of X, but this would be enormously expensive and will not scale as the dimension or number of individual states grows.

The distribution is going to be sparse.  We know this because 5^10 >> 10,000 observations.   The worst case algorithm would involve a scan for a given pattern, calculating the frequency.   This poor approach would have complexity  **O(N S^d)**, where **N** is the number of samples, **S** is the number of distinct symbols, and **d** is the dimension.

Some simple observations:



	
  1. It makes sense to restrict the domain of the integral to existant sequences (get rid of complexity S^d)

	
  2. It makes sense to avoid multiple passes, where naively there would be a pass for each frequency calculation


Since we are dealing with a finite sample and known symbols there is an approach that is **O (N log N)**.   We create a binary or n-nary tree ordered lexically by symbol sequence.   Iterating through the samples, for each sample locate or insert a node in the tree, tracking its frequency.   Increment frequency.

[![](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-31-at-7-41-11-pm.png)](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-31-at-7-41-11-pm.png)

The tree is rearranged as necessary to create balance and reduce the average path length to log N.  In one pass we now know both the domain of the distribution and the frequencies.   The expectation is now straight-forward to evaluate.

Note that instead of using a tree, one can use a hash-table keyed off of sequences.   This will result in an O(N) algorithm.   For some applications, though, it is useful to have a tree that places neighbors close to each other (for instance if we want to run a near neighbor analysis).

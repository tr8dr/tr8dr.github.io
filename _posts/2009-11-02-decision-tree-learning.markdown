---
author: tr8dr
comments: true
date: 2009-11-02 01:33:45+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/11/01/decision-tree-learning/
slug: decision-tree-learning
title: Decision Tree Learning
wordpress_id: 86
categories:
- machine-learning
---

Some time ago began using bayesian networks (decision trees with conditional relationships) to boost signal for strategies, the idea being that a combination of related observations can be combined conditionally to provide a posterior conclusion with a higher degree of confidence.

In the context of trading our bayesian network tells us:



	
  1. given the coincident set of events should we trade and if so, in what direction

	
  2. with what degree of confidence


My approach up until now had been to carefully construct the relationships.  It requires painstaking research and a lot of trial and error.   Why not use an algorithm to assemble / find relationships, and classify data.

**General Approaches**
The more general decision tree algorithms allow me to present many factors (even with high dimension), determine which factors have the highest degree of information relative to our classification targets (buy, sell, don't-trade), and formulate a classification or regression that models this.

Key to the assembly on the tree are measures taken from information theory.  We want to make arrangements in the tree such that the "entropy" of the tree is minimized or in other words information is maximized.   This is often calculated with a discrete form of the [Kullback-Leibler divergence metric](http://en.wikipedia.org/wiki/Kullback–Leibler_divergence).

**Algorithm**
In particular the "[Random Forest](http://en.wikipedia.org/wiki/Random_forest)" approach is quite appealing.   Multiple decision trees are constructed against training subsets drawn randomly from a larger training set.   These ultimately produce many variations on the "true" model.   The approximation to the true model is made by observing the mode (similar to a robust mean / expectation) across the random trees.

Taking advantage of the classification ability of the algorithm is going to allow me to try many new inputs in my strategies without the huge research overhead and without the near-intractable multivariate optimization required in other approaches.   I'll post some results on this soon.

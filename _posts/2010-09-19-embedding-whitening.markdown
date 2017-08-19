---
author: tr8dr
comments: true
date: 2010-09-19 20:10:23+00:00
layout: post
link: https://tr8dr.wordpress.com/2010/09/19/embedding-whitening/
slug: embedding-whitening
title: Embedding & Whitening
wordpress_id: 787
categories:
- strategies
---

A reader asked about why we need to normalize differential entropy to remove autocorrelation (and cross-correlation).   I should have said, the whitening is trying to establish the difference (or ratio in this case) between the entropy of the series with autocorrelations / cross-correlations removed and the series with the correlations.

In fact, we are trying to isolate the correlations because this is where the mutual information is, the rest is the ambient noise and variance of the data set.

With the embedding vector we are trying to find a vector series that maximizes correlations.   Ok, so why do we need the normalization?

In searching for the appropriate embedding dimension and lag, we vary these quantities.  Increasing embedding dimension, for instance, is adding new variables to the system.

So for instance, let us say we have a system of 2 variables **A** and **B**, with a timeseries of { {A0,B0}, {A1,B1}, ...} pairs.   Our embedding vector starts at dimension 2 { A[t], B[t] }.   Increasing the dimension, we have { A[t], B[t], A[t-1], B[t-1] } and so on.

Having added A[t-1], B[t-1], we may have increased the correlation between neighbors, but cannot be certain if we just observe the change in entropy without a relative measure for the same system variables.   We may have increased or decreased entropy just by adding a new dimensions, in addition to the correlation effect.

The goal of normalization is then to determine the contribution of entropy introduced by the new variables, without any correlations, and compare that to the possible entropy reduction produced by increased correlation between neighbors.

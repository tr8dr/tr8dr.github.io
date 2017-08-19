---
author: tr8dr
comments: true
date: 2010-08-30 19:17:52+00:00
layout: post
link: https://tr8dr.wordpress.com/2010/08/30/multivariate-independence-tests/
slug: multivariate-independence-tests
title: Multivariate Independence Tests
wordpress_id: 707
categories:
- strategies
---

Thought I should mention the obvious multivariate independence tests.   These can be used in a 1st pass to trim out the obviously independent variables.   Assume a d-dimensional joint distributions **p(x0, x1, x2, ... xd)** as a starting point with sample vector X = { x0, x1, x2, .. xd }.

Let us consider a subdividing of the above **d** dimensional distribution into **n** and **m** dimensional distributions respectively (where d = n+m).  We want to test to see whether the marginal distribution of **n** variables is independent of the distribution of **m** variables.   Our sample vector for the full joint distribution can be expressed as:


X = { {a1,a2, ..an}, {b1,b2,...,bm}}


and we have marginal distributions **p(A)** and **p(B)** each of which is a subset of the dimensions of **p(X)**.   The covariance matrix for **p(X)** looks like:


[![](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-30-at-2-46-32-pm.png)](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-30-at-2-46-32-pm.png)


Intuitively if A and B are independent then Σab should be 0 (well not significantly different from 0).   We set up the null hypothesis H0 to indicate independence.   The wilks' test rejects independence if:


[![](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-30-at-3-13-57-pm.png)](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-30-at-3-13-57-pm.png)


where N is the number of samples.   This test is valid even for non-elliptical distributions, so is a good 1st line test.   The intuition with the above measure is that the determinant ratio is measuring the volume of the covariance matrix in ratio to the volumes of the non-crossing components of covariance.   If the volume is mostly explained by the non-crossing components, then A and B are independent.

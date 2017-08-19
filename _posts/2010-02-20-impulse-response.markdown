---
author: tr8dr
comments: true
date: 2010-02-20 17:10:16+00:00
layout: post
link: https://tr8dr.wordpress.com/2010/02/20/impulse-response/
slug: impulse-response
title: Impulse Response
wordpress_id: 618
categories:
- strategies
---

This is just a quick note on deriving an impulse response function for a VECM system.   Basically we want to get the system into a form where we can take the partial derivatives at various lags.   Starting with a simplified VECM:


[![](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-20-at-11-40-57-am.png)](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-20-at-11-40-57-am.png)


[](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-20-at-11-40-57-am.png)Convert this into a form expressing in terms of X instead of ΔX:


[![](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-20-at-11-42-10-am.png)](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-20-at-11-42-10-am.png)


[](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-20-at-11-42-10-am.png)We change variable to simplify the form:


[![](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-20-at-11-44-05-am.png)](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-20-at-11-44-05-am.png)


[](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-20-at-11-44-05-am.png)Via Pesaran and Shin (1996) we transform this into the following recursive expression:


[![](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-20-at-11-47-18-am.png)](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-20-at-11-47-18-am.png)


We determine the partial derivative of ∂vj / ∂vk  (i.e. the impact of a change in the kth variable on the ith) after n time periods (t+n) to be:


[![](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-20-at-11-53-29-am.png)](http://tr8dr.files.wordpress.com/2010/02/screen-shot-2010-02-20-at-11-53-29-am.png)


where Si is a selection vector with 1 at the ith position and 0 elsewhere.

Normally the cholesky decomposition is used to orthogonalize the covariance (U U' = Σ), however other decompositions can be used, providing different measures of  response such as the Bernanke-Sims approach.

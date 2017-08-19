---
author: tr8dr
comments: true
date: 2009-09-12 23:51:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/09/12/cointegration-models/
slug: cointegration-models
title: Cointegration Models
wordpress_id: 29
---

A colleague had asked if I could help develop a multi-factor cointegration model for the Canadian bond market on daily or more frequent sampling, based on a variety of market data and fundamental factors.  I had not developed a model like this before and was skeptical that could produce a useful result short of some man years of research.  
  
To my surprise, found a very high probability model with 95% R-squared values and very high significance in a variety of tests.   Now have a variety of models based on it depending on all or some of the below:  


  * US 3m rates
  * US 2y swap rates
  * S&P 500
  * S&P / TSE Composite  

  * Shanghai Composite index (SSE 300)
  * Momentum
  * CAD/USD fx rate
  * CAD 5Y liquid bond
  * Surprise Index  

With the 2 variable cointegration, one is simply trading mean reversion on the spread between one security and another.   With a multivariate cointegration, one trades a long or short basket against the cointegrating security.

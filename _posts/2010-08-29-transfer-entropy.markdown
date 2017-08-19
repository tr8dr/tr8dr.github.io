---
author: tr8dr
comments: true
date: 2010-08-29 20:40:32+00:00
layout: post
link: https://tr8dr.wordpress.com/2010/08/29/transfer-entropy/
slug: transfer-entropy
title: Transfer Entropy
wordpress_id: 682
categories:
- strategies
---

I am revisiting spanning trees & clusters that express relationships amongst assets.   I am also interested in a related problem: reducing dimensionality in a high-dimensional distribution of asset returns.

[![](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-30-at-12-56-59-pm.png)](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-30-at-12-56-59-pm.png)

**Linear Approaches
**The most naive approach is to look at the correlations amongst time-series.   Correlations have a number of well-known problems for this purpose:



	
  1. correlation ≠ causality

	
  2. correlations can occur between completely unrelated variables for arbitrary sample periods

	
  3. correlations are a linear measure of similarity


The VECM model offers significant improvement over the correlation approach, at least in terms of identifying causality.   For those unfamiliar with VECM it is similar to an ARMA model, but extended to include lags from the other timeseries.   For 2 variables, a lag-p VECM would be set up as:


[![](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-29-at-2-29-29-pm.png)](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-29-at-2-29-29-pm.png)


That is Δx is described in terms of lagged Δx's and lagged Δy's.   Solving for this (usually in matrix form), one arrives at coefficients (assuming statistical significance) which on-average describe the interactions between the series X and Y.

Taking this a step further, one can do the "Granger Causality test", doing a F-test to determine whether Δx with cross-lags produces significantly less error variance than Δx without cross-lags.   This is performed for various lags to determine the minimum number of lags for which there is "causality" (or none at all).

This is not a bad approach for normally distributed returns, but is flawed for data with non-linearities.

**Information Theory
**It turns out information theory provides a powerful tool in analyzing causality (or at least temporal flows of information from one market to another).

Shannon measured the information for a particular event "e" as:


[![](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-29-at-2-56-56-pm.png)](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-29-at-2-56-56-pm.png)


Let us associate a symbol with each possible distinct event in a system {A, B, C, ... }.  A sampling of these events across time will lead to a sequence of symbols (for example:  ABAAABBBBABABAA).   If the symbol B occurs with p(B) = 1 (i.e. BBBBBBBB), the sequence can carry no information as can only represent 1 state.   Note that I(B) would be 0 in this case.

Shannon went on to define the entropy of the system as the expected information content:


[![](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-29-at-3-44-54-pm.png)](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-29-at-3-44-54-pm.png)


This can be extended to look at the joint entropy of for 2 or more "symbol" generators such as:


[![](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-29-at-3-49-31-pm.png)](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-29-at-3-49-31-pm.png)


Observing that if X and Y are independent, p(x,y) = p(x)p(y), we can determine how much information has been introduced into the joint event space versus the amount of information were the two sequences independent as:


[![](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-29-at-4-02-49-pm.png)](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-29-at-4-02-49-pm.png)


The above is called the "Mutual Information" measure.   This measure does not differentiate between information X provides Y or Y provides X.   In the context of finance it is useful to know more about the directional flow of information than that they simply share information.

**Transfer Entropy**
Transfer Entropy is a more precise measure than Mutual Information in that it captures information flow direction and temporal relationship.  The Transfer Entropy approach is a nearly 1:1 analog with Granger causality, except that it is applicable for a wider range of systems (as it turns out granger causality and transfer entropy have been shown to be equivalent for data with normally distributed noise).

Like Granger Causality (GC), we look at the entropy (or in the case of GC: error variance) with and without an explanatory variable from the other series.   For a single lag, this results in the following measure:


[![](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-29-at-4-21-53-pm.png)](http://tr8dr.files.wordpress.com/2010/08/screen-shot-2010-08-29-at-4-21-53-pm.png)


The above expressed the transfer entropy of y[t] on x[t+1], i.e. how much impact does y[t] have on x[t+1].   Changing the conditional probabilities to express p(y[t+1] | x[t], y[t]) would allow us to explore the other direction.   Of course this can be evaluated for more lags (the above is just for 1 lag).

Finally one needs to consider the level of significance for a given transfer energy to understand at which point there is no further relationship when looking at past lags or other variables.   The approach taken is to measure the baseline entropy in a shuffled series (one that removes the correlations but maintains the symbols and marginal frequencies).

This approach is much more robust than granger if the data set one is working with has non-linearities.

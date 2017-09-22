---
author: tr8dr
comments: true
date: 2017-08-21 17:00:00+00:00
layout: post
title: Information in the Volatility Surface [part 1]
wordpress_id: 1126
categories:
- data
- strategies
---

I've developed signals based on the "spot" market, but had not really explored the options market as a source of information.  In particular want to look at discrepancies in option demand / pricing that may relate to future returns or risk.  In scenarios where there is an expected dislocation in price, there may be more demand for calls vs puts or vice-versa.  Buying pressure on puts or calls will tend to impact the option price (and therefore implied vol), much as unbalanced buying / selling impacts price in the "spot" market. 

Towards this end I want to look at measures of the shape of the implied vol (IV) curve for one or more given constant maturities.  The most obvious place to start is by looking at volatility skew as measured by:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Skew](/assets/2017-09-21/skew1.png)

for a specific fixed constant maturity over time.  Given the need to look at a constant maturity, we will be walking the volatility surface from day-to-day, so must have both a reasonable estimate of the strike / moneyness cross-section of the surface as well as a reasonable interpolation on the time dimension.

As with building interest-rate curves, there are certain constraints with respect to arbitrage that should be observed, but the rest is more of an art than a science.  I am not going to be using the vol surface to price exotic options and am more interested in a smooth fit through (often) noisy data than in arbitragable irregularities.  After working with a number of different models, determined that the SABR model was a reasonable choice.   While the SABR model may not be as sophisticated as some of the later stochastic volatility models it is fairly easy to calibrate and reason with.   The SABR model allows for separate, correlated, evolution of both the forward price and volatility over time, expressed as:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![SABR Model p1](/assets/2017-09-21/SABR-eqn1.png)

where the stochastic processes are correlated in the following manner:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![SABR Model p1](/assets/2017-09-21/SABR-eqn2.png)

To calibrate a section of the surface, along the strike / moneyness axis, we then determine appropriate parameters of alpha, beta, nu, and rho (often with a fixed value of beta), that determine a least-squares fit between market IVs and modeled IVs.  An approximation for IV is given by:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![SABR Correlation](https://wikimedia.org/api/rest_v1/media/math/render/svg/b655790d56b2db650aaf6be8acc919dc0d12cecb)

One can then use multivariate gradient descent to determine a least-squares fit beteen observed market IV and SABR-implied IVs.  Using the Levenberg-Marquardt algoritm paired with an error function, and reasonable initial starting parameters, can regress towards an error minimum.

## Problems

Given only 3 degrees of freedom and a log polynomial structure in F, the SABR model cannot express the full variety of curvature seen in empirical smiles.   Below is a case in point, where in order to accommodate the steep smile, the curvature near the ATM causes the function to undershoot the market vols (observe the undershoot between 220 and 240 strikes).   The market vols should be flatter near the money and steeper further away.

![SABR unweighted](/assets/2017-09-21/SABR-1m-unweighted.png)

Given that most of the trading volume occurs on near-the-money strikes, thought could rectify this by using a weighted least squares solution, where the error function is weighted by the trading volume at a particular strike.  This improve the accuracy of the near-the-money strikes significantly, but the SABR function does not have enough degrees of freedom to overcome the steep curve required for the deep-in-the money strikes (below 200).

![SABR weighted](/assets/2017-09-21/SABR-1m-weighted.png)

Neither of these approaches is sufficient for my purposes.  The weighted SABR provides good resolution on 20 to 75 delta but with poor resolution on the wings.  Conversely, the unweighted SABR provides the best estimation of the wings, but with poor resolution on the near-the-money strikes.

## A Heuristic Approach
I took the approach of computing both of the weighted and unweighted SABR model and blending on transition points on the wings.  I use a linear blend between the weighted and unweighted over a 10 delta range between the near-the-money portion (weighted) and the deep in/out of the money portion (unweighted).  This works rather well, providing needed precision at the near-the-money strikes and preserving overall shape in the deep in/ou of the money wings:

![SABR weighted](/assets/2017-09-21/SABR-1m-blended.png)

## Final Thoughts
The solution is far from perfect.  One would hope to use a model that has just enough degrees of freedom to express the full repertoire of smiles.  I have not fuly investigated all of the stochastic volaility models, though expect some will do quite a bit better here, though at the cost of much greater complexity.   Given that my goal is to smooth over data anomalies (for example the 150 strike in the above example), the explanatory shortcoming is not pose a problem.

I will be evaluating 10 years of data across a wide range of underliers.  Will post more about what I find in coming weeks.




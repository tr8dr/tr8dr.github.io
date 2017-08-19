---
author: tr8dr
comments: true
date: 2014-12-26 15:37:17+00:00
layout: post
link: https://tr8dr.wordpress.com/2014/12/26/causality-in-observational-data/
slug: causality-in-observational-data
title: Causality in observational data
wordpress_id: 1009
categories:
- strategies
---

In the past had created [clusters](http://tr8dr.wordpress.com/2010/01/22/cointegration-clusters/) on assets to help identify relationships across assets.   The resulting graphs were useful in identifying assets for use in mean-reverting portfolios.   A difficulty in the approach was always around accurately measuring the strength of relationships between asset pairs.   I had looked at Granger-causality, which is fairly limited in that expects asset relationships to follow a VECM-like model, and eventually settled on weighting across a number of techniques as an approximate.

Determining causality for more general relationships requires a very different approach, where Y ← f(X) + ε.   i.e. f(x) may be an (unknown) non-linear function on X.   I came across an interesting paper: "Distinguishing cause from effect using observational data: methods and benchmarks" ([link](http://arxiv.org/pdf/1412.3773v1.pdf)) which builds on work around looking at the asymmetry of the "complexity" of p(Y|X) versus p(X|Y), where if  Y ← X (X causes Y), p(Y|X) will tend to have lower complexity than p(X|Y).

The paper provides results on two methods: Additive Noise Methods (ANM) and Information Geometric Causal Inference (IGCI), where ANM generally did better across a variety of scenarios than IGCI.

The algorithm for ANM in python-like pseudo-code (taken from the paper):

[code language="python"]
def causality(xy: DataFrame, complexity: FunctionType, complexity_threshold: float):
    n = nrows(xy)
    xy = normalize (xy)
    training = sample(xy, n/2)
    testing = sample(xy, n/2)

    ## regress Y <- X and X <- Y on training set
    thetaY = GaussianProcess.regress (training.y, training.x)
    thetaX = GaussianProcess.regress (training.x, training.y)

    ## predict Y' <- X' and X' <- Y' & compute residuals on testing set
    eY = testing.y - GaussianProcess.predict (testing.x, thetaY)
    eX = testing.x - GaussianProcess.predict (testing.y, thetaX)

    ## determine complexity of each proposition on variable and residuals
    Cxy = complexity (testing.x, eY)
    Cyx = complexity (testing.y, eX)
    
    ## determine whether Y <- X, X -> Y, or indeterminate
    if (Cyx - Cxy) > complexity_threshold:
        return X_causes_Y
    elif: (Cxy - Cyx) > complexity_threshold:
        return Y_causes_X
    else:
        return None

[/code]

The paper recommended using a scoring of the Hilbert-Schmidt Independence Criterion (HSIC) as the complexity test. I will have to code the above up and see how it does on a broad set of assets. The paper did test the CEP benchmark which includes some known financial relationships.


### Addendum


Am quite busy now with another investigation, but will revisit this with a proper implementation.

---
author: tr8dr
comments: true
date: 2020-07-27 17:00:00+00:00
layout: post
title: Why ML is Hard (part 2)
subtitle: Label Noise and what we can do about it
categories:
- machine learning
---
Following on from the prior post, want to discuss the repercussions of the low signal / noise ratio and how it effects:

- labeling / mis-labeling
- patterns unsupported by features
- buried signal

How does this manifest and what might we do to ameliorate the issues it poses.

## Introduction 
Financial timeseries appear to have a very low signal to noise ratio, where the variance (the power of the noise frequency) can 
be higher than the power of the overall signal.  There are many different ways to quantify the S/N ratio of a financial timeseries, but
will not pursue here.  That said, price uncertainty is a function of limited information.  If we knew or could deduce
the individual decision making and information state of each market actor, we would could have certainty about the next action 
of each participant.  This would still not be enough to predict future prices far forward, unless we could also model the future states of
exogenous information influencing market actors.

... work in progress ...


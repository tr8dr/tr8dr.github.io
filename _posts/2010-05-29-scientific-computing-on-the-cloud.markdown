---
author: tr8dr
comments: true
date: 2010-05-29 00:50:52+00:00
layout: post
link: https://tr8dr.wordpress.com/2010/05/28/scientific-computing-on-the-cloud/
slug: scientific-computing-on-the-cloud
title: Scientific Computing on the Cloud
wordpress_id: 655
categories:
- strategies
---

I've been keeping a close eye on the costs of the various clouds versus the costs of internal cpu farms.   [Amazon EC2](http://aws.amazon.com/elasticmapreduce/#pricing) pricing for **high CPU map-reduce** appears to be evenly priced with my costs to host internally.

I calculated this based on depreciating core i7 920s over a 3 year period and accounting for 0.14$ / khw @ 150 watts continuously.   I arrived at a lower cost than Amazon's, however when adjusting for cpu performance, !/$ ties out or is bettered by the Amazon proposition.

Amazon's rates for calculations using map-reduce are 1/5th the cost of a normal instance.    I'm estimating that the the high cpu 8 core is performing at SPECfprate2006 of approximately 150, twice the core i7 920.   The cost is $0.12 / hr versus a non map-reduce instance cost of 0.68 / hr.

This is great news for those doing transient large scale scientific computations (such as myself).   I now need to look to map my machine learning and strategy evaluation algorithms to map-reduce.

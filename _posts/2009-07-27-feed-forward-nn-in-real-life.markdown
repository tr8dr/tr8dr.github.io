---
author: tr8dr
comments: true
date: 2009-07-27 18:12:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/07/27/feed-forward-nn-in-real-life/
slug: feed-forward-nn-in-real-life
title: Feed Forward NN in "real life"
wordpress_id: 23
categories:
- biological systems
- genetic algorithms
- neural networks
- regression
---

Turns out that the Nematode Caenorhabditis Elegans has a nervous system that is similar to a feed forward network.  A feed forward network is one where neurons have no backward feedback from neurons "downsignal" (i.e. the neurons and synapses can be arranged as a directed acyclic graph).     This is very analogous to the feed forward network first envisaged for Artificial Neural Networks.  
  
The worm has exactly 302 neurons and ~5000 synapses, with little variation in connection between one worm and another.   This implies on average less than 20 synapse connections per neuron.   This is in contrast to the mammalian brain, where most neurons have a feedback loop back from other neurons downstream of the signal.  
  
I am very enthusiastic about this area of [research](http://www.setiai.com/archives/000050.html) as it progresses us step-by-step closer to realizing mapping an organism brain onto a machine substrate.   The nematode is quite tractable because of the fixed and very finite number of neurons.  
  
ANNs are no longer in vogue, but I use feed forward ANNs for some regression problems.   Of course my activation function is likely to be quite different from the biological equivalent.   ANNs are not a very active area of research given their limitations, but one does find them convenient for massive multivariate regression problems where one does not understand the dynamics.  
  
The regressions that I solve only have sparse {X,Y} pairs if at all and can only be evaluated as a utility function across the whole data set.   This precludes the various standard incremental "learning" approaches.   Instead I use a genetic algorithm to find the synapse matrix that maximizes the utility function.  
  
SVM is more likely to be used in this era than ANNs for regression.  Its drawback is that it requires one to do much trial and error to determine an appropriate basis function, transforming a nonlinear data set into a reasonably hyperlinear dataset in another space.

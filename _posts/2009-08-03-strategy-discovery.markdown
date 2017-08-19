---
author: tr8dr
comments: true
date: 2009-08-03 12:39:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/08/03/strategy-discovery/
slug: strategy-discovery
title: Strategy Discovery
wordpress_id: 28
categories:
- genetic algorithms
- genetic programming
- neural networks
- regression
- strategies
---

Today I want to discuss the process of building or discovering a strategy.  Generally medium to high-frequency models fall into one of the following catagories:  


  * set of rules / heuristics on top of statistical observations
  * analysis of price signal
  * evolving state-based model of prices
  * spread or portfolio based relationships
  * technical indicators
  * some combination of these within an bayesian or amplification framework

These models share a common problem in that they are just crude approximations.  They attempt to accurately determine behavior on a macro level.  


The market is the emergent behavior of the trades and order activity of all of its participants.  The perfect model is one that would have to be able to predict the behavior of each individual participant and be aware of all external stimuli affecting their behavior.   This is at worst unknowable and at best would require something akin to an omniscient AI.  


The best we can do is have a view or views around how to model market behavior.   We can chose one of three approaches towards modelling:

  * create models that rationalize some statistical or behavioral aspect of the market
  * create models using a evolved program or regression, without a preconceived rationalization  

  * create models that embody a combination of the above two approaches  


Preconceived models have the advantage of being explainable, whereas, generated models often are not. That said, it is intriguing to pursue evolution and/or program generation as a means of discovering strategies in an automated fashion.  


Rationale  
Manual model development and testing is very time consuming.  One will start with a conjecture or skeleton idea for a new strategy.   The parameter space or variants of the idea may be large.   Each has to be tested, optimized, retested.  
****

Many of my strategies start out as models that digest raw prices and produce some form of "hidden state". This hidden state is designed to tell us something useful with less noise than the original signal. This state may be multidimensional and may require further regression to map to buy and sell signals.

Obtaining optimal strategies point towards a multivariate numerical or codified regression approach.  The testing and discovery of parameters and model variations would best be automated.  


**Tools**  
There are a number of approaches used in optimization, regression, or discovery problems:

  * Regression  
ANN (Artificial Neural Nets), SVM (Scalable Vector Machine), RL (Reinforcement Learning)  

  * Optimization  
GA (Genetic Algorithms), Gradient Descent, Quadratic Optimization, etc  

  * Discovery  
GP (Genetic Programming), perhaps ANN as well

**Strategy Discovery**  
Thus far I have mostly used tools in the Regression and Optimization categories to calibrate models. Genetic Programming represents an interesting alternative, where we generate "programs" or strategies, testing for viability and performance.

The "program" represents a random combination from an algebra of possible operations that operates on a set of inputs to produce an output. In our case, our inputs will be the digested information that our models produce. The program will map this into something that can be used to generate buy/sell/out signals. 

Thousands of such programs are generated and evaluated against a fitness function. The fitest programs replicate, perform crossover, and mutate. This can be repeated for thousands of generations until programs with strong trading performance are determined.

An alternative and perhaps simpler approach is to use an ANN coupled with a GA. The GA generates weights / connections between neurons to produce a model between inputs and outputs.

**Questions Under Consideration  
**ANNs and GPs differ in a number of important ways. Need to think further on the following:

  * ANNs and GPs can represent an infinite number of functions  
ANNs accomplish this, though, at the cost of numerous neurons  

  * ANNs and GPs may have a very different search space in terms of volume  
We want to choose an approach that will converge more quickly (ie have a smaller search space)  

  * How should we constrain the algebra or permutations to affect convergence  
There are many "programs" which are equivalent, there may also be certain permutations we may not want to allow.  

  * What sort of inputs are useful and how do we detect those that are not  
Inputs that are not useful should ultimately have very little trace through the model. Will have to determine how to detect and prune these.

More thought needed ...

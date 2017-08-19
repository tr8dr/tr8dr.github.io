---
author: tr8dr
comments: true
date: 2009-12-15 19:21:49+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/12/15/advances-in-computing/
slug: advances-in-computing
title: Advances in Computing
wordpress_id: 378
categories:
- HPC
- machine-learning
---

Have been watching advances in computing power for some time, since university really.   I did research in parallel algorithms and architecture for my first position at the university and later applied practically on Wall Street.   In those days super-expensive machines like the Intel Hypercube, Paragon, and many other architectures were the backbone of the HPC community.

HPC (High Performance Computing) roughly breaks down into 4 categories:



	
  1. Big iron supercomputers (MIMD generally)

	
  2. Distributed computing (these days advertised as Cloud Computing)

	
  3. The emerging SIMD GPU based solutions

	
  4. Quantum Computing (not really here yet for the mainstream)


In the machine learning and optimisation world there are massive problems, some of which are not computable on von-neumann architectures, as their runtime would be astronomical.    An (absurd) example of such a problem would be to simulate a large number monkeys typing on typewriters, stopping when one produces the works of Shakespeare.   The number of monkeys required to produce such a work on average in astronomical.     This seems like an absurd problem, but is comparable to the GP / GA approach.

Then of course there are numerous problems with high dimensionality and/or with polynomial order complexity.

**Supercomputing on the Cheap
**The [FASTRA](http://fastra.ua.ac.be/en/index.html) team at the University of Antwerp has put together an inexpensive multi-teraflop machine with 7 gaming cards.  Check out their video.

Unfortunately the "easy" part of these sort of solutions is the hardware.  The problem is the (often) great expense to develop one's models in a SIMD framework, so can be applied to for the GPU architecture.    Although there is now standardization on the low-level C-variant used to program GPUs, there are significant differences between different models of GPUs, that even if you manage to write a correct SIMD program, may have to rearrange for a specific GPU implementation.   (I guess this is not all that different from my experiences with big-iron parallel architectures of the past).

One could have a team devoted to parallelization, tuning, and retuning / reworking for the new GPUs that are out periodically.   Very time consuming!

For my work, the problems that would map well are particle filters and monte-carlo based models, each of which have obvious fine-grained parallel operations.

**Quantum Computing
**The other notable announcement this week was Google's use of [quantum computing](http://googleresearch.blogspot.com/2009/12/machine-learning-with-quantum.html) to solve pattern recognition problems.   I have not done the leg-work to fully understand the algorithms in quantum computing, but broadly it seems to be a matter of framing one's problems statistically as path integration problems (i.e., expectations), where quantum computing allows the paths to be explored simultaneously.

**
**

---
author: tr8dr
comments: true
date: 2009-07-27 18:43:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/07/27/future-for-commercial-hpc/
slug: future-for-commercial-hpc
title: Future For Commercial HPC
wordpress_id: 24
categories:
- grid
- HPC
- performance
---

As I noted in an earlier post I have High Performance Computing requirements.   Basically if you can give me thousands of processors, I can use them.   The problem with HPC today is that it is one or more of the following (depending on where you are):  


  * academic and only open on a limited basis to researchers based on their proposals
  * internal
  * available but not cost effective (8 core @ $7000 / compute year at amazon)
This flies in the face of what we know:  


  * there are many thousands of under or un-utilized computers available
  * the true cost of computing power + ancillary costs (power, people) can be scaled to a much lower #
  * organizations should want to monetize this underutilized capacity  

  
Why do I care about this?   Well, I could use cheap computing power today, but also I used to be a parallel algorithm researcher back in the day, so have been waiting for this for a long time.  
  
The solution needs to be to allow compute resource providers a means to auction their unused resources for blocks of time, immediate or future.   HPC users that want to evaluate a massively parallel problem can collect a forward dated/timed group of nodes for execution, finding a group within their cost range or wait for lower cost nodes to become available.  
  
How would this be accomplished?  


  1. Exchanges are set up for geographical areas where providers can offer gflop-hr futures and consumers can buy computing futures or alternatively sell their unused futures.  

  2. Contract requires standardized power metrics (SPECfprate2006 for instance)
  3. Contract requires standardized non-CPU resource (min memory, disk)  

  4. Standard means of code and data delivery (binary form, encryption, etc)  

  5. Safe VM in which to run code  

  6. Checkpointing to allow for a computation to be moved (optional)  

  
Research into auction-based scheduling and resource allocation began in the early 90s, perhaps earlier.   The first paper I saw in this regard was in 1991.   There are now hundreds of papers on this and a few academic experiments.    There should be a big market for this amongst web hosting companies, etc.  
  
Amazon and Google, although likely to be very efficient with resource utilization,  are likely to have peak periods and slack periods like everyone else.   The strategy would be to price resources lower during slack periods to attract "greedy" computations looking for cheap power.  
  
I have specific ideas about how this would be implemented.  Contact me if you are interested.

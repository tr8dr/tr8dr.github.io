---
author: tr8dr
comments: true
date: 2009-07-27 12:44:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/07/27/high-performance-computing-on-the-cheap/
slug: high-performance-computing-on-the-cheap
title: High Performance Computing on the Cheap
wordpress_id: 22
categories:
- HPC
- performance
---

I have a couple of trading strategies in research that require extremely compute intensive calibrations that can run for many days or weeks on a multi-cpu box.     Fortunately the problem lends itself to massive parallelism.  
  
I am starting my own trading operation, so it is especially important to determine how to maximize my gflops / $.   Some preliminaries:  


  * my calibration is not amenable to SIMD (therefore GPUs are not going to help much)
  * I need to have a minimum of 8 GB memory available
  * my problem performance is best characterized by the SPECfprate benchmark
I started by investigating grid solutions.   Imagine if I could use a couple of thousand boxes on one of the grids for a few hours.   How much would that cost?  
  
Commercial Grids  
So I investigated Amazon EC2 and the Google app engine.   Of the two only Amazon looked to have higher performance servers available.    Going through the cost math for both Amazon and Google revealed that neither of these platforms is costed in a reasonable way for HPC.  
  
Amazon charges 0.80 cents per compute hour, $580 / month or $7000 / compute year on one of their "extra-large high cpu" boxes.   This configuration of box is a 2007 spec Opteron or Xeon.   This would imply a dual Xeon X5300 family 8 core with a SPECfprate of 66, at best.     $7000 per compute year is much too dear, certainly there are cheaper options.  
  
Hosting Services  
It turns out that there are some inexpensive hosting services that can provide SPECfprate ~70 machines for around $150 / month.   That works out to $1800 / year.   Not bad, but can we do better?  
  
Just How Expensive Is One of these "High Spec" boxes?  
The high-end MacPro  8 core X5570 based box is the least expensive high-end Xeon based server .  It does not, however, offer the most !/$ if your computation can be distributed.   The X5500 family performs at 140-180 SPECfprates, at a cost of > $2000 just for the 2 CPUs.  
  
There is a new kid on the block, the Core i7 family.   The Core i7 920, priced at $230 generates ~80 SPECfprates and can be overclocked to around 100.   A barebones compute box can be built for around $550.   I could build 2 of these and surpass the performance of a dual cpu X5500 system, saving $2000 (given that the least expensive such X5500 system is ~$3000).  
  
Cost Comparison Summary  
Here is a comparison of cost / 100 SPECfprate compute year, for the various alternatives.   We will assume 150 watt power comsumption per cpu at 0.10 / Kwh, in addition to system costs.  
  


  1. Amazon EC2  
$10,600 / year.   100/66 perf x 0.80 / hr x 365 x 24  
  

  2. Hosting Service  
$2,700 / year.  100/70 perf x $150 x 12  
  

  3. MacPro 2009 8 core dual X5570  
$1070 / year.  100 / 180 perf x $3299 / 2 + $160 power  
  

  4. Core i7 920 Custom Build  
$430 / year.  100 / 80 perf x $550 / 2 + $88 power  
  

  5. Core i7 920 Custom Build Overclocked  
$385 / year.   100 / 100 perf x $550 / 2 + $100 power  

  
The Core i7 920 build is the clear winner.   One can build 5-6 of these for the cost of every X5570 based system.  Will build a cluster of these.  
  


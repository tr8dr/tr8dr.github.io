---
author: tr8dr
comments: true
date: 2007-10-29 23:54:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2007/10/29/path-dependent-problem/
slug: path-dependent-problem
title: Path Dependent Problem
wordpress_id: 9
categories:
- probability
---

Saw a fun problem posed on one of the [Joel On Software](http://joelonsoftware.com/) forums:  


<blockquote>  
On a empty chessboard, a knight starts from a point (say location x,y) and it starts moving randomly, but once it moves out of board, it can't come back inside. So what is the total probability that it stays within the board after N steps.  
</blockquote>

  
This problem is similar to a digital knockout barrier option, but where the path is the random movement of a chess piece across the board rather than the movement of a security price over time.  The solution I wrote up was as follows:  
  


<blockquote>  
This can be computed as the an expectation function across all possible paths (including paths outside of the 8x8 grid).  Think of a 8-way tree where the root represents the starting position and each node has 8 children representing each of the 8 possible moves from the current position.  
  
The joint probability of:  
  
- making the move (1/8)  
- within grid (0 or 1)  
  
should be the value of any given node (basically 1/8 or 0).  For any given path of length N, the joint probability of having taken the path and being fully within the grid is either (1/8)^N or 0.  The expectation function simply sums across all possible N length paths.  
  
The expectation function and paths can be expressed as a recursive sum (assuming i,j in chessboard for [1,8]):  
  
E(i,j,N) =  

> 
>   * 1/8[E(i-2,j-1,N-1) + E(i-2,j+1,N-1) + E(i+2,j-1,N-1) + ...]  if  1 <= i,j <= 8
>   * 1  if N <= 0
>   * 0  otherwise
>   
  
</blockquote>

  
I would be curious to see other approaches for this problem.  The recurrence relation with conditional function is interesting because can introduce a wide variety of conditions and payoffs.  Would be easy to change the "barriers" and/or skews in the probability of moving in one direction versus another.

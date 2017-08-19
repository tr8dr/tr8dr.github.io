---
author: tr8dr
comments: true
date: 2010-09-12 14:19:57+00:00
layout: post
link: https://tr8dr.wordpress.com/2010/09/12/graph-visualization/
slug: graph-visualization
title: Graph Visualization
wordpress_id: 762
categories:
- strategies
---

I am looking for a good application or library for graph (as in network) visualization.  I need to be able to visualize very large graphs so the static graph rendering approach does not work.   Want something dynamic where can focus on local context while navigating the graph.  Something a bit more sophisticated than [this](http://www.neuroproductions.be/twitter_friends_network_browser/) for instance.    I'd like to have a notion of weight and direction on the graph.   Ideally should be able to take graphml as input.

Ran across some visualization libraries:



	
  1. [Protoviz](http://vis.stanford.edu/protovis/ex/) (beautiful rendering and graph ideas)
It looks like a bit of work, but with some code could probably use this library as a basis for interactive exploration.   Perhaps too much work for my busy schedule.  There are a number of examples that give me ideas for other visualizations.

	
  2. [JUNG](http://jung.sourceforge.net/) (I already use this for graph analysis, but not for visualization)
Visualization is not terribly pretty but functional.   Is not dynamically oriented.

	
  3. [ThinkMap](http://www.thinkmap.com/)
These guys were one of the first I saw on the web (maybe 10 years ago) with a dynamic graphing technology.  It was flash based at the time.  Now it looks like they are using Java.   Strangely, their java UI is much inferior to their prior flash API.

	
  4. [Flare](http://flare.prefuse.org/) (Flex / Flash based API)
The graph layouts appear to be dynamic, but require quite a bit of code.  The code is actionscript based.   Hmm, don't want to go there.


I'm sure there are others.   Just some random ones ran across ...

**Addendum
Shane Conway pointed me to his [rwebvis](http://code.google.com/p/rwebvis/) package for R that provides a R interface to rendering with Protoviz (amongst other web based renderers).   Looks like it is just what I need (thanks)!   Protoviz requires some programming, however the level of abstraction seems about right for something this flexible. **

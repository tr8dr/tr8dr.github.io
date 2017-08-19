---
author: tr8dr
comments: true
date: 2010-10-03 16:35:42+00:00
layout: post
link: https://tr8dr.wordpress.com/2010/10/03/languages-again/
slug: languages-again
title: Languages Again
wordpress_id: 801
categories:
- strategies
---

I do a lot of my production/  performance-oriented stuff in Java (for better or worse) because it is so portable and is very close to the performance of C++ if you know what you are doing.   I would much prefer to be using F# / C#, as they are far superior as languages in my view (though C# owes its heritage to Java more than any other language).

The problem is that I need to deploy on Linux (and for that matter, my dev platform of choice is osx).  Here are some performance #s for 500 million iterations of the nbody problem on a core i7 920 linux box:

[![](http://tr8dr.files.wordpress.com/2010/10/screen-shot-2010-10-03-at-12-20-32-pm.png)](http://tr8dr.files.wordpress.com/2010/10/screen-shot-2010-10-03-at-12-20-32-pm.png)

The nbody problem uses sqrt when calculating the euclidean distance between bodies.  Began to wonder whether much of the difference is in the implementation of the library math functions.   So I did an implementation that uses a (slow) numerical algorithm to calculate the sqrt, common across the languages.   In that way we have an apples-to-apples comparison.   The C++ vs Java numbers are now:

[![](http://tr8dr.files.wordpress.com/2010/10/screen-shot-2010-10-03-at-12-20-48-pm.png)](http://tr8dr.files.wordpress.com/2010/10/screen-shot-2010-10-03-at-12-20-48-pm.png)

The difference is now 1.9%.   In fact the additional 10 seconds difference may be Java startup + JIT compilation.   This resonates with other comparisons I have done in the past.

I have code in Java that outperforms equivalent C++ code.  For instance, the JIT compilation is clever enough to avoid virtual function invocation when it can be avoided / determined at runtime.   The same cannot always be done as effectively with static compilation.    (The cost of virtual function calls is 3x more expensive than non-virtual on the intel architecture).

The JVM is just the best VM around (even faster than the MS .NET vm).   I would love to see the JIT compilation technology in the JVM applied to a cross-platform .NET VM, such as Mono.   Alternatively, if the JVM was expanded to include value types, a properly implemented generic type system, and some functional features, could imagine targeting IL for the JVM.

My ideal would be to be able to cross-compile C# and F# to JVM bytecode (much in the way one can map java bytecode to the CLR with ikvm).   The .NET libraries would be a much smaller problem given that the Mono implementation already exists.    The mono guys have their hands full, and seem to have focused more on breadth rather than ultra-high performance.

I suppose the .NET and Java communities are mostly distinct, so probably few people interested in seeing F# targetted to the JVM.   Can only hope that either the mono VM continues to improve performance and GC or a new subsuming VM fills the gap where the JVM once ruled.

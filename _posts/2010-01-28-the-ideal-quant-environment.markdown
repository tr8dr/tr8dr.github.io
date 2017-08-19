---
author: tr8dr
comments: true
date: 2010-01-28 02:36:42+00:00
layout: post
link: https://tr8dr.wordpress.com/2010/01/27/the-ideal-quant-environment/
slug: the-ideal-quant-environment
title: The ideal Quant Environment
wordpress_id: 578
categories:
- strategies
---

R is a wonderful tool in that one can prototype and test ideas very quickly.   If your not using R and doing most of your work in C++ or other low-level language, your missing a lot.   The speed of development between a C++ / Java / C# versus R is 10-50x.

R definitely has its warts as a language and environment.  I not a huge fan of its matrices, index operators, or lazy expression parsing.   More crippling is the fact that R is slow and has memory issues for large data sets.    I would estimate that R is 100x slower than java or C++, depending on what you are doing.

My current environment is a combination of R and a number of lower level languages.   For much of my post-exploration work I feel compelled to write in a lower level language due to the performance issues with R.   My preference is to be using a functional language, as they are generally very concise and elegant.

**Ideal Environment**
Here is what an ideal environment for me would be:



	
  1. breadth and depth of R

	
  2. clean functional language design

	
  3. concise operations (as close to the math as possible)

	
  4. excellent rendering facilities

	
  5. real-time performance

	
  6. ability to work with large data-sets (memory efficient)

	
  7. concurrency (I do a lot of parallel evaluation)


**Candidates**
Here are candidate environments that I've used or explored:



	
  1. python
cleaner, generally faster than R, very little in the way of statistics and poor integration with R.   No real concurrency as interpreter locked.

	
  2. Ocaml
Beautifully concise language, INRIA implementation does not support concurrency

	
  3. F# variant of Ocaml
Solves Ocaml's problems but bound to MS platform

	
  4. Scala
Excellent performance, a bit bleeding-edge, much more complex than Ocaml, but on the JVM.


**The Special Blend**
It is impractical to consider reimplementing even a subset of R into another language environment.   A hybrid approach makes sense.   With python you have **Rpy** and with Java  **JRI**.   Neither of these have first class interaction with the language though.

I would like to have the power of a production functional language, but with the same development ergonomics and interaction that R has.    It occurred to me that I could do the following:



	
  1. dump function templates of all R functions in one's environment into a Scala module

	
  2. create implicit conversions from R fundamental types into Scala objects (matrices, vectors, data frames, etc)

	
  3. create specialized mappings between my Scala-side timeseries and R zoo / ts objects

	
  4. create some wrappings for unusual usage patterns such as the ggplot operators


Scala has an interactive mode where functions, classes, etc. can be created on the fly.   Because we've dumped proxy function for each R function, we also have near first class access to R functions.   There would be some differences in that would not be able to replicate the lazy evaluation aspect of some of the R functions.   Functions that use expression() would have to be wrapped specially.

Because scala allows a lot of syntactic magic, the environment would look very much like R and have complete access to R, but with the huge upside of the more powerful Scala.    One can write code that is "production ready" from the get-go and/or do very compute intensive operations otherwise prohibitive in R.

Now I need to find the time to put this together ...

**Addendum**
I have not decided on which language / environment to base this on.   Scala's main sell is that it is on the JVM.   The other functional language contender on the JVM with above-scripting-level performance is Clojure.

Clojure is basically a dialect of Lisp and there is already a project  called [Incanter](http://github.com/liebke/incanter#readme) that provides a statistical environment within Clojure.    It looks interesting, if early.   Clojure does not yet have a performance profile that is close enough to the metal.   I would expect to see improvements over time though, but due to Lisp's lack of static typing or type inference, I am doubtful that will see Clojure at the level of statically typed languages.

Since writing this post and having some conversations, I've begun to think that F# may be the best choice.   My language preference has been to use Ocaml, but the INRIA Ocaml implementation is handicapped.   F# is closely related to Ocaml and therefore may be a fit.

F# on the MS .NET platform has been shown to be as performant as C#.    From benchmarking C# a couple years ago, was clear that the CLR is pretty close to the JVM in terms of performance.   Given my cross platform constraints, the question has been how viable is F# with Mono from a performance point of view?

It seems like the Mono performance  is being addressed.    The mono LLVM experiment improved mono benchmarks significantly.   I have not been able to test Mono with this extension.   Will have to experiment.

To wed this to R would require writing the equivalent of JRI for .NET / R.

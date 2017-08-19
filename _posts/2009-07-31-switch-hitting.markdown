---
author: tr8dr
comments: true
date: 2009-07-31 11:24:00+00:00
layout: post
link: https://tr8dr.wordpress.com/2009/07/31/switch-hitting/
slug: switch-hitting
title: Switch Hitting
wordpress_id: 26
categories:
- programming
---

I've used many languages across the years, both functional and imperative. Since I live in the real-world (ie non-academic), I've generally had to use languages with a large user base, strong momentum, etc. In general my must-have criteria has been:  
  


  * performance
  * access to wide range of libraries, both infrastructural and numerical
  * scalable to large, complex, applications
  * runs on one of the major VMs (JVM or CLR)

I would have liked to have had the following as well:

  * elegance
  * functional programming constructs
**Changing Climate**  
Things have changed over the last few years though. We now have strong functional languages with high performance, access to diverse libraries, and a wider user base.   There is now even talk in the wider (non-functional programming) community about functional languages.  
  
On the imperative side have used (by frequency Java, C#, Python, C/C++, Fortran, etc). I have probably invested the most in Java at this point, as have found the complexity of C++ over the years distasteful and time consuming.  
  
I write parallel-targetted numerical models and write trading strategies around them. The problem is that none of the above imperative languages map well to the way I think about problems and require a lot of unnecessary scaffolding.  
  
**Functional Languages on the VM**  
There have been some stealth functional language projects (ie not all that well known outside of the functional community), such as [Clojure ](http://clojure.org/)for the JVM. Clojure, however, does not have the performance level I am looking for and is not much elevated above lisp. I love scheme / Lisp, but feel that it doesn't scales well for large projects.  
  
Relatively new on the scene is [Scala](http://www.scala-lang.org/). Scala aims to be a statically typed language with OO and functional features, bridging between the two paradigms. It has many nice features, but is more verbose then other functional languages. It is certainly much more concise than Java, which is a big plus, but does have some bizarre syntax and inconsistencies I'm not thrilled with. There are also some performance issues related to for-comprehensions that mean I cannot use it right now. Nevertheless, for the JVM, I think it is the only language I could consider for functional programming.  
  
Enter [F#](http://research.microsoft.com/en-us/um/cambridge/projects/fsharp/). Ok, Microsoft scares me. That said, as with C# and the CLR, they have been leading the pack in innovation. They picked up on the Java mantle, fixed its flaws, and have since evolved into Java into something much better: C#.  
  
F# is a new language (well a few years old) for the .NET CLR based on OCAML, not too distant from Haskell or ML. The language is concise, integrates well with the CLR and libraries, and performs very well.  
  
Frankly, I think F# is the superior to any of the other languages available on the JVM or CLR. My concern now is: is it worthwhile switching from the JVM to the CLR. I have a large set of libraries written in Java. I am also concerned about the degree of portability and that the Web 2.0 APIs (which Google is largely defining) tend to be Java / JVM based.  
  
**Practical Details**  
Am I going to have to live in a hybrid world where I write, say, some GWT apps in java and do my core work in the .NET CLR? Is there any hope of anything like F# on the horizon for the JVM?   Should I abandon eclipse and my other tools and look at Mono tools or worse be required to use Visual Studio?  
  
Give me a F# equivalent on the JVM.  Please!

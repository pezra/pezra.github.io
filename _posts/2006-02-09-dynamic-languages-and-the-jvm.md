---
id: 204
title: Dynamic Languages and the JVM
date: 2006-02-09T06:32:30-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=204
permalink: /blog/2006/02/dynamic-languages-and-the-jvm/
categories:
  - Software Development
tags:
  - Software
---
[Stephen O&#8217;Grady](http://www.redmonk.com/sogrady/archives/001251.html) and [Tim Bray](http://www.tbray.org/ongoing/When/200x/2006/02/02/LAMP-Java-Sun) have been having an interesting [conversation](http://www.redmonk.com/sogrady/archives/001261.html)  
about how the Java, and the JVM, relates to the LAMP stack, and dynamic  
languages in general. Dynamic languages are a subject close to my heart  
so I want to toss in my two cents.

On the ubiquity issue I am with Mr. O&#8217;Grady here, except, perhaps,  
that I will go a little further. I think that the JVM is in a lot fewer  
places than most Java advocates do. It is true that you can get JVM for  
just about any platform, but it is also true that you can get a Ruby or  
Python interpreter for just about any platform. Ubiquity is a function  
of how many people use a technology not the other way around and PHP,  
Perl and Python are just as ubiquitous as Java, if not more so.<footnote>I _really_ wish I could say this about Ruby, but I just can&#8217;t. Yet.</footnote>

I have only used Jython a little bit but I recognize the [frustration Mr. Sequira describes](http://www.jsequeira.com/blog/2006/02/07.html).  
I found it quite disconcerting to be working in an environment that  
was, at the same time, both, and neither, Java and Python. Particularly  
for the conceptual types that both languages support. For example, if  
you call a method that returns a string having to figure out whether it  
is a Python strong for Java string is just annoying.

The threading issue is the one that interests me the most. I think  
that Mr. Bray is right that hardware threading is quickly becoming an  
important issue. In the near future most machines will support  
significant levels of true, hardware level, concurrency. But I think  
the shared memory native threading model that Java has is _completely untenable_.

Even vm (green) threads, which are well understood and have nice  
uniform semantics are difficult to use correctly. Once you throw in the  
vagaries of native threads you have morass of complexity that is  
practically unbearable. Worst of all, the usefulness of most of the  
techniques we have to help manage software quality, like automated unit  
tests and continuous integration, are inversely proportional to the  
number of threads in the application.

The most important thing to keep in mind here is that, in the long  
run, it is always better (read: cheaper) to use extra computing  
resources to make problems more tractable for the humans in the system.  
For example, we have garbage collectors because it is cheaper to buy a  
slightly more powerful computer than to have you developer waste time  
managing their own memory. I am not sure what the best solution to  
highly concurrent hardware<footnote>The Cincom Smalltalk guys [think that green threads and multiple processes are the best solution](http://www.cincomsmalltalk.com/blog/blogView?showComments=true&entry=3303013147) and they might be correct. I have my doubts but I do think it works better than Java&#8217;s native threads approach.</footnote> is but I am quite positive it is not Java style threading. My gut tells  
me that none of the approaches I know for apparent concurrency are  
going to work well for a highly concurrent application on highly  
concurrent hardware. If that is the case we will see something new and  
different<footnote>I am guessing new and different will not from from Java-land because it  
Sun is looking very risk averse with regards to the Java spec right  
now. For example, they appear to have gutted Java generics with  
erasure, just to maintain backwards compatibility</footnote> as soon as the cool hardware gets into circulation.

It does feel broken that there are so many bits of code written in  
different languages that do the same thing. However, the JVM is not the  
One True Platform on which to solve that problem, if, indeed, it even  
is a real problem. The JVM appears, from a my layman&#8217;s point of view,  
not to be well suited to dynamic languages. Even if it were technically  
well suited the culture around Java and the JVM is far too static to  
support the experimentation need to find new and better ways to deal  
with the complexities of highly concurrent environments. I think, for  
the most part, dynamic languages need to stay off the JVM.
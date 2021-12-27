---
id: 81
title: The Perils of Duck Typing?
date: 2005-04-15T12:00:59-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/wordpress/?p=81
permalink: /blog/2005/04/perils-of-duck-typing/
categories:
  - Software Development
tags:
  - Software
---
In [The Perils of Duck Typing](http://beust.com/weblog/archives/000269.html) Cedric writes about some fears he has related to duck typing.

He says 

> Duck Typing is a big time saver when you write code, but is it worth it? Don&#8217;t you pay this ease of development much later in the development cycle? Isn&#8217;t there a risk that you might be shipping code that is broken?
> 
> The answer is obviously yes.
> 
> The proponents of Duck Typing are usually quick to point out that it should never happen if you write your tests correctly. This is a fair point, but we all know how hard it is to guarantee that your tests cover 100% of the functional aspects of your application.

There is certainly a risk that you will ship broken code. In fact, you will almost certainly ship broken code. But you will ship broken code regardless of typing model you use. Static typing is no solution to the problems of defects. But the fact that you saved a lot of time by using duck typing in development means that a) you can send a little more time on testing and there by reduce number of defects you ship, b) get to market earlier or c) both. The fact of the matter is that type related error do not happen often enough in practice to make them worth worrying about (when is the last time you got a ClassCastException while working Java collections).

Cedric goes on to describe the use of interfaces as documentation (using interfaces to document what methods must exist for a piece of code to work) while implying that duck typing prevents this. Interfaces as documentation is a nice use of interfaces but duck typing does not preclude this. Smalltalk has [SmallInterfaces](http://wiki.cs.uiuc.edu/VisualWorks/What+is+the+difference+between+Java&apos;s+interfaces+and+Smalltalk+interfaces%3F). In Ruby, MixIns are commonly use to define the set of methods that are required. But both of the environments are duck typed. In Ruby, for example, if I create a MixIn to define an interface, you can &#8220;implement&#8221; my interface merely by implementing the appropriate methods, regardless of whether you include my MixIn or not. Interfaces as documentation should be treated just like all other documentation &#8212; when it is helpful use it, when it is not ignore it.

To be fair, Cedric likes Ruby because you can used MixIns to define interfaces, but I think his has conflated two completely separate issues. Duck typing does not preclude well documented interfaces. You can poorly design and document an interface a statically typed language just as easily as you can in a duck typed one. You should take care to reasonably document the interfaces you use, regardless of the type system.
---
id: 101
title: Coding Is Not Construction
date: 2005-04-25T12:57:39-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2005/04/design-vs-construction/
permalink: /blog/2005/04/coding-is-not-construction/
categories:
  - Software Development
tags:
  - Design
  - LOP
  - Software
---
The other day I was talking to a colleague and I compared software development with building a building. I have heard this analogy often and there are a lot of similarities. (For example, most buildings and software systems are, at least partly, custom.) I think there is much to be learned from this analogy when correctly applied, however it is more often than not mis-applied and when it is it leads to all sorts of false conclusions.

The basics of this analogy are that building construction and software development have the following phases:

<di></p> 

Inception
:   Someone has an idea about what to build.
</di>

<di>

Design
:   An architect/engineer designs the building or software by drawing a set of picture and writing some text about the thing to be built.
</di>

<di>

Construction
:   A bunch of labors use the documents produced in design step to produce the final product.
</di>

<di>

Profit!
:   Sell the building or software.
</di> </dl> 

People often equate the construction to coding when applying this process to software development. The [RUP](http://en.wikipedia.org/wiki/Rational_Unified_Process) process, for example, uses these phases. But coding is design, not construction. The construction phase of building a building is more equivalent to compiling in software development. This is a bit easier to see if you look at what the output of a project is. In a building project the output is the building. In a software development project that output is it is the executable, and it&#8217;s supporting data, **not** the code. In the software industry we have already completely automated the construction phase. I think that we already know, sub-consciously at least, that coding is not construction because we call systems like make, ant, etc. &#8220;build&#8221; tools, implying that they construct the final product.

When you write code you are not producing the final product, you are producing a set of instruction to the construction team &#8212; the compiler and build tool &#8212; in much the same way an architect of a build produces instructions in the form of a set of blue prints. This distinction may not seem particular important at first, but the, incorrect, equation of coding to construction leads to some bad conclusions. Some example are component-based software engineering and certain types of out-sourcing.

What we call designs in software development are more like the artist&#8217;s view of a building than what architects produce as input to the building construction phase. While these nice pictures are useful, I think we have done our selves a disservice by calling them designs. Calling them designs implies that all the necessary information for construction is present and it never is. Design choices keep getting made until the day you freeze the code.

If we want to improve software development what we need are better construction teams, not off-the-shelf walls or structural designs for each floor done in different lower-cost-countries. Architects do not have to design a house down to the detail software developers write code because physical construction teams can fill in a lot of detail by themselves. I think this is why DSLs are often a big win. The compiler (or interpreter) for the DSL can fill in a lot of detail based on the it&#8217;s understanding of the domain.

<p class='update'>
  Fixed a couple of spelling errors.
</p>
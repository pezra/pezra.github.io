---
id: 130
title: The Null Object Pattern
date: 2005-06-27T14:39:17-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2005/06/null-object-pattern/
permalink: /blog/2005/06/the-null-object-pattern/
categories:
  - Software Development
tags:
  - Software
---
Noticed this [interesting article](http://www.smalltalkchronicles.net/edition2-1/null_object_pattern.htm) about the Null Object Pattern. Basically, it is the idea that the null/nil object could accept any message and return itself, rather than the more canonical approach of throwing an exception. This seems to lead to simpler, more elegant, code due to the lack of exception handling and null/nil checks. The trade off seems to be, you get generally simpler, more obvious, code at the expense of the occasional hard to track down bug.

I am not sure what I think about this but it does feel a bit like the debate around static vs. dynamic languages and in that debate I am firmly on the dynamic side of the fence because the subtle errors just do not seem to occur very often. Perhaps the subtle errors around null/nil do not occur often enough to warrant the checks we currently have.

<p class='via'>
  [via: <a href='http://www.rcrchive.net/rcr/show/303'>RCR 303</a>]
</p>
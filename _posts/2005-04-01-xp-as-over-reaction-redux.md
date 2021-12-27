---
id: 90
title: XP as Over-Reaction (Redux)
date: 2005-04-01T12:00:59-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/wordpress/?p=90
permalink: /blog/2005/04/xp-as-over-reaction-redux/
categories:
  - Software Development
tags:
  - Methodology
  - Software
---
In an [earlier entry](http://pezra.barelyenough.org/blog/2005/03/stop-over-reacting.html) I said, &#8220;XP is an over-reaction to water fall development methodologies.&#8221; I was wrong. I still think XP is an over reaction but I think it is reacting to heavy-weight methodologies rather than water fall methodologies.

The primary difference between XP and other methodologies is that it is that XP urges you to not do a lot of things you have been told to do in the past, because &#8220;you are not going to need it&#8221;, such as design documents and anticipated features. This mentality definately has some benefit &#151; it is very easy to over-engineer software &#151; but I think that most implementations of XP take it too far. It is easy to just decide not to do anything you do not need at this very moment. I think this sets you up for trouble in the future.

For example, I have been told that it is uncommon for Java code implemented using XP to have javadoc comments. This follows from the basic principles of XP, if you apply them with little thought. You do not need the comments when you are writing the method &#151; you just wrote the test and you know what the method is suppose to do &#151; so writing a comment is a waste of time. However I think that method and class comments are invaluable, they allow for much easier maintenance and refactoring in the future.

I suspect that most of my issues with XP come from poor implementations of the process. On the other hand, it almost does not matter. XP is intended to produce better software more reliably and if it is difficult to use correctly then it is unlikely to achieve that goal.
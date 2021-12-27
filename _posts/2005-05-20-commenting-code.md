---
id: 114
title: Commenting Code
date: 2005-05-20T17:29:23-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2005/05/commenting-code/
permalink: /blog/2005/05/commenting-code/
categories:
  - Software Development
tags:
  - Software
---
Mike Clark wrote and [article](http://www.stickyminds.com/s.asp?F=S9041_ART_3) on commenting code and Cedric adds a few things in [The Art of Commenting](http://beust.com/weblog/archives/000286.html), both are worth reading. I have a couple of other techniques which I find useful.

Make comments complete sentences. It takes a little longer but it helps make the context you have in your head, that the next guy will not, more explicit.

Put comments after the code to which they relate. Keeping it DRY is hard because before you write the code you are thinking about how you will achieve the result you want. _How_ you achieve something it not, generally, useful in a comment. If I want to see _how_ something is done I will read the code. I want comments about _what_ the code achieves, and _why_ achieving that is important. It is easier to comment about what has been achieved, and why, by writing the comments after the code because how you achieved it is already written. In my experience, this practice also leads to comments reading a bit like assertions, which I quite like because it make bugs stand out a bit more.
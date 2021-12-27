---
id: 241
title: A Better Ruby Profiler
date: 2006-06-22T09:46:00-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=241
permalink: /blog/2006/06/a-better-ruby-profiler/
categories:
  - Software Development
---
[Mr Savage and Shugo Maeda have released ruby-prof 0.4.0](http://cfis.savagexi.com/articles/2006/06/21/ruby-prof-0-4-0-with-call-graphs). I have to admit that I have not had much need for a profiler in Ruby (For Rails or otherwise) so far but it is nice to know that there is a decent profiler available when, and if, I need one.

The addition of a call graph report is absolutely vital and having them in a cross referenced HTML report is pretty sweet. I have used call graph profiling reports in the past but it was always with plain text reports. Following a call path in a plain text report can be little tedious at times. I expect that being able to follow links to particular call would make things quite a bit easier.
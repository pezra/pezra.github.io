---
id: 313
title: Concurrency and System Architecture
date: 2008-01-30T14:27:01-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2008/01/concurrency-and-system-architectures/
permalink: /blog/2008/01/concurrency-and-system-architectures/
categories:
  - Software Development
tags:
  - concurrency
---
[Mr Dekorte take on concurrency in shared memory systems](http://www.dekorte.com/blog/blog.cgi?do=item&id=3188)

> If you&#8217;re looking for languages or concurrency tools that will scale to the high core count desktop machines of the near future, I wouldn&#8217;t put stock in [MISD](http://en.wikipedia.org/wiki/MISD) oriented solutions such as transactional memory or elaborate functional programming compiler techniques. Shared memory systems simply won&#8217;t survive the exponential rise in core counts.

He is right, what we have now is not going to scale in the long run. I am not sure we will see much change on the ground any time soon, though. People, and industries, have an strong inclination to hang onto the status quo, even when there are better alternatives available. On the other hand, I would not be surprised if the future is largely populated by virtual shared memory systems running on top of physical [MIMD](http://en.wikipedia.org/wiki/MIMD) machines.
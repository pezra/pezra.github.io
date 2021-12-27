---
id: 498
title: resque-fairly
date: 2010-08-24T13:58:50-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=498
permalink: /blog/2010/08/resque-fairly-announce/
aktt_notify_twitter:
  - 'no'
aktt_tweeted:
  - "1"
categories:
  - Software Development
tags:
  - projects
  - resque
  - Ruby
---
I have been using [Resque](http://github.com/defunkt/resque) quite a bit recently. It is a really nice asynchronous job system based on [Redis](http://code.google.com/p/redis/).

Resque checks the queues for jobs to process in a fixed order. (In alphabetic order, to be precise.) This turns out to be a problem is you want predictable handling time for jobs. For example, consider a system which has queues `aaa` and `zzz`. If you add 100 jobs to `aaa` and 1 job to `zzz`, the job on `zzz` will wait a long time before being processed.

This problem is easily solved by just checking the queues in random order. Over time, any particular queue will be checked early so a few deep queues will not starve the other queues in the system.

[resque-fairly](http://github.com/pezra/resque-fairly) is a Resque [plugin](http://wiki.github.com/defunkt/resque/plugins) which provides that behavior. Just install the gem, add `require &#39;resque-fairly&#39;` and Resque will handle queues with approximate fairness.
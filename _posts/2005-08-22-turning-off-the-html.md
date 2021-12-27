---
id: 153
title: Turning Off the HTML?
date: 2005-08-22T13:00:07-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=153
permalink: /blog/2005/08/turning-off-the-html/
categories:
  - Software Development
tags:
  - Software
  - Web Applications
---
Mr. de hÃ“ra has an interesting [post about solving the one-click subscription issue](http://dehora.net/journal/2005/08/should_we_solve_oneclick_subscription_by_turning_the_html_off.html). I think he is wrong to write off the browser. It seems to me that feeds are best suited to providing sets of data which change regularly or are useful more than once and require little user interaction to construct. That tends to exclude a significant number of the things I do on the internet. For example, driving directions rarely change and are rarely useful more than once and getting them often requires some user interaction (e.g. clarification). Sure, you could view them in an aggregator but I don&#8217;t see any benefit.

On a side note, I currently use an online aggregator because I find the ability to access my subscriptions from any computer invaluable. I can imagine a way that different aggregators could be made to work off of an online subscription list so that I could use any aggregator on any machine, but I do not see that happening anytime soon. Until then I will be sticking with my browser based aggregator.

One other little point I wanted to bring out. Mr. de hÃ“ra says, &#8220;permalinking to a html file is starting to look more and more like a bug&#8221;, and he is right, it does look like a bug. Permalinking should be done at the resource level. The HTML and XML feed formats are just different representations of the same resource and therefore they should have the same URI. Blogging software could easily serve up different formats based on the accept header. &#8220;Auto-discovery&#8221; is a bug, not a feature.
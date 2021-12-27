---
id: 145
title: REST vs SOAP
date: 2005-08-05T16:15:13-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2005/08/rest-vs-soap/
permalink: /blog/2005/08/rest-vs-soap/
categories:
  - Software Development
tags:
  - REST
  - Software
  - Web Applications
  - 'ws-*'
---
Johannes Ernst posted an [interesting comparison of REST vs SOAP](http://netmesh.info/jernst/2005/08/03#soap-rest-noun-verbs) (<cite class='via'>via: <a href='http://www.furl.net/members/jamesgo#2005-08-04'>James Governor</a></cite>). He makes an excellent point that SOAP URIs point at the type, rather than an instance, of a service. I do not think that the fact that there will be more REST URIs means that this space is more innovative. I am not sure that the number of URIs is, in any way, correlated with innovation. Neither model seems to inherently limit innovation based on URIs, creating a new URI is free and there is an infinite supply (I suppose those two are related) regardless of how you are going to use it.

I think that there will be more innovation in REST architectures because they are more easily understood. REST architectures are easier to understand because they are goal oriented. To use Johannes&#8217; example, the accountant using the system does not want to &#8220;calculate this week&#8217;s payroll taxes&#8221;. She want to know how much this week&#8217;s payroll taxes are. If finding that out involves the system calculating it that is fine but the calculation is an _implementation detail_. The &#8220;this weeks payroll taxes&#8221; URI is cleaner because it describes the goal rather than the way we achieve that goal this week.

This seems reasonably obvious at the macro level, no thinks that humans should be making SOAP call from their browsers. I think that it makes just as much sense at lower levels. Interfaces, boundaries and encapsulation are for human consumption (the computer would be just as happy without those things) so it makes sense to design those interfaces in a way that is easy for humans to grasp. The data is usually much easier to grasp that the processes using the data. We have know this for year, Frederick Brooks pointed it out in <cite>The Mythical Man Month</cite> with this statement, &#8220;Show me your flow charts and conceal your tables and I shall continue to be mystified, show me your tables and I won&#8217;t usually need your flow charts; they&#8217;ll be obvious.&#8221;

<cite><a href='http://users.ipa.net/~dwighth/smalltalk/byte_aug81/design_principles_behind_smalltalk.html'>Dan Ingalls</a></cite> said, &#8220;If a system is to serve the creative spirit, it must be entirely comprehensible to a single individual.&#8221; REST architectures will be more innovative because they can be understood by most single individuals.
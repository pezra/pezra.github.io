---
id: 351
title: HTTP and XMPP
date: 2008-06-03T15:36:02-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=351
permalink: /blog/2008/06/http-and-xmpp/
categories:
  - Uncategorized
---
My new boss is contemplating [whether or not HTTP will remain the protocol of choice in the future](http://one.valeski.org/2008/06/sockets-http-xmpp-and-leap-frog.html). He seems to have reached the conclusion that XMPP is a better protocol than HTTP for the network infrastructure we have today.

> With today&#8217;s connection characteristics, I wonder if HTTP would have been the weapon of choice 15-20 years ago? I doubt it.

Based on this conclusion Jud appears to believe that XMPP will replace HTTP in the future as the protocol of choice. I disagree with Jud on both points.

The Internet is much more reliable today than it has ever been before. It is so good, in fact, that there are many situations where you can trust the network these days. This is particularly true high levels of reliability are not required. However, the network is still not perfect, nor is it likely to ever be. Worse yet, the software that uses the network is still depressingly flaky.

More importantly, I don&#8217;t think HTTP &#8220;won&#8221; because of problems with the network. HTTP is ubiquitous today because it facilitates a programming model that can the use to solve some really hard problems reasonably easily. That programming model an implementation of the [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) architectural style. The constraints of REST allow building highly scalable and reliable systems far more easily than any other approach available today.

In his post Jud speaks of HTTP as if it where transport protocol. As a transport HTTP is not terribly compelling. It has fairly high overhead for individual requests. It&#8217;s connection model usually ends up creating more TCP connections absolutely necessary. It allows a lot of variability in the capabilities of clients. And so on.

However, HTTP is decidedly _not_ a transport protocol. It is an application protocol. HTTP provides a sophisticated set of semantics specifically designed to facilitate the implementation, and optimization, of REST style applications.

XMPP is, on the other hand, is a transport protocol (unlike HTTP). To be precise it is a (near) real-time message transport protocol. If you need that XMPP is an excellent choice. Particularly if the messages you a dealing with have a limited duration of meaningfulness. For example, if your application loses it&#8217;s connectivity to the message sender for any significant period of time it is likely that the application will not receive at least some of the messages sent via XMPP during that time. The server may spool some messages but completely unreasonable to expect APIs to keep track of an arbitrary number of undelivered messages for an arbitrary number of clients. The cost of doing that are just too high for a high volume producer to be able to implement.

I think XMPP will continue to get more penetration and mind share. It is a good protocol. It is not a competitor to HTTP, though. The two protocols serve very different purposes. I expect that many systems will utilized both. If you have a need for real-time messaging and you have relatively weak reliability requirements, or you are willing and able to invest significant effort implementing reliability in your application, use XMPP. But real-time messaging do not an application make.

HTTP is not a &#8220;dinosaur&#8221;, as Jud puts it, it is a shocking advanced piece of alien technology we have only recently discovered (as an industry) how to fully utilize. Actually, I am pretty sure we have not yet figured out how to fully utilized it. We will continue to see more and more applications and data service APIs implemented using the wicked cool semantics of REST/HTTP.
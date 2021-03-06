---
id: 388
title: Push or pull?
date: 2009-05-21T07:10:36-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=388
permalink: /blog/2009/05/push-or-pull/
categories:
  - Software Development
tags:
  - api design
---
The question of how to communicate events often comes up when designing APIs and multi-component systems. The questions basically boils down to this: should events be pushed to interested parties as they occur, or should interested parties poll for new data?

The short answer: interested parties should poll for new data.

The longer answer is, of course, it depends.

[Polling is the only approach that scales](http://www.dehora.net/journal/2008/12/13/design-considerations-for-fine-grained-data-access-via-the-web/) to internet levels. For the smaller scales of internal multi-component systems the answer is much less clear cut. It is clear that a push approach can be implemented in such environments using either [web hooks](http://webhooks.org) or XMPP. Such approaches often appear to be simpler more efficient than the pull equivalents, and they are definitely lower latency.

The appearance of simplicity is an illusion, unfortunately. Event propagation using a push is only easy if are willing to give up a lot of reliability and predictability. It is easy to say &#8220;when an event occurs i will just POST it to registered URI(s)&#8221;. That would be easy, but the world is rarely that simple. What is the receiving server is down or unreachable? Are you going to retry, if so how many times? If not, is that level of message loss acceptable to all the interested parties. If the receiving system is very slow, will that cause a back-log in the sending system? If a lot of events happen in a very short period of time, can the receiving system handle the load?

The efficiency benefits of a push approach are real, but not nearly as significant as they first appear. HTTP&#8217;s conditional request mechanism provides, when used effectively, a way to reduce the cost of polling to quite low levels.

## Pull is cool {#pull_is_cool}

APIs should be built around pulling data unless there is a particular functional concern that makes pull not work (e.g. low message latency being very important). Any push approach with have all the complexities if pull approach (to handle reliability issues) combined with a lot less predictable behavior because it&#8217;s performance will be dependent on one or more other systems ability to handle the event notification work load.
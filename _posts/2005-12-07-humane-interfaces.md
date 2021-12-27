---
id: 186
title: Humane Interfaces
date: 2005-12-07T12:13:29-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=186
permalink: /blog/2005/12/humane-interfaces/
categories:
  - Software Development
tags:
  - Software
---
Martin Fowler has posted a nice article on [humane interface design](http://martinfowler.com/bliki/HumaneInterface.html) (as opposed to minimal interface design). I am definitely on the side of right and good (read: humane interfaces) in this debate. Nothing takes the fun out of programming faster than having to write a bit of code that you know has already been written a bazillion times.<footnote>Even if it is only a few lines code.</footnote> When I program in Java, which exemplifies the minimal interface approach, I feel like this most of the time. I almost never feel that way in Ruby because humane interfaces are deeply ingrained in the community.

Minimal interfaces shift the maintenance burden to the clients. This is great for the library writer because they have less to maintain, but it is devastating for the community. Humane interfaces have the extra behavior because clients need them.<footnote>It is rarely, if ever, appropriate to provide behavior that no one needs, regardless of the interface style style.</footnote> The fact that a minimal interface does not provide an particular bit of behavior does not change the fact that clients need that behavior, it just means that that bit of behavior will be implemented independently in a significant subset of clients. This duplication of common but non-standard behaviors will, over time, significantly increase the total amount of code that a community has to maintain.

This is made worse by the fact that a well designed minimal interface requires fairly small amounts of addition code to implement each individual bit of common behavior. The fact that each of these common behaviors can be implemented quite easily means that they rarely get packaged and reused. For example, you are probably not going to add a dependency to Jakarta Commons IO to your project just to get `RegexFilenameFilter`, you will just implement it yourself because &#8220;it&#8217;s only 5-10 lines of code&#8221; even though someone has already packaged it for you. And you are definitely not going to package and publish that useful 10-15 line utility you wrote last week because cost far outweighs the value of that one utility. Each of those decisions are reasonable in isolation but put together pretty soon you and your community are stuck with a lot more code to maintain than if that behavior had been included in the core library to start with. (See: boiled frog syndrome)
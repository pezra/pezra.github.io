---
id: 121
title: 'RE: Dynamicity and Throwing Money at Problems'
date: 2005-06-13T13:00:06-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2005/06/brian-mccallister/
permalink: /blog/2005/06/re-dynamicity-and-throwing-money-at-problems/
categories:
  - Software Development
tags:
  - LOP
  - Software
---
Brian McCallister has in interesting [post on how XML is used by the Java community](http://kasparov.skife.org/blog/src/java/throwing-money.html). I think he is right about the fact that most of the XML dialects being used in the Java world go against the grain of Java. However, I think they are not being used for dynamicity, at least by his definition. Sure the dynamicity argument is there but it is a red herring because, as Brian points out, no one actually uses the dynamicity. What these Java developers are really doing is creating domain specific languages. They need to do this because, in many cases, the equivalent functionality written in Java would be an excessive amount of code.

And there-in lies the rub, if using plain Java is too expensive and hard to maintain then the logical thing to do is to switch to language in which solving your problem is not too expensive and hard to maintain. Unfortunately, most Java developers either A) are not allowed to even consider using a language other than Java or B) have some emotional ties to Java. If A is true the logical thing to do is to write the language you really need, and want, but to call it &#8220;configuration&#8221; so that it sounds like you are still writing the app in Java. If B is true the logical thing to do is to write the language really need but to call it &#8220;configuration&#8221; so that it sounds like you are still writing the app in Java. Notice that those two are pretty much exactly the same. Either way you end up with a custom programming language specific for your domain and you call it &#8220;configuration&#8221; even though the contents are obviously source code.
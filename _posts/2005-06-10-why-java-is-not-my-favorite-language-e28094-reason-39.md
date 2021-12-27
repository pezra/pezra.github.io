---
id: 120
title: 'Why Java is Not My Favorite Language â€” Reason #39'
date: 2005-06-10T12:28:51-06:00
author: Peter Williams
layout: post
guid: 'http://pezra.barelyenough.org/blog/2005/06/why-java-is-not-my-favorite-language-%e2%80%94-reason-39/'
permalink: '/blog/2005/06/why-java-is-not-my-favorite-language-%e2%80%94-reason-39/'
categories:
  - Software Development
tags:
  - Software
---
Java is early bound. Early binding is an optimization which, like most other optimizations, it is almost always premature. This particular premature optimization causes significant amounts of superfluous code. For example, assume that we the following API for writing to a log file.

<pre class='code'>class Logger
{
    public void log(Object obj) {...}
    public void log(Throwable e) {...}
}
</pre>

`log(Object)` writes the results of `obj.toString()` to the log file and `log(Throwable)` writes `e.getMessage()` and a backtrace. Now I want to wrap this API in my class so that when something goes wrong I can do some clean up, in addition to logging the fact that something went wrong. Sometimes my code detects problems by catching an exception and other times by explicitly checking for certain conditions. So I write the following method and call it every time I detect a problem, either with the exception or with a string that describes the detected problem.

<pre class='code'>private void handleError(Object description)
{
    // do clean up
    myLogger.log(description);
}
</pre>

Because Java is early bound this `handlerError()` method always calls `Logger.log(Object)` even if `description` is actually a Throwable, which means I never get the backtrace in my log file. To make this work correctly I have to write multiple version of the `handlerError()` method or add some `instanceof` checking and casting just to inform the compiler of the object type. Either way that extra code adds no value to my application and I hate writing code that adds no value to my application. Unfortunately, I feel like I end up doing that a lot in Java.
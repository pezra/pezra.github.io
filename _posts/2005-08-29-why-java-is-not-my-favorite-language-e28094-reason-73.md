---
id: 158
title: 'Why Java is Not My Favorite Language â€” Reason #73'
date: 2005-08-29T15:57:49-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=158
permalink: '/blog/2005/08/why-java-is-not-my-favorite-language-%e2%80%94-reason-73/'
categories:
  - Software Development
tags:
  - Software
---
The garbage collector is not observable.

Every non-trivial application I have worked on, regardless of language, has had memory leaks. Even in garbage collected environments memory leaks happen. Given that, why would you design a GC&#8217;d language without providing observation hooks into the garbage collector? With Java your only, standard, option is &#8216;-verbose:gc&#8217; which writes some information to the standard out. Not enough information, mind you, but some.

Unfortunately, I, and everyone else, use java on the server where standard out is useless. Repeat after me, there is no standard out. I could use -Xloggc to get that information logged to a file but that means the the file does not get rolled with the rest of my log files. And I still cannot do real time checking to see what my application is doing when the GC occurs. The whole thing is just so annoying when it could have easily been solved by allowing me to register a GC observer.

<pre class='markdown-html-error' style='border: solid 3px red; background-color: pink'>REXML could not parse this XML/HTML: 
&lt;/p&gt;</pre>
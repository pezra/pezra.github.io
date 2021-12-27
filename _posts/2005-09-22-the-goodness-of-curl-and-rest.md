---
id: 164
title: The goodness of curl (and REST)
date: 2005-09-22T12:01:01-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=164
permalink: /blog/2005/09/the-goodness-of-curl-and-rest/
categories:
  - Software Development
tags:
  - Design
  - Software
  - Web Applications
---
Tim Bray, in an [post about testing atom protocol](http://tbray.org/ongoing/When/200x/2005/09/21/Atom-Protocol), said

<blockquote cite='http://tbray.org/ongoing/When/200x/2005/09/21/Atom-Protocol'>
  <p>
    We got two different clients to talk to it; one was a Big Secret Project from a Big Famous Company based on all sorts of slick infrastructure. Mine was curl. [&#8230;] Those who know what curl is are probably snickering now. But I think the fact that you can debug a nontrivial application with curl -X -i -d -H is a significant weapon in the quiver of RESTafarians.
  </p>
</blockquote>

We are only snickering because we have done the same thing. When you are testing a RESTful application a big sophisticated client mostly just complicates things.

There is a huge advantage to technologies that can be worked with using existing, generalized and scriptable tools. That ability allows for easy testing and debugging, as Mr. Bray points out, but the advantages do not end once you are done debugging.

These days most people are employed to deal with abnormal situation. The normal situations are mostly automated and require very little human intervention. Having software that can be worked with and monitored by existing, scriptable, tools means the you have to write less code to handle those abnormal situations when they arise. If you are a RESTafarian, you just grab curl and a few minutes later you are done. Even better, if that abnormal situation is common enough to warrant it, you can even wrap it up in a script so that anyone can handle it.
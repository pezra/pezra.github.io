---
id: 166
title: The Power of Ruby (and Semantic Markup)
date: 2005-09-28T12:48:55-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=166
permalink: /blog/2005/09/the-power-of-ruby-and-semantic-markup/
categories:
  - Software Development
tags:
  - Software
  - Web Applications
---
I haven&#8217;t written much about Ruby lately. That is mostly because the piece of software I am working on is written in Java, with a little PHP on the side. I do write Ruby code, though mostly in the form of small scripts. In the last few months, I have started using Ruby in places where in the past I would have used shell scripting. It turns out that just about anything you can do with a unix shell you can do with Ruby more easily and faster. And there are a lot of things you can do with Ruby that just cannot done, easily, with a shell.

For example, yesterday we had a minor issue here at work. We fixed the problem quickly but there were are few orders that we needed to watch, just to make sure there were no residual issues. So I grabbed my trusty Emacs and IRB and wrote a little, 50-ish lines long, ruby script to mash-up information from a little [web application](http://pezra.barelyenough.org/blog/2005/07/the-joy-of-programming/) and a bunch of files on disk to let me easily keep track of how things were progressing. I would not have even tried that with shell scripting (or with any other language I know, for that matter) but it saved several hours and completely removed the risk that something important would slip through the cracks.

This mash-up was almost completely based on open-uri and REXML. Both open-uri and REXML are petty sweet, but together they are enough to send you into diabetic shock from across the room. If you are not familiar with them already you definitely should be. Those two libraries combined with the semantic markup used by the web app (or should I service?) made the whole thing fairly straight forward.
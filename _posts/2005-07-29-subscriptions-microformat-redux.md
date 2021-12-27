---
id: 144
title: Subscriptions Microformat (Redux)
date: 2005-07-29T10:34:18-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2005/07/subscriptions-microformat-redux/
permalink: /blog/2005/07/subscriptions-microformat-redux/
categories:
  - Software Development
tags:
  - Microformats
  - Software
---
Previously I proposed a mircoformat for [describing blog subscriptions](http://pezra.barelyenough.org/blog/2005/07/subscriptions-microformat/). Ryan King pointed out in a comment that there is one already, [Attention.XML](http://developers.technorati.com/wiki/attentionxml). That does solve my problem.

I do see one potential issue with it. There appears to be no way to distinguish an Attention.xml outline from any other XOXO outline. I suppose it might be acceptable to treat any XOXO block that contains links as a set of subscriptions but it feels a bit kludgey to me. Having an Attention.XML blocks declare their class as &#8220;xoxo attentionxml&#8221;, or something similar, would solve this problem, and make Attention.XML blocks a bit more obvious. Or have I just missed something?

Tags: <a href='http://microformats.org/wiki/xoxo' rel='tag'>XOXO</a>, <a href='http://developers.technorati.com/wiki/attentionxml' rel='tag'>Attention.XML</a>
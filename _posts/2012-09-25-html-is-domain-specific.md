---
id: 740
title: HTML is domain specific
date: 2012-09-25T15:54:47-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=740
permalink: /blog/2012/09/html-is-domain-specific/
aktt_notify_twitter:
  - 'yes'
aktt_tweeted:
  - "1"
categories:
  - Software Development
tags:
  - api design
  - hypermedia
  - REST
---
The partisans of generic media types sometimes hold up HTML as an example of how much can be accomplished without domain specific media types. HTML doesn&#8217;t have application/business specific semantics and the whole human facing web uses it, so machine clients should be able to use a generic media type too. There is just one flaw with this logic. **HTML is domain specific in the extreme.** HTML provides strong semantics for defining document oriented user interfaces. There is nothing generic about HTML.

In the HTML ecosystem, the generic format is SGML. Nobody uses SGML out of the box because it is too generic. Instead, various SGML applications, such as HTML, are created with the appropriate domain semantics to be useful. HTML would not have been very successful if it had just defined links via the `a` element (which is all you need to have hypermedia semantics) and left it up to individual web sites to define what various other elements meant.

The programs we use on the WWW almost exclusively use the strongly domain specific semantics of HTML. Browsers, for example, render HTML based to the screen based on the specified semantics. We have web readers which adapt HTML &#8212; which is fundamentally visually oriented &#8212; for use by the visually impaired. We have search engines which analyze link patterns and human readable text to provide good indexing. We have super smart browsers which can often fill in forms for us. They can do these things because of the clear, domain specific semantics of HTML.

Programs don&#8217;t, generally, try to drive the human facing web to accomplish specific application/business goals because the business semantics are hidden in the prose, lists and labels. Anyone who has tried is familiar with the fragility of [web scraping](http://en.wikipedia.org/wiki/Web_scraping). These semantics, and therefore any capabilities based on them, are unavailable to machine clients of the HTML based web because the media type does not specify those semantics. Media types which target machine clients should bear this in mind.
---
id: 208
title: Reading List URIs
date: 2006-02-08T23:53:39-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=208
permalink: /blog/2006/02/reading-list-uris/
categories:
  - Software Development
tags:
  - Software
  - Web Applications
---
<p class='update'>
  Update: Due to a misconfiguration some of my blog entries from the month of Feburary recently lost. This is merely a repost of the original content.
</p>

Danny Ayers has been going on about [grazing the web](http://dannyayers.com/archives/2006/02/06/grazing/)  
lately. I think he is on to something with this meme. Managing a set of  
subscriptions is hard work if you don&#8217;t want to be overwhelmed<footnote>I know because I am failing miserably at keeping my subscriptions to a manageable number.</footnote> and I might be willing to delegate that to someone, or several someones, that I trust. Recently, Mr. Ayers decided to [use del.icio.us to create/manage a technical reading list](http://dannyayers.com/archives/2006/02/07/delicious-reading-lists/)<footnote>I do not use [del.icio.us](http://del.icio.us/) much but I never cease to be amazed at how other people put that system to good use.</footnote>.

Sounds pretty good so far. Well except that [FeedLounge](http://feedlounge.com/) does not support reading lists, but I assume that is only a matter of time before this is rectified. And then I read this:

<blockquote cite='http://dannyayers.com/archives/2006/02/07/delicious-reading-lists/'>
  <p>
    So I&#8217;ll find a few feeds (the feeds, not the blogs) and tag them &#8216;readinglist&#8217;.
  </p>
</blockquote>

My heart falls. I hate this. The fact that the feed and HTML  
versions of most blogs have different URIs has got to be one of the _vilest kludges_<footnote>I think this kludge came about because a) most developer have yet to  
internalize RESTful architecture and b) most web application frameworks  
are exceedingly bad at content negotiation. As a practical matter it is  
probably not as bad as I make it sound, but it just so dumb that it can  
hardly be borne.</footnote>  
to ever exist. It should not, under any circumstances, be encouraged.  
In almost all cases, a feed is just another representation of the blog.  
Therefore, the HTML and feed versions of the blog should have the _same URI_. It is rare, today, to find a blog in which then HTML, RSS and Atom representations share the same URI<footnote>Even  
my blog has a separate URI for each representation. This has bugged me  
for a long time but I have yet to find the time to fix it.</footnote>  
but that does not mean that we should give in. New system, like reading  
lists, should strive to be better than the systems that came before. We  
can start by not muddling our documents with the bad design decisions  
of the past, especially when there is an easy alternative.

Reading lists should point at the _resource_ of interest,  
namely main human readable page of the blog. Aggregators can, and  
should, deal with blog software that does not handle content  
negotiation correctly by requesting the resource with an accept header  
that looks like <samp>application/atom+xml;q=1, application/rss+xml;q=0.9, text/xhtml+xml;q=0.2, text/html;q=0.1</samp>.  
If an html document is returned the aggregator should use the  
&#8220;auto-discovery&#8221; kludge to work around that particular blogging  
software&#8217;s damage. Someday the world will be RESTful and we don&#8217;t need  
a bunch of broken interchange formats, like reading lists that point as  
ugly feed URIs, holding us back.
---
id: 143
title: Subscriptions Microformat
date: 2005-07-28T14:18:26-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2005/07/subscriptions-microformat/
permalink: /blog/2005/07/subscriptions-microformat/
categories:
  - Software Development
tags:
  - Software
  - Web Applications
---
I am really intrigued by the concept of [Microformats](http://microformats.org/wiki/Main_Page). I love the idea of &#8220;presentable and parsable&#8221; formats.

I am a little disappointed, however, that there is not a microformat for describing feed subscriptions. The de facto standard for subscription lists is OPML. I am not sure how this happened but it does not seem particularly well suited for this task. I think a microformat could work much better. After all, I use my subscription list for just two thing, my blogroll (needs to be presentable) and as the subscription list for my aggregator (needs to be parsable). It would be nice if they were the same and I did not have to keep the two synchronized.

I am thinking something like the following

<pre class='code'>&lt;ul class="<abbr title='Open Subscription List'>osl</abbr>" &gt;
  &lt;li class="subscription"&gt;
    &lt;a class="title" href="http://pezra.barelyenough.org/blog" 
             type="text/xhtml" rel="feed"&gt;Peter Williams' Weblog&lt;/a&gt;
    (&lt;a href="http://pezra.barelyenough.org/blog/feed" 
              type="application/rss+xml" rel="feed"&gt;RSS 2.0&lt;/a&gt;)
    (&lt;a href="http://pezra.barelyenough.org/blog/feed/atom" 
              type="application/atom+xml" rel="feed"&gt;Atom&lt;/a&gt;)
  &lt;/li&gt;

  &lt;li class="subscription"&gt;
    &lt;a class="title" href="http://www.toolshed.com/blog/articles" 
             type="text/xhtml" rel="feed"&gt;/\ndy's Blog&lt;/a&gt;
    (&lt;a href="http://www.toolshed.com/blog/xml/rss/feed.xml" 
              type="application/rss+xml" rel="feed"&gt;RSS 2.0&lt;/a&gt;)
  &lt;/li&gt;

&lt;/ul&gt;
</pre>

That results in 

<pre class='markdown-html-error' style='border: solid 3px red; background-color: pink'>REXML could not parse this XML/HTML: 
&lt;samp&gt;
&lt;ul class="osl"&gt;
  &lt;li class="subscription"&gt;
    &lt;a class="title" href="http://pezra.barelyenough.org/blog" type="text/xhtml" rel="feed"&gt;Peter Williams'; Weblog&lt;/a&gt;
    (&lt;a href="http://pezra.barelyenough.org/blog/feed" type="application/rss+xml" rel="feed"&gt;RSS 2.0&lt;/a&gt;)
    (&lt;a href="http://pezra.barelyenough.org/blog/feed/atom" type="application/atom+xml" rel="feed"&gt;Atom&lt;/a&gt;)
  &lt;/li&gt;</pre></p> 

<li class='subscription'>
  <a href='http://www.toolshed.com/blog/articles' class='title' rel='feed' type='text/xhtml'>/\ndy&#8217;s Blog<br /> (</a><a href='http://www.toolshed.com/blog/xml/rss/feed.xml' rel='feed' type='application/rss+xml'>RSS 2.0</a>)
</li>
<pre class='markdown-html-error' style='border: solid 3px red; background-color: pink'>REXML could not parse this XML/HTML: 
&lt;/ul&gt;</pre>

<pre class='markdown-html-error' style='border: solid 3px red; background-color: pink'>REXML could not parse this XML/HTML: 
&lt;/samp&gt;</pre>

I could style this up nicely for my blog and my aggregator could, directly, use my blogroll as it&#8217;s subscription list. Now that would be nice.
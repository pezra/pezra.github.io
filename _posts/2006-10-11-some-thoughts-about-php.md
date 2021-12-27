---
id: 259
title: Some thoughts about PHP
date: 2006-10-11T14:04:35-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2006/10/some-thoughts-about-php/
permalink: /blog/2006/10/some-thoughts-about-php/
tags:
  - PHP
  - Software
  - Web Applications
---
I have been using PHP<sup id='62a9c6bde3b41445c96f3997ce59737a0962e463fnref:1'><a href='#62a9c6bde3b41445c96f3997ce59737a0962e463fn:1' rel='footnote'>1</a></sup> as my primary language for five months now so I feel somewhat qualified to speak about it. My overall conclusion is that PHP is weak sauce. It is easy to get started with PHP but it&#8217;s usefulness decreases as the complexity of the application increases. This is primarily because keeping PHP code maintainable requires unnatural levels of discipline. Choosing PHP for a greenfield project is a technical risk that is unnecessary in today&#8217;s rich web application ecosystem. Of course, most projects aren&#8217;t greenfield so there are plenty reasons to have PHP around.

I find PHP to be a deeply frustrating environment in which to work. It seems to have occurred, as opposed to designed or even evolved, without much of an overarching vision. In &#8220;The Mythical Man-Month&#8221; Fredrick Brooks claims that &#8220;conceptual integrity is _the_ most important consideration in system design&#8221;. Unfortunately, PHP has very weak conceptual integrity. It seems mostly to be a collection of decisions that seemed expedient at the time with little thought about how that would impact the over all system.

I remember hearing a few years ago about PHP is syntax to appear more Java like. That was back when it looked like there was going to be a real Java hegemony in business programming. At the time I thought it was a slightly odd but defensible idea. Today I see that decision as an indicator of weak conceptually integrity. I think any system willing to give up its character so completely must lack, almost by definition, the conceptual integrity needed to be great.

I covered several concrete issues with PHP in [Early impressions of PHP](/blog/2006/05/early-impressions-of-php). All of those issues still stand but my biggest problem with PHP, after working with it for a while, is that is seems designed to actively discourage meta-programming. This means I find myself writing annoying amounts of boiler plate code<sup id='62a9c6bde3b41445c96f3997ce59737a0962e463fnref:2'><a href='#62a9c6bde3b41445c96f3997ce59737a0962e463fn:2' rel='footnote'>2</a></sup>. I strongly believe that the future of programming is [language-oriented](http://en.wikipedia.org/wiki/Language_oriented_programming). This make PHP hard for me because even rudimentary language oriented techniques are simply not feasible in PHP.

A Somewhat more minor annoyance is the lack of closures and blocks. I first learned blocks and closures about two years ago and now find programming without them mildly painful. I think that [Mark Jason Dominus got it right when he said](http://www.theperlreview.com/Interviews/mjd-hop-20050407.html)

> in another thirty years people will laugh at anyone who tries to invent a language without closures, just as they&#8217;ll laugh now at anyone who tries to invent a language without recursion.

There are just so many common classes of problem that are simply and cleanly solved by closures that not having them seems like a crime. I hope it does not take thirty years, though.

It would not be fair to leave this post without a discussion of the good things about PHP. PHP excels at lowering the barrier to entry. There is no other system I am aware of that even come close the ease of getting start with PHP. The weakness of PHP&#8217;s conceptual integrity does not seem to noticeably impact productivity in the context of small systems. The idea that you can have a web application by creating one text file and copying it to the web server is radically powerful.

And then there is [Smarty](http://smarty.php.net). Smarty is a really nice [external DSL](http://www.martinfowler.com/bliki/DomainSpecificLanguage.html) for generating web pages, i.e. a template engine. The core of Smarty is well thought out and has very nice extension mechanisms. I is a joy to work with. If you are doing PHP work I can strongly recommend Smarty.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='62a9c6bde3b41445c96f3997ce59737a0962e463fn:1'>
      <p>
        As always, I need to point out that I am speaking primarily about PHP 4. I suspect that PHP 5 suffers from many of these issues but I have spent very little time in PHP 5.
      </p>
      
      <p>
        <a href='#62a9c6bde3b41445c96f3997ce59737a0962e463fnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='62a9c6bde3b41445c96f3997ce59737a0962e463fn:2'>
          <p>
            Usually this boiler plate code gets written after I have already spent an inordinate amount of time tyring and failing to automate it. The usable area in PHP seems remarkably small. That means it is going to take a bit more running into the edges before I completely accept that they are really that close.
          </p>
          
          <p>
            <a href='#62a9c6bde3b41445c96f3997ce59737a0962e463fnref:2' rev='footnote'>&#8617;</a></li> </ol> </div>
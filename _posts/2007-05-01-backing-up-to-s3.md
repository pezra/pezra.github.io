---
id: 276
title: Backing up to S3
date: 2007-05-01T15:50:31-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/05/backing-up-to-s3/
permalink: /blog/2007/05/backing-up-to-s3/
categories:
  - Software Development
tags:
  - REST
  - Web Applications
---
I recently setup an automated backup system for my (and my [wife&#8217;s](http://barelyenough.org/notablog)) blog.<sup id='54d4964a22376e03e7b2d3a04a31dcc3b7f0e63cfnref:1'><a href='#54d4964a22376e03e7b2d3a04a31dcc3b7f0e63cfn:1' rel='footnote'>1</a></sup> Based on the [recommendation of Mr O&#8217;Grady](http://redmonk.com/sogrady/2007/01/13/itreport_backup/) (and my belief that RESTful architectures are a good way to solve most problems) I decided to use [Amazon&#8217;s S3](http://www.amazon.com/gp/browse.html?node=16427261) as the off site storage. I did not to take the same approach as RedMonk, however, because I wanted to play with S3 a bit more directly.

After playing with it I have to say that I am very impressed. S3&#8217;s RESTful API is powerful while being simple enough get started with right away. The Ruby [AWS::S3](http://amazon.rubyforge.org/) library makes it even easier to get started by providing a nice, idiomatic, wrapper around S3&#8217;s functionality.

My backup solution ended up being a 20 line ruby script<sup id='54d4964a22376e03e7b2d3a04a31dcc3b7f0e63cfnref:2'><a href='#54d4964a22376e03e7b2d3a04a31dcc3b7f0e63cfn:2' rel='footnote'>2</a></sup> that dumps a database, compresses the dump and then pushes it to S3. That combine with a couple of crontab entries and I was done.

It gets better, though. I got my first bill today:

> Greetings from Amazon Web Services,
> 
> This e-mail confirms that your latest billing statement is available on the AWS web site. Your account will be charged the following:
> 
> Total: $0.02
> 
> Please see the Account Activity area of the AWS web site for detailed account information:

So there you go, a secure remote backup for only 2 cents (and a couple of hours of my time). I think these web service things may be around to stay.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='54d4964a22376e03e7b2d3a04a31dcc3b7f0e63cfn:1'>
      <p>
        I cannot believe it took me so long to get around to that.
      </p>
      
      <p>
        <a href='#54d4964a22376e03e7b2d3a04a31dcc3b7f0e63cfnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='54d4964a22376e03e7b2d3a04a31dcc3b7f0e63cfn:2'>
          <p>
            That 20 lines includes nice command line argument parsing, too, thanks to <a href='http://groups.google.com/group/ruby-talk-google/browse_thread/thread/88bf54ad98a769ca'>Main</a> (maybe when it grows up it will get a website of it&#8217;s own).
          </p>
          
          <p>
            <a href='#54d4964a22376e03e7b2d3a04a31dcc3b7f0e63cfnref:2' rev='footnote'>&#8617;</a></li> </ol> </div>
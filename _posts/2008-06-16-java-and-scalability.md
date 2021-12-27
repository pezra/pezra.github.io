---
id: 352
title: Java and Scalability
date: 2008-06-16T21:20:03-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=352
permalink: /blog/2008/06/java-and-scalability/
categories:
  - Software Development
tags:
  - Java
---
Every time I hear someone say that Java is &#8220;scalable&#8221; my initial reaction is to kick the person who said it in the shin.

I have been talking to a lot of people lately about the tools we are using at [Gnip](http://gnipcentral.com). Every time I tell someone that major parts of our system are written in Java the response seems to be, &#8220;Oh, for it&#8217;s scaling capability?&#8221; While I was safely ensconced in the Ruby world I had hope that this malformed meme was dead. It seems that in the wider world it&#8217;s not quite dead.

I never actually kick the person, by the way. Instead I just sigh and explain that, no that is not the reason. Scalability _cannot_ be the reason we use Java, because Java does not scale any better, or worse, than any other general purpose language.

There are a variety of different sorts of scalability. The most interesting type of scaling in the context of web applications, like Gnip, is how easily can you increase the number of requests/sec the system can handle. This sort of scalability, or lack thereof, derives pretty much entirely from the architecture of the system. No language will magically make your system be able, or unable, to handle an order of magnitude increase in the number of requests.

The culture<sup id='fnref:1'><a href='#fn:1' rel='footnote'>1</a></sup> of Java actually encourages the development of mediumly, rather than highly, scalable systems. It does this by favoring the use of multi-threading, shared state, vertical scaling and large monolithic components. These techniques do not scale infinitely. Fortunately, Java is fast enough that they can scale to quite significant levels. Even though the culture of Java encourages these less than perfectly scalable techniques you can build highly scalable systems with Java quite readily. You just have to be willing to buck the culture when it is appropriate.

Performance, on the other hand, does derived, to a significant degree, from your language,<sup id='fnref:2'><a href='#fn:2' rel='footnote'>2</a></sup> and that is why we use Java.

<div class='footnotes'>
  <hr />
  
  <hr />
  
  <ol>
    <li id='fn:1'>
      <p>
        Every language has a set of idioms and practices that it, and it&#8217;s community, implicitly encourage. This set of idioms and practices are what I mean by culture.
      </p>
      
      <p>
        <a href='#fnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='fn:2'>
          <p>
            I really wish this were not the case. I don&#8217;t think it has to be this way but today Java is a lot faster that most of the languages I really like.
          </p>
          
          <p>
            <a href='#fnref:2' rev='footnote'>&#8617;</a></li> </ol> </div>
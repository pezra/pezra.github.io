---
id: 308
title: How REST Can Relieve Your (Lack of) Documentation Guilt
date: 2007-11-30T14:11:14-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/11/how-rest-can-relieve-your-lack-of-documentation-guilt/
permalink: /blog/2007/11/how-rest-can-relieve-your-lack-of-documentation-guilt/
categories:
  - Software Development
tags:
  - architecture
  - documentation
  - REST
  - Software
---
A couple of months ago we hired a contractor to write a reporting interface for our high volume monitoring system. Our system exposes all of it&#8217;s data in RESTful web services, and his job has been to take that data and allow users to create reports based on it.

This morning a couple of my teammates and I asked him if he thought our documentation was sufficient to allow supplementary applications, like the one he was finishing up, to be written without having direct access to the developers. He replied to this effect,

> To be honest, I did not really look at the documentation. I just fetched the URLs you gave me, and the ones I found in those documents, and so on. It did not take me very long to get a pretty good idea of what kind of data was available and where.

That ability to understand a large system by simply exploring it is one of the most powerful features RESTful architectures. But only if you are using all the precepts of REST, including that resources are represented by documents with links<sup id='395ec1948751f75f130cb0e9d1b30a5bf20e0a34fnref:1'><a href='#395ec1948751f75f130cb0e9d1b30a5bf20e0a34fn:1' rel='footnote'>1</a></sup> (or, hypermedia is the engine of application state, if you prefer a more traditional phrasing).

A RESTful architecture will let you scale, and distribute your application beyond all reasonable expectations. But even better, since you know that anyone who cares can just go exploring, it will also let you feel less guilty about not writing all that documentation that you never quite get around to.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='395ec1948751f75f130cb0e9d1b30a5bf20e0a34fn:1'>
      <p>
        Hat tip to <a href='http://www.innoq.com/blog/st/2007/11/08/qcon_sf_sanjiva_weerawarana_ws_vs_rest_mashing_up_the_truth_from_facts_myths_and_lies.html'>Stefan Tilkov</a> for either reporting that Sanjiva Weerawarana used this phrase in is QCon 07 presentation, or for coining that phrase himself (I cannot tell for sure which it was).
      </p>
      
      <p>
        <a href='#395ec1948751f75f130cb0e9d1b30a5bf20e0a34fnref:1' rev='footnote'>&#8617;</a></li> </ol> </div>
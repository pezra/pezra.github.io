---
id: 277
title: GRDDL
date: 2007-05-17T22:42:27-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/05/grddl-is-smart/
permalink: /blog/2007/05/grddl-is-smart/
categories:
  - Software Development
tags:
  - Microformats
  - Web Applications
---
I have been watching the [Semantic Web](http://en.wikipedia.org/wiki/Semantic_Web) efforts with guarded interest for the last few years. I really like the idea. However, I have always thought it was probably a pipe dream. The Semantic Web is a chicken and egg problem, there must be a lot of data published to attract the general developer population but it needs to attract the general developer population to get a lot of data published.

[RDF](http://www.w3.org/RDF/), [SPARQL](http://www.w3.org/TR/rdf-sparql-query/) and the other Semantic Web technologies are pretty uniformly wicked cool. Unfortunately, they are also rather unlike the technologies with which most developers are familiar. I has never obvious to me how we, as an industry, could get to the Semantic Web from here. But today I became aware of [GRDDL](http://www.w3.org/TR/grddl/)<sup id='39376a21ae447ea2a8f6516a91da5c685abde381fnref:1'><a href='#39376a21ae447ea2a8f6516a91da5c685abde381fn:1' rel='footnote'>1</a></sup>, which is the path to the Semantic Web.

As I understand it, GRDDL amounts to this: publish your data in what ever format you like but include a link to an [XSLT](http://www.w3.org/TR/xslt) transform that will convert your published format into an RDF document. So you can continue to publish your [microformatted](http://microformats.org/) HTML document and be part the Semantic Web just by adding a link element.

My initial reaction to GRDDL is an exquisite combination of &#8220;man, there are some really smart people in the world&#8221; and &#8220;duh, why did I not see that&#8221;. That set of feelings is usually a strong indication of a good idea.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='39376a21ae447ea2a8f6516a91da5c685abde381fn:1'>
      <p>
        via <a href='http://dig.csail.mit.edu/breadcrumbs/node/194'>Dan Connolly</a>
      </p>
      
      <p>
        <a href='#39376a21ae447ea2a8f6516a91da5c685abde381fnref:1' rev='footnote'>&#8617;</a></li> </ol> </div>
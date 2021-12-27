---
id: 136
title: PHP and Pretty URIs
date: 2005-07-09T22:56:03-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2005/07/php-and-pretty-uris/
permalink: /blog/2005/07/php-and-pretty-uris/
categories:
  - Software Development
tags:
  - Software
---
The W3C has had published [guidelines](http://www.w3.org/Provider/Style/URI) for designing URIs since 1998. On the public internet they even seem to be followed reasonably well, but the applications on company intranets generally seem to have depressingly ugly URIs. I think this is because many web application servers make it more difficult to have pretty URIs than to have ugly ones &#8212; which really sucks. When you are deploying an app to the public the pretty URI argument is more compelling, which basically means that time is rarely devoted to prettying-up the URIs of internal tools.

I am writing this fresh from my first encounter with PHP. PHP seems very useful, except for it&#8217;s URI handling. In PHP it is really easy to get information from the client that is sent in the query string but that pretty much dooms to to having ugly URIs. The right way to deal with this, IMHO, is to add information to the path-info portion of the URI. For example, if you have a URI &#8220;http://tools.mydomain.com/workflow/processes.php/fulfillOrder/1523&#8221; the &#8220;/fulfillOrder/1523&#8221; part is the path-info portion of the URI (same for for CGIs). (This URI does break at least one pretty URI rule, it has a &#8216;.php&#8217; part, but you can always remove that part with a _simple_ mod_rewrite rule.) The path-info portion of the request URI is passed into the PHP script but there is an complete lack of tools to help with interpreting it.

There are not even functions to normalize it (remove dots and repeated slashes and remove levels for double dots) and to split it into it&#8217;s component parts, which I think are the bare minimum functionalities required to use the path-info safely. It is no wonder that internal applications URIs suck. To make them not suck you have to write a bunch of code. Either lots of complicated mod_rewrite rules or some really boring URI path manipulation functions in PHP. Not that this problem is exclusive to PHP, I think that many of the web application frameworks out there have similar, or worse, hurdles for pretty URIs. (Even the web application server I helped design completely prevents the use of pretty URIs, a decision for which I apologize.)

Anyway, I guess what I am trying to say is that if you design a web application server, please make it easy to use pretty URIs. I hate to use ugly URIs but I have a hard time convincing my boss that pretty URIs are worth the money for internal projects.
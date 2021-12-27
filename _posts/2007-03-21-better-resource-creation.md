---
id: 271
title: Better resource creation
date: 2007-03-21T11:07:02-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/03/better-resource-creation/
permalink: /blog/2007/03/better-resource-creation/
tags:
  - REST
  - Software
  - Web Applications
---
[It seems that I was a little unclear](http://www.innoq.com/blog/st/2007/03/20/deprecating_post_or_maybe_not.html) in my post about [using the HTTP PUT method for resource creation](http://pezra.barelyenough.org/blog/2007/03/deprecating-post/), so let me try again.

In [this post](http://soundadvice.id.au/blog/2007/03/18#deprecatingPOST) Benjamin Carlyle points out that using POST for resource creation has some serious flaws. To see the main problem with using POST for resource creation consider the following scenario.

You make a POST request to create a new purchase order resource but you don&#8217;t get a response. One of two things may have happened.

  1. the server got your request and create the purchase order

  2. the server never received the request, or crashed before it was able to create the new purchase order

If 1 happened you _don&#8217;t_ want to re-issue the new purchase order request because that would double the order you just made. If 2 happened you _do_ want to re-issue the new purchase order request otherwise you are not going to get the stuff you are trying to order. Unfortunately, with POST base resource creation there is no way to tell in which way the request failed, and therefore no way to decide what to do to resolve the problem without some outside (read: human) input.

It should be noted that when a human is already involved (such as in a user facing web app) this is really not much of a problem. If you try to create a new blog post but don&#8217;t get a response you just go look at your blog to see if actually worked or not and then do the appropriate thing. However, when you get into computer-to-computer interactions this issue is a big deal, and in many situation it borders on completely unacceptable.

### An alternate approach {#an_alternate_approach}

One way to handle resource creation that does not suffer from these issues is to use PUT instead of POST. PUT request are idempotent, by definition, so anytime you make PUT request and don&#8217;t get a response you can just re-issue the request until you do. Using that characteristic to implement resource creation would look something like this:

  1. A client needing to create a new purchase order makes a PUT request, containing a representation of the purchase order to create, to a well know new resource URI (something like `http://example.com/purchase-orders/new`)

  2. The server generates a new URI to reference this new purchase order, using whatever mechanism it chooses, and responses with redirect to this new URI.

  3. The client re-issues the PUT request containing the purchase order representation to the newly generated URI.

  4. The server stores the new purchase order and responses with a &#8220;201 Created&#8221;

In this scenario there are two points at which you could get no response from the server, but at each of those points there is a exactly one correct thing to do to resolve the failure. If you don&#8217;t get a response to the initial PUT to the &#8220;new resource&#8221; URI you can just re-issue it until you do get a response. Because the server never actually creates a resource at this point it does not matter if the server processed your request an the response got lost, or if your request never made it to the server. By re-issuing the request the worst thing that can happen is that a few URIs are generated that will never get used but URIs free so that is no problem at all. Once you have the redirect from the initial &#8220;new resource&#8221; request you can re-issue the PUT against the redirect URI. If no response is received you can simply re-issue the request until you do get one. The idempotence guarantee of PUT requests means that making the same request multiple times has the same effect as making it once.

This approach is adapted from a very similar [approach suggested by Benjamin Carlyle](http://soundadvice.id.au/blog/2007/03/18#deprecatingPOST). My primary concerns with the approach Mr. Carlyle suggested is that it forces the client of a RESTful web service to understand the URI generation scheme used by the server or the server to understand a URI generation scheme it has no intention of actually using. The GUID based URIs Mr. Carlyle suggests is workable but I think it forces far too much knowledge about the implementations of the client and server into each other. This knowledge would cause the server, clients or both to become more complicated than necessary.
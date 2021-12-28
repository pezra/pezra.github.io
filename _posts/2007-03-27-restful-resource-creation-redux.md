---
id: 272
title: RESTful resource creation (redux)
date: 2007-03-27T00:04:11-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/03/restful-resource-creation-redux/
permalink: /blog/2007/03/restful-resource-creation-redux/
tags:
  - REST
  - Software
  - Web Applications
---
[Benjamin Carlyle has posted a followup](http://soundadvice.id.au/blog/2007/03/24#deprecatingPOST2) about [using PUT to create new resources](http://soundadvice.id.au/blog/2007/03/18#deprecatingPOST) in which he brings up some interesting issues.

First, it seems I miss understood his original idea slightly. My misunderstanding does not affect how I feel about his approach much. I don&#8217;t like the idea of PUT-ting to a &#8220;factory&#8221; resource with a GUID in the query string any more than I like putting to a GUID based URI that the server might actually be able to use. In fact, I think I might like it less. On the other hand, PUT-ting to a &#8220;factory&#8221; is really the the same same thing I proposed in response to Mr Carlyle&#8217;s original post, I just left off the GUID bit. I find GUIDs to be slightly repulsive and I really don&#8217;t see any need for them in the approaches being discussed.

## Response codes {#response_codes}

Mr Carlyle also points out that the HTTP spec demands a 301 (Moved Permanently) redirect be used if the server wants a PUT applied to a different URI. Unfortunately that does not really match semantics of redirecting from a factory/URL generator resource. This occured to me when I was writing [my proposal for safe resource creation](better-resource-creation) (which is really just Mr Carlyle&#8217;s proposal without the GUID) but I punted and did not even mention the issue. A possible solution might be to use an extension code in the 300 series to mean &#8220;URI Reserved&#8221;. The would mean a PUT request to a factory/URL generation resource would response with

    HTTP/1.1 372 URI Reserved
    Location: http://example.com/yourNewResource

The semantics are nice and clean but it has the disadvantage of being non-standard. This sort of &#8220;extension code&#8221; is explicitly supported in the HTTP spec but it does require that clients be customized to understand it.

## Leveraging POST {#leveraging_post}

[Stefan Tilkov proposes an alternate approach](http://www.innoq.com/blog/st/2007/03/24/posting_reliably.html). His idea involves a POST request to URI generation service. This would then return the new URI in the `Location:` header. Totally workable. It requires a significant, though not disastrous, level of coupling between the server and client. The approach is loses some of it&#8217;s tidiness if the server would like to use the a natural key mechanism for the URIs (and it is positively messy if the server would like to transition from a generated key to a natural key). For the naturally keyed URIs to be generated the POST request to the URI generation service would have to include the the complete representation you want to store. This is fine in practice it makes it look even more like the PUT based approaches.

## What does PUT mean {#what_does_put_mean}

One of the reasons Mr Tilkov does not like the redirected PUT approach to resource creation is that

> it violates the original purpose of PUT, though â€” if I PUT to a > URI, I don't expect it to have different results each time I do so

There is certainly one way to look at the redirected PUT request that is a little out of sync with the canonical PUT semantics but I don&#8217;t think it has any thing to do with the results or the request. The semantic problem I see is that a PUT is a request to store an entity at a particular URI. In the context of redirected PUT based resource creation is that the new entity will never be stored at the initial URI to which it is PUT.

This is not as big an issue as it seems at first glance, however. If you think of the initial URI used for resource creation as pointing to the next unused slot in a collection of resources, rather than it being a resource factory, the semantics line up much more cleanly. From this point of view, PUT-ting an entity to the &#8220;next empty slot&#8221; URI and being redirected to a permanent URI for that slot fits rather nicely with normal PUT semantics. The redirect is necessary because once a slot is spoken for the &#8220;next slot&#8221; URI, by definition, points someplace else.

This way of thinking about a the new resource is similar to a &#8220;latest&#8221; URI. No one would quibble about a resource with a URI like `http://example.com/blog/latest`. The response to a GET of that URI like this would change often and those changes would result from the change in state of some other resource (namely the posts collection resource). The important thing to keep in mind is that the resource in question is just most recent post. Similarly, a &#8220;next slot&#8221; resource always points to the next new member of a resource collection.

If you choose to use this way of thinking about resource creation perhaps a URI whose purpose is slightly harder to confuse would be helpful. Say something like `http://example.com/purchase-order/next-new`, though I am not sure if that is really better.

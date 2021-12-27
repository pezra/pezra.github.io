---
id: 269
title: Deprecating POST
date: 2007-03-19T15:57:04-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/03/deprecating-post/
permalink: /blog/2007/03/deprecating-post/
categories:
  - Software Development
tags:
  - REST
  - Software
  - Web Applications
---
[Benjamin Carlyle has an interesting bit](http://soundadvice.id.au/blog/2007/03/18#deprecatingPOST) about the possibility of deprecating the HTTP POST method. I think most people who have thought deeply about RESTful architectures have had similar thoughts. GET, PUT and DELETE are all nicely [idempotent](http://en.wikipedia.org/wiki/Idempotence_%28computer_science%29), but POST is not. GET, PUT, and DELETE have clean, well defined semantics, but POST does not. POST generally seems a little out of place next to it&#8217;s cleaner cut cohorts. However, there is an absolute requirement for the &#8220;process this&#8221; semantics of POST so deprecating it is completely out of the question. On the other hand, it does get used in some situations where there are better approaches.

POSTs lack of idempotence has some nasty side effects particularly for what is probably the most common use of POST today, new resource creation. Consider the following scenario, you POST a request to create a new resource but you don&#8217;t get a response. It is impossible to automatically recover from this scenario. You cannot resend the request because the new resource may have been created and you just did not get the response and you cannot check to see if the resource was created because you don&#8217;t know the URI it would have been assigned if it had, in fact, been created.

I think Mr. Carlyle is correct that using POST for resource creation is sub-optimal. He suggest the following approach<sup id='49b55d5dbbb95943a4d2665d529adb1754b79cacfnref:1'><a href='#49b55d5dbbb95943a4d2665d529adb1754b79cacfn:1' rel='footnote'>1</a></sup>, suppose you have a new purchase order resource you want to make known to the server. The client generates a GUID and the issues the following request

    PUT /purchaseOrders/76fd9473-a270-4aac-8a06-e5265048cbbc HTTP/1.1
    Host: example.com
    Content-Length: ....
    
    <PurchaseOrder>
    ...
    </PurchaseOrder>

The server thinks &#8220;I do not know of a purchase order with an id of 76fd9473-a270-4aac-8a06-e5265048cbbc so this request is regarding a never before seen purchase order&#8221; then the go about storing that brand new purchase order and returns a &#8220;201 Created&#8221; response. From are RESTful point of view this is a reasonable approach. It relies on only standard PUT semantics, is resource focused and is idempotent so you can safely keep repeating the request until you get a response.

While relatively straight forward the approach does requires a lot of out of band information because the client has to understand the servers id generation scheme and be able to reliably generate a new id that is unique. If you are using GUIDs as the resource id&#8217;s you can be reasonably assured that clients can, in fact, generate globally unique ids so there are not really a practical short term problem.

Some potential problems arise when you have a server that uses a scheme for which ids cannot be reliably generated on the client, such as monotonically-increasing numbers, or where there would be a different way to figure the id for each type of resource, such as natural keys. It also tightly couples the clients to the server implementation in ways I think are dangerous. For example, what happens if you decide you would like to move to an URL scheme that is less ugly than GUIDs?

For all those situations Mr. Carlyle proposes continuing to use a GUID based URI for resource creation PUTs but rather than actually creating the resource, having the server response with a permanent redirect to a new URI at which the resource should exist. The client would then re-issue the PUT request to the new URI and only then would server create the new resource.

I really like the general arc of this proposal but I despise the GUID part. GUIDs are _ugly_ and they offend my sense of elegance. However, this proposed approach can be easily tweaked into something I really like. Suppose that rather using GUID based URIs you instead used a &#8220;new resource&#8221; URI. For example you could say

    PUT /purchaseOrders/new HTTP/1.1
    Host: example.com
    Content-Length: ....
    
    <PurchaseOrder>
    ...
    </PurchaseOrder>

the server would respond with

    HTTP/1.1 303 See Other
    Location: http://example.com/purchaseOrders/any_random_sort_of_id_the_server_wants
    ...

and the client would re-issue the PUT to the URI specified in the redirect. This pushes URI generation back to the server, where it belongs, while still retaining the general goodness of having all interactions between the client and server be idempotent.

It&#8217;s just too bad this approach does not work in a browser<sup id='49b55d5dbbb95943a4d2665d529adb1754b79cacfnref:2'><a href='#49b55d5dbbb95943a4d2665d529adb1754b79cacfn:2' rel='footnote'>2</a></sup>.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='49b55d5dbbb95943a4d2665d529adb1754b79cacfn:1'>
      <p>
        I rephrase it here to make sure I really understand it and because I found the examples in the original a little hard to follow.
      </p>
      
      <p>
        <a href='#49b55d5dbbb95943a4d2665d529adb1754b79cacfnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='49b55d5dbbb95943a4d2665d529adb1754b79cacfn:2'>
          <p>
            According to <a href='http://www.w3.org/Protocols/rfc2616'>RFC 2616</a> (section 10.3) a redirection
          </p>
          
          <blockquote>
            <p>
              MAY be carried out by the user agent without interaction with the user if and only if the method used in the second request is GET or HEAD.
            </p>
          </blockquote>
          
          <p>
            So while technically you could do this in a browser the user experience would suck.
          </p>
          
          <p>
            <a href='#49b55d5dbbb95943a4d2665d529adb1754b79cacfnref:2' rev='footnote'>&#8617;</a></li> </ol> </div>
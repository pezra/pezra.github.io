---
id: 335
title: Versioning REST Web Services
date: 2008-05-11T23:59:37-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=335
permalink: /blog/2008/05/versioning-rest-web-services/
openid_comments:
  - 'a:2:{i:0;s:5:"85653";i:1;s:5:"85764";}'
aktt_notify_twitter:
  - 'yes'
categories:
  - Software Development
tags:
  - REST
  - rest-versioning
---
Managing changes to APIs is hard. That is no surprise to anyone who has ever maintained an API of any sort. Web services, being a special case of API, are susceptible to many of the difficulties around versioning as other types of APIs. For HTTP based REST style web services the combination of resources and content negotiation can be used to mitigate most of the issues surrounding API versioning.

Let&#8217;s assume you have a REST/HTTP web service that has some account resources. Say you can make a request like this:

    ===>
    GET /accounts/3 HTTP/1.1
    Accept: application/vnd.mycompany.myapp+xml
    <===
    HTTP/1.1 200 OK
    Content-Type: application/vnd.mycompany.myapp+xml
    
    <account>
      <name>Inigo Montoya</name>
    </account>

First, you probably noticed that my example uses a [vendor MIME media type](http://tools.ietf.org/html/rfc4288#section-3.2) to describe the representation. Using a more generic MIME media type like `application/xml` is much more common, at least in my experience. Using generic media types is perfectly legal but a bit silly. You are not really asking for any old XML document, but rather an XML document that has a quite specific schema. Aside from my idealistic rantings, using a specific media type has some strong practical benefits which are at the core of this post.

## Backwards compatible changes {#backwards_compatible_changes}

Often changes will need to be made to expose new behavior of the system that do not negatively impact correctly implemented clients. Say, for example, you want to start tracking email address for accounts. If the `application/vnd.mycompany.myapp+xml` format documentation is clear that elements that are not recognized should be ignored you can simply add a email element to the account representation.

    <account>
      <name>Inigo Montoya</name>
      <email-address>mailto:prepare-to-die@youkilledmyfather.example</email-address>
    </account>

Any client that was created before the addition of the email element will simply ignore it&#8217;s presence. Problem solved.

## Incompatible changes {#incompatible_changes}

Unfortunately, not all changes can be implemented in a way that is backwards compatible. For example, a couple of months after adding email to accounts the sales team sign a deal for 1 bazillion dollars. But the new customer demands that each account be allowed to have more than one email address. After thinking for a while, you decide that the best way to expose that is by changing the account representation as follows.

    <account>
      <name>Inigo Montoya</name>
      <email-addresses>
        <email-address priority=&#39;1&#39;>mailto:prepare-to-die@youkilledmyfather.example</email-address>
        <email-address priority=&#39;2&#39;>mailto:vengeance@youkilledmyfather.example</email-address>
      <email-address>
    </account>

That, of course, will break any clients that are expecting the old format &#8211; so pretty much all of them. This is a place where we can bring content negotiation to bear. You can simply define a new media type &#8211; say `application/vnd.mycompany.myapp-v2+xml` &#8211; and associate new multi-email format with it. Clients can then request whichever format they want. Older clients don&#8217;t know the new media type so they get served the older single email format.

    ===>
    GET /accounts/3 HTTP/1.1
    Accept: application/vnd.mycompany.myapp+xml
    <===
    HTTP/1.1 200 OK
    Content-Type: application/vnd.mycompany.myapp+xml
    
    <account>
      <name>Inigo Montoya</name>
      <email-address>mailto:prepare-to-die@youkilledmyfather.example</email-address>
    </account>

Newer clients do know the new media type so they can have access to the new functionality.

    ===>
    GET /accounts/3 HTTP/1.1
    Accept: application/vnd.mycompany.myapp-v2+xml
    <===
    HTTP/1.1 200 OK
    Content-Type: application/vnd.mycompany.myapp-v2+xml
    
    <account>
      <name>Inigo Montoya</name>
      <email-addresses>
        <email-address priority=&#39;1&#39;>mailto:prepare-to-die@youkilledmyfather.example</email-address>
        <email-address priority=&#39;2&#39;>mailto:vengeance@youkilledmyfather.example</email-address>
      <email-address>
    </account>

Everyone gets what they need. Easy as pie.

## Alternate approaches {#alternate_approaches}

The most commonly proposed approach for versioning REST/HTTP web service interfaces today seems to be to mutilate the URIs by inserting a version. For example,

    http://foo.example/api/v1/accounts/3

I really hate this approach as it implies that an account in one version of the API is really a different resource than the account in a different version of the API.

It also forces clients into a nasty choice, either support multiple versions of the API simultaneously or break one of the core constrains of REST. For example, say a client exists for the v1 API that saves references (URIs that include the version indicator) to accounts. Some time later the client is updated to support the new version of the API. In this situation the The client can support both versions of the API simultaneously because all the previously stored URIs point at the old version of the API or it has to mung all the URIs it has stored to the point at the new API. Munging all the URIS breaks the [HATEOAS](http://pezra.barelyenough.org/blog/2007/05/hypermedia-as-the-engine-of-application-state/) constraint of REST and supporting multiple versions of the API is a maintenance nightmare.

## Conclusion {#conclusion}

Changes to REST/HTTP web service interfaces come it three basic flavors, changes to the properties associated with a type of resource, additions of new types of resources and deprecation of obsolete types of resources. If you are following the [HATEOAS](http://pezra.barelyenough.org/blog/2007/05/hypermedia-as-the-engine-of-application-state/) constraint of REST the approach described here can be used to safely handle all three scenarios.

This approach does lead to media types being created, but media types are cheap so we can &#8211; and should &#8211; have as many as we need. Used properly, content negotiation can be used to solve the problems related to versioning a REST/HTTP web service interface.

* * *

#### Related Posts {#related_posts}

If you&#8217;re interested in REST/HTTP service versioning be sure not to miss the [rest of the series.](/blog/tag/rest-versioning/)
---
id: 674
title: Media types and profiles
date: 2012-06-25T05:00:11-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=674
permalink: /blog/2012/06/media-types-and-profiles/
aktt_notify_twitter:
  - 'yes'
aktt_tweeted:
  - "1"
categories:
  - Software Development
tags:
  - REST
  - rest-versioning
---
[Opponents of API versioning using media types often suggest that media type proliferation is a cause for serious concern](http://www.mnot.net/blog/2012/04/17/profiles). The implication is that the more media types that exist, the more different formats intermediates and tools will need to understand in order to be useful. Fortunately, this is just not true. Having lots of media types does **not** imply having a lot of incompatible formats. Nor does it imply requiring tooling and intermediates to be a lot more complex.

This is difficult to come to terms with, in part, because most media types are not just one thing. The [Atom](http://tools.ietf.org/html/rfc4287) feed for this blog is simultaneously an article syndication document, an XML document processable by any compliant XML parser, a UTF8 encoded plain text document and a octet stream. All of those are media types. It would be perfectly legal to return an Atom document but set the `Content-Type` to `text/plain`, but we generally choose to request and identify Atom feeds using the Atom media type because that provides the most value to the client making the request. Notice that we choose the most &#8212; not the least &#8212; specific media type.

A downside of using the most specific media type available is that some intermediates are put at a disadvantage. If some intermediate is able to do something useful with XML but does not understand that Atom is XML it might not do that useful thing with our request. On the other hand, using a less specific media type might have the same disadvantage. If we call our Atom document an octet stream intermediates are going to pretty much ignore it. We have a stack of formats each of which is a compatible extension of the all the ones below it, but we are only allowed to give it one name. This is bound to leave some components unable to work optimally.

Only being able to specify a single name is the root of the problem, not having lots of compatibly layered formats. Fortunately, the profile link relation provides a solution. You just have to use it in a slightly different way than its proponents currently suggest.

## Very specific media types

The client constructing the requests needs to be able to tell the server what it needs to accomplish its goal. If it can work with any old octet stream it can put `*/*` in the `Accept` header field. If, on the other hand, it is expecting specific information to be provided in an element with a specific id then it needs to be able to let the server know that. A very specific media type combined with content negotiation is great way to provide this while still allowing substantial flexibility to servers.

## Use profile link header for more generic format information

Rather than prevent clients from asking for what they need, servers should decorate responses with profile link headers that provide hints about alternate ways a representation could be processed. This provides intermediates and tooling a way to identify representations they can work with, regardless of what the `Content-Type` header field says.

Consider an API that uses a media type based on Atom but with extensions. It could register a very specific media type in the vendor tree for that particular flavor of atom and use that in `Content-Type` header field. In addition, it could provide link headers pointed to `http://tools.ietf.org/html/rfc4287` (Atom), `http://www.w3.org/TR/2006/REC-xml11-20060816/` (XML), and some URI representing plain text. Clients that need the specific extensions to work can make that known. Clients that can work with any old Atom document can request Atom documents and get them with or without the extensions. Intermediates that work with any Atom document can easily detect that very specific media typed responses are, in fact, Atom so they can do their job. And if at some later date a standard way to represent this data emerges the API can add support for it without breaking any of its existing clients, direct or implicit.

## Background

This particular line of thinking was prompted by [Peter Janes pointing out that profiles and media type based versioning might be complementary](http://peterjanes.ca/blog/2012/06/19/rest-period/).

> Something has been nagging at me about the approaches to REST API versioning presented by [Peter Williams](http://barelyenough.org/blog/tag/rest-versioning/) and [Mark Nottingham](http://www.mnot.net/blog/2012/04/17/profiles). I'm sure they're complementary, but I'm not quite grokking how.

That insight really got me thinking. The impacts to intermediates of media type versioning has been a nagging issue for me for a while now and i am happy to finally have a solution.

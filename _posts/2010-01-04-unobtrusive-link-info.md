---
id: 416
title: Unobtrusive link info
date: 2010-01-04T23:19:14-07:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=416
permalink: /blog/2010/01/unobtrusive-link-info/
categories:
  - Software Development
tags:
  - JSON
  - REST
  - XML
---
[Mr Amundsen&#8217;s](http://amundsen.com/blog/archives/1023) recent post regarding the design of &#8220;semantic machine media types&#8221; got me thinking about media type design. One of the commonly encouraged practices, particularly on the [REST discuss group](http://tech.groups.yahoo.com/group/rest-discuss), is the use of [`link` elements](http://tools.ietf.org/html/rfc4287#section-4.2.7).

I really dislike this idea. It sets my teeth on edge because it treats links &#8211; which are possibly the most important bits of data in existence &#8211; as second class citizens. It is easiest to show what i mean with a bit of extrapolation:

    <complexElement rel="entry">
      <string rel="id">234132</string>
      <string rel="displayName">Peter Williams</string>
      <complexElement rel="name">
        <string rel="familyName">Williams</string>
        <string rel="givenName">Peter</string>
      </complexElement>
      <complexElement rel="emails">
        <link rel="email" href="mailto:pezra@barleyenough.org"/>
        <string rel="type">personal</string>
      </complexElement>
    </complexElement>

That is what a [portable contact](http://portablecontacts.net/) might look like if we treated all data the way `link` elements work. That example looks pretty ugly to me, as i suspect it does to most people. It is ugly because very important information regarding the role of elements is relegated to a subsidiarity position in favor of fairly unimportant information about its type. However, `link` elements do to links exactly what my example does to all the data. The effect is that properties whose values happen to be independently addressable resources are obfuscated.

The [revealed preference](http://en.wikipedia.org/wiki/Revealed_preference) of the world is against `link` elements. Just look at pretty much any format that embeds application specific semantics. As far as i know, there is not a single widely used format that actually represents its links as `link` elements. Even [atom](http://tools.ietf.org/html/rfc4287) uses properly named elements for most its links. The `link` element it defines is largely relegated to the back water of extensibility.

One benefit that `link` elements have, or at least could have if they where more widely used, is the facilitation of standard link processing tools. Fortunately, we do not have to give up the expressiveness and clarity of [intention revealing names](http://c2.com/cgi/wiki?IntentionRevealingNames) to achieve this result. Rather than obscuring the links we could just treat them as normal data. The additional information needed to support standard tools could be added in a relatively unobtrusive way. Consider the following:

    <entry xmlns:link="http://unobtrusive-generic-linking.org/">
      ...
      <emails>
        <value link:hrefDisposition="elementContent" 
               link:rel="foo">
          mailto:pezra@barelyenough.org</value>
        <type>personal</type>
      <emails>
    </entry>

This is idea is similar to [xLink](http://www.w3.org/TR/xlink) but more flexible and simpler to use.

You could expand the idea to JSON with relative ease. Consider the following expansion of the portable contacts json format tagged with some unobtrusive link info.

    {"entry": [
      {"id": "42",
      "emails":
        [{"address"   : "mailto:pezra@barelyenough.org",
          "type"      : "personal",
          "_linkInfo" : {"hrefDisposition" : "address", 
                         "rel" : "foo"}}]}]}

Unobtrusive link info makes links visible to and usable by generic link processing tools while protecting the use of intention revealing names that format designers, and users, want. This is important because it allows new formats to reuse &#8220;standard&#8221; link semantics more easily and uniformly.
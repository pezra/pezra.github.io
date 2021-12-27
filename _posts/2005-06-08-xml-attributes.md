---
id: 119
title: XML Attributes
date: 2005-06-08T17:45:34-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2005/06/xml-attributes/
permalink: /blog/2005/06/xml-attributes/
categories:
  - Software Development
tags:
  - Software
---
XML attributes have always bugged me. When I first learned XML I immediately disliked attributes but I was not able to articulate why I did not like them, it was just a feeling. I think I have finally figured out what exactly I dislike about attributes.

I was searching for some best practices in XML design, in an attempt to convince some people I work with that attributes are evil, and I came across [Refactoring XML: The Attribute Question](http://idealliance.org/papers/dx_xmle04/papers/04-03-02/04-03-02.html#s4). It, rightly, points out that XML attributes are just syntactic sugar. Anything you can express with an attribute you can also express with an element, but not vise-versa, so strictly speaking you do not need attributes. In general, I am all for syntactic sugar, at least the sort that just removes finger-typing by making common choices for you. XML attributes, however, have different semantics than elements, for which they are syntactic sugar. The difference in semantics is a bad smell and has led to endless debates about when and why attributes should and should not be used.

XML would be a lot nicer if attributes were just a short cut for writing an element with simple content. In my perfect world

<pre class='code'>&lt;person name="Fred"/&gt;</pre>

would be exactly equivalent to

<pre class='code'>&lt;person&gt;
  &lt;name&gt;Fred&lt;/name&gt;
&lt;/person&gt;
</pre>

Unfortunately, we are stuck with XML as it stands today, for a while anyway.
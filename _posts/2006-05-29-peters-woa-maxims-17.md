---
id: 237
title: 'Peter&#8217;s WOA Maxims &#8212; #17'
date: 2006-05-29T23:06:07-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=237
permalink: /blog/2006/05/peters-woa-maxims-17/
categories:
  - Software Development
tags:
  - REST
  - Software
  - Web Applications
---
**Never provide representations of static resources by dynamic means.**

Or, serve anything you possibly can directly from a file on a local file system.

There are several reasons for avoiding dynamic mechanisms whenever possible. The most obvious reason is that it is wasteful to generate the identical content repeatedly. If the resource never, or very rarely, changes you can generate the representation(s) once, either at development time or when the application is built/deployed. Even if generating a resource requires little effort it will still result is longer response times, unnecessary server resource (CPU, memory, etc) usage.

The performance impacts of dynamically generating content should not be ignored. However, I think the impacts on ease of comprehension have a much greater effect. Understanding data that is create dynamically is significantly more difficult than static data. For the most part, static data may be understood simply by reading it. Dynamically generated data, on the other hand, requires that both the generated data and that the ways in which that data can vary be understood. Even in cases where the data does not actually vary the fact that it is served via a dynamic mechanism means that its generation must be treated as if the data can vary until the source code has been fully examined. Such examination can range from trivial to fairly hard but it is _never_ free.

Using static representations does raise some practical issues, however. Most web servers tie significance to the pattern of filenames. For example, a file whose name ends with &#8220;.html&#8221; will be treated as a static HTML file and files whose name ends with &#8220;.php&#8221; will be treated as a script that will dynamically generate content. Additionally web servers also directly map URIs to filenames and directory structures. Those two decisions make it difficult to change one&#8217;s mind about the dynamism of a particular resource because if you want to change a representation from static to dynamic, or vice versa, that changes the URI itself.

If you are using Apache 2.x you avoid this issue by using the [MultiViews option](http://tranchant.plus.com/notes/multiviews) which causes Apache to correctly select the appropriate file if the filename extension is not explicitly specified in the URI. Unfortunately, [MultiViews in Apache 1.3.x do not work well enough to completely solve the problem](http://www.gerd-riesselmann.net/archives/2005/04/beware-of-apaches-multiviews) , but [apparently rewrite rules can be used](http://www.gerd-riesselmann.net/node/60#comment-48). Even if a technical fix is not available available this issue is fairly easy to manage. Changes in the dynamism of resources are rare and when they do occur correcting links to point to the new URI will generally only require a search and replace operation.
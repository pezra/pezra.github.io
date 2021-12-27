---
id: 221
title: 'Tonic &#8212; A RESTful Web Application Framework'
date: 2006-03-31T10:50:57-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=221
permalink: /blog/2006/03/tonic-a-restful-web-application-framework/
categories:
  - Software Development
tags:
  - Rails
  - Software
  - Web Applications
---
[Tonic](http://tonic.sourceforge.net/)<footnote>Via [Sam  
Buchanan](http://afongen.com/blog/)</footnote> is a very promising RESTful web application framework for PHP. If you are doing web development in PHP I definitely suggest you take a closer look.

I am always interested in how RESTful behaviors are implemented, regardless of the language and environment used so I went and had a little peek at the docs. I noticed a couple of details that I think are interesting and might be relevant to the [RESTful](http://cfis.savagexi.com/articles/2006/03/26/rest-controller-for-rails) [Rails](http://microformats.org/discuss/mail/microformats-rest/2006-March/000158.html) [work](http://rubyforge.org/projects/restful-rails/) going on now.

### Controller&#8211;Resource Type Relationship {#controllerresource_type_relationship}

Tonic seems to define a controller class (though they call them something different) per resource type. This is different than any of the rails approaches I have seen, which combine the code to support multiple resource types into a single controller class. It is not clear to me, from the documentation I have read so far anyway, exactly how collection resources with addressable members are handled. For example, if `http://store.example/products/` is a collection of all known products and `http://store.example/products/magik-eight-ball` is a particular product how does Tonic handle route those requests to the appropriate resource controllers?

### Resource Modifier Resources {#resource_modifier_resources}

Tonic uses the concept of editor and delete resources to manage the modification and deletion of resources. This is quite similar to [Charlie&#8217;s approach](http://cfis.savagexi.com/articles/2006/03/26/rest-controller-for-rails). This is at least one significant difference. It appears&#8211;but I am not sure about this so correct me if I am wrong&#8211;that you post to the editor to update the resource. From an REST standpoint that is fairly unclean but it would make handling bad form data issues Charlie talked about a lot easier. And if you also supported PUT on the target resource you app would still be RESTful.
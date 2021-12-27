---
id: 356
title: Announcing Resourceful
date: 2008-06-30T11:48:18-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=356
permalink: /blog/2008/06/announcing-resourceful/
categories:
  - Software Development
tags:
  - http
  - Resourceful
  - REST
  - Ruby
---
[Resourceful](http://resourceful.rubyforge.org/) has its [initial (0.2) release](http://github.com/paul/resourceful/commits/rel_0.2) [today](http://www.theamazingrando.com/blog/index.php/2008/06/30/announcing-resourceful/).

Resourceful is a sophisticated HTTP client library for Ruby. It will (when it is complete, at least) provide an simple API for fully utilizing the amazing goodness that is HTTP.

It is already tasty, though. The 0.2 release provides

  * fully compliant HTTP caching
  * a framework for implementing cache managers (memory based cache manager provided)
  * fully compliant transparent redirection handling (with hooks for overriding the default behavior)
  * plugable HTTP authentication handling (Basic provided)

### Introduction {#introduction}

The API is strongly influenced by our successful experiences with REST. Each URI is represented by a `Resource` object. The `Resource` objects act as a proxy for the conceptual resource. `Resources` expose the basic set of HTTP verbs: get, put, post, delete. For example to get a representation of some resource you do this

    require &#39;resourceful&#39;
    http = Resourceful::HttpAccessor.new
    resp = http.resource(&#39;http://rubyforge.org&#39;).get
    puts resp.body

If you want to post a form you do this

    require &#39;resourceful&#39;
    http = Resourceful::HttpAccessor.new
    resp = http.resource(&#39;http://rubyforge.org&#39;).post("name=Peter&hobbies=programming,diy", :content_type => "application/x-www-form-urlencoded")
    puts resp.code

All non-2xx responses are either handled transparently (e.g. by following redirects) or the method will raise a `UnsuccessfulHttpRequestError`.

### Conclusion {#conclusion}

If you need a decent HTTP library in Ruby come on over and check it out. If you see something you want, or want fixed, feel free to branch the [git repo](http://github.com/paul/resourceful/tree/master) and do it. [Paul](http://www.theamazingrando.com/blog/) and I would love some more contributors and will welcome you with open arms.
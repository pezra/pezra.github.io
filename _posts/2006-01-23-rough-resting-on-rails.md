---
id: 194
title: Rough RESTing on Rails
date: 2006-01-23T01:39:16-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=194
permalink: /blog/2006/01/rough-resting-on-rails/
categories:
  - Software Development
tags:
  - Rails
  - Software
  - Web Applications
---
When I started using Ruby on Rails professionally a few months ago I hoped that Rails was first of a new breed of web application frameworks. After working with it for several months I have reached conclusion that it is not the paradigm shifting framework I had hoped for. Rather, it is just an incremental, though significant, improvement to the existing paradigm of web application frameworks. It is significantly<footnote>I think the [5-10 time productivity improvements people claim](http://www.onlamp.com/pub/a/onlamp/2005/01/20/rails.html) are probably not far off.</footnote> less effort to implement a web application in Rails than in any other framework in which I have worked but it is not _better_ than those other frameworks, just easier.

I am an unabashed [REST](http://en.wikipedia.org/wiki/REST)afarian. I believe that the RESTful nature of the web is what has made it successful. I think that every entity (read: resource) of any consequence in any system should have at least one URI. I believe that RESTful architectures are the best way to produce scalable, flexible and powerful applications. And, sadly, I believe that it is, generally, inappropriate to build RESTful applications in Rails.

### The Problem

Rails is fundamentally a procedural framework. You can see the in the default form of the URI it uses, for example, <samp>http://example.com/product/show/1</samp>, in the names of the methods that get called on the controllers, for example, show, create, destroy, etc, and even what those methods are called, actions. The web is fundamentally not a procedural environment, it is a RESTful environment. However, much like how you can write procedural code in an object oriented environment you can also write procedural code for a RESTful environment, but by doing so you miss many of the advantages of the RESTful environment, which is quite unfortunate.

I am _not_ disappointed by the fact that Rails is, by default, procedural. In fact, for any web application framework to succeed, at this point in time, it must support a procedural approach to web applications. This is because, basically, every mainstream web application framework in existence is procedural in nature. That fact combined with the fact that people will not, usually, adopt a new technology unless the new technology allows them to use their existing idioms more effectively<footnote>I picked this meme up from someone&#8217;s blog but I cannot remember whose. If you recognize it please let me know where I saw it so I and link to it.</footnote> means the any web application framework must support tunneling RPC over the web. Rather, the disappointing thing is that I now believe it is impractical to make Rails behave in a RESTful manner, in the general case<footnote>Though, you can reasonably create RESTful interfaces for small parts of an applications.</footnote>.

Initially I thought that [RestController](http://microformats.org/discuss/mail/microformats-rest/2005-November/000042.html)  
might be a solution. RestController provides a mechanism for easily defining the actions of a controller in terms of the HTTP method used in the request. For example, you might have the following code using the RestController<footnote>All error handling has been left out for clarity.</footnote>

<pre class='code'>class BooksController &lt; RestController&lt;footnote>In reality you would want to  modify ApplicationController inherit from RestController, rather than ActionController::Base, and to inherit from ApplicationController so that ApplicationHelper would be included.&lt;/footnote>
  ...
  verbs_for :show do
    def get
      @book = Book.find(params['id'])
    end
    def put
      @book = Book.find(params['id'])
      @book.update_attributes(params)
    end
  end
  ...
end
</pre>

This works reasonably well, as far as it goes. Unfortunately, merely respecting the action specified at the protocol level is not sufficient for RESTful behavior. Further, the astute reader will have noticed that the abstraction is starting to leak a little already. If you want to create or update a Book entity you need call the show action. Hardly what you would call obvious.

You can hide this inconsistency from the client with a clever set of routes but you are forced to deal with it inside the application code, and that clever set of routes will cost you. You end up needing a lot of routes and most of them have to be hand constructed. Routes in Rails are pretty easy to create but each custom route increases the cognitive load when trying to understanding the entire system. Worse yet, routes are physically separated from controllers (a sub-optimal design decision in my book) so it can often be non-obvious how a particular controller is called from the outside world. Even given a clever set of routes the URI production mechanisms in Rails force you to acknowledge that the system is procedural by requiring a controller and action be specified<footnote>If you do not specify them it assumes the current controller and/or action.</footnote> every time a link is produced.

The problem is an absolutely fundamental one. Namely, Rails equates each URI with as single action and in a RESTful architecture a URI equates to a resource with, up to, four actions. While you can write code in Rails that will do the correct thing the names will never be quite right and you end up maintaining a lot of custom, and non-obvious, code to protect the illusion that your frameworks abstractions match those fundamental to the Web.

### A RESTful Future?

There is a lot to like about Rails. It would be nice if we could retrofit RESTfulness onto Rails, but I cannot see an approach that I think would work. However, I am not a Rails expert so I am going to sketch out what I think a RESTful Rails would look like and hope that someone smarter than I will see a way where I do not.

The following code is approximately what I think a really RESTful controller should look like

<pre class='code'>class BooksController &lt; RestController
  uri_base "/books/"

  resource "." do
    get "application/atom+xml" do
      # return XML representation of collection containing all known books.
    end

    get "text/html" do
      # return HTML representation of collection containing all known books.
    end

    post do
      # create new book and add it to the collection
    end
  end

  resource "./:id", :requirements => {:id => /^[[:digit:]]+$/}  do
    get  do
      # yield html representation of a individual book
    end
    
    put do
      # update and individual book resource
    end
  end

  resource "./:id/editor", :requirements => {:id => /^[[:digit:]]+$/}  do
    get do
      # yield a representation of the book editor (a page with a 
      #   form that puts to "./:id" on submit)
    end   
  end

  resource "./creator" do
    get do
      # yield a representation of the book creator (a page with a 
      #   form that posts to "." on submit)
    end
  end
end

</pre>

In some was this looks a bit like what the RestController provides but it is vitally different is several ways. First, resources are specified by explicit URI patterns. URIs are a fundamental part of the web and are beautiful and elegant in their own right. There is no reason to hide them. There are some practical concerns with explicit URI patterns but this example avoids those by having all the URI patterns be relative to a base URI pattern specified in the controller. If the controller is moved to another URI it need only be tweaked in one place. Second, action implementation is allowed to vary on URI, HTTP method and requested content type. Varying on content type is basically required for any non-trivial REST application so it should be baked into the framework from the beginning.

Having actions specified by URI patterns means that we would need new URI production mechanisms in Rails. These new URI production mechanisms would take the target controller because, in this world view, URIs are usually relative to a controller. The should also, optionally, take a MIME type.

<pre class='code'>&lt;%= link_to "I Know A Rhino",  :controller => 'books', :resource => './:id', :id => a_good_childs_book -%&gt;
</pre>

In principle the above is doable, I think. For example, a RestfulController could write new routes based on the resources calls. The code would probably be a bit hairy but the interface could be made pretty clean. And URI production functions would be cake by comparison to the non-RESTful ones. The problems are mostly practical. For example, controllers are currently loaded lazily. They are not loaded until the first time a route to that controller is exercised. This means that you cannot have the controllers add the appropriate routes for the resources they declare. It might be possible to change Rails to load the controllers eagerly but that raises the question of if two controllers URI spaces overlap who should take precedence. I think you would just do last loaded wins but that means that you would need a way to explicitly tweak the load order. And how do generated routes relate to explicitly routes, precedence-wise. I haven&#8217;t even really thought about how backwards compatibility with RPC style controllers would be maintained, and that would be absolutely required, or how template lookup would work.

### Our Last Best Hope?

I fear that we RESTafarians might have to embrace (and extend) Rails, even though it is not the optimal platform for a RESTful web application framework. The reason is that Rails is _really good_ at, human facing, web applications. I doubt that we will get this sort of productivity compatibility in developing RPC style web applications again anytime soon, if ever. That means that it will be hard to convince the world to embrace a truly RESTful web applicationframework because, even it is great, it will provide only marginal improvements for the RPC web application idiom that people know.

Hopefully someone with more skills and free time than I will take up the challenge of making Rails RESTful framework. Until then I suppose I will continue to write ugly, but functional, RPC style web applications. At least with Rails I do not feel like I am wasting most of my time with administrivia. On the other hand, the lack of administrivia leaves me with more time to feel dirty about the interface I am creating.
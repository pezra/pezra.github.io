---
id: 283
title: Hierarchical Resources in Rails
date: 2007-07-16T09:03:39-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/07/hierarchical-resources-in-rails/
permalink: /blog/2007/07/hierarchical-resources-in-rails/
tags:
  - Rails
  - REST
  - Software
  - Web Applications
---
Consider a situation where you have a type of resource which always belongs to a resource of another type. How do you model the URI space using [Rails](http://rubyonrails.org)? For example, say you have an address resource type. An address is always associated with exactly one user, but a user may have several addresses (work, home, etc).

## The simple approach {#the_simple_approach}

The simplest approach from a Rails implementation perspective is to just have a flat URI space. In this scenario the URI for the collection of addresses associated with a user and a particular address would be, respectively:

    http://example.com/addresses?user_id={user_id}
    http://example.com/addresses/{address_id}

From a REST/Web arch standpoint there is absolutely no problem with this URI. It is a bit ugly for the humans around, though. Worse yet, one might reasonably infer from it that `http://example.com/addresses` references the collection of all the addresses known to the system. While that might be nice from an information modeling point of view, in reality that collection is probably going to be too large to return as a single documents. To be fair, it would be perfectly legal to respond to `/addresses` with a 404 or 403, but it would be a bit surprising to get that result if you were exploring the system.

## The fully hierarchically approach {#the_fully_hierarchically_approach}

Edge Rails contains some improvements to the resource oriented route generators. One of the changes adds support for sub-resources. Sub-resources are support via the `:has_many` and `:has_one` options to `ActiveController::Routing::Map#resources`. These options produce fully hierarchically URIs for the resources. For example

    http://example.com/users/{user_id}/addresses
    http://example.com/users/{user_id}/addresses/{address_id}

The first URI references the collection of all the addresses for the specified user. The second URI references a particular address that belongs to the specified user. These URIs are very pretty, but they add some complexity to the controllers that fulfill them.

The additionally complexity stems from the fact that `address_id` is unique among all addresses of (in most cases it would be an automatically generated surrogate key). This leads to the potential for the `address_id` to be valid but that the address it identifies to not belong to the user identified by `user_id`. In such cases the most responsible thing to do is to return a 404, but doing so takes a couple of extra lines in each of the actions that deal with individual addresses.

## The semi-hierarchical approach {#the_semihierarchical_approach}

After trying both of the previous approaches and finding them not entirely satisfactory. I have started using a hybrid approach. The collection resources are defined below the resources to which they belong but the collection member resources are referenced without an intermediate. For example

    http://example.com/user/{user_id}/addresses
    http://example.com/addresses/{address_id}

This has the advantage of producing fairly attractive URIs across the board. It also provides an obvious location to add a collection resource containing all the child resources if you have a need for looking at all of them with out the parent resource being involved. And it does not require any extraneous code in the controllers to deal will the possibly of the specified parent and child resources being unrelated.

On the downside, it does requires some changes to the routing system to make defining such routes simple and maintainable. Also, it might be a bit surprising if you are exploring the system. For example, if you request `http://example.com/addresses/` you will get a 404, which is probably not what you would expect.

Even with the disadvantages mentioned above I am quite pleased with how the URIs and controllers turn using this technique. If you are looking for a way to deal with hierarchical resources you should give it a try.
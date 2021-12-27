---
id: 925
title: Embedding
date: 2012-12-05T05:00:30-07:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=925
permalink: /blog/2012/12/embedding/
categories:
  - Software Development
tags:
  - api design
  - REST
---
Designing the messages (or representations, i&#8217;ll use the terms interchangeably) is the most important part of API design. If you get the messages right everything else will flow naturally from them. Of course, there are trade offs that must be made when designing messages. One of those trade offs is how much data to put in each message. If they are too small clients must make too many calls to be performant. If they are too big generating, transferring and parsing the messages will be excessively slow.

Any entity worth discussing should have a URI of it&#8217;s very own. That is, it should be a resource. This means that we often (read: almost always) end up with a lot of resources that don&#8217;t really have much data directly. The usual pattern is that they have a small number of properties and then link to a bunch of other resources. For example consider an invoice: a few properties like purchase date, etc and then links to the customer, billing address, shipping address, and line items. The line items would, in turn, link to a product.

We often bulk up the representations of these lightweight resources by directly embedding representations of the other resources to which they link. This tends to reduce the number of requests needed because those embedded representations don&#8217;t need to be requested explicitly. This approach has substantial downsides, at least if implemented naively. Consider the following representation of an invoice with embedded representations.

<pre><code class="brush: javascript"> 
{"purchase_date" : "2012-10-29T4:00Z",
 "customer"      :     
   {"uri" : "http://example.com/custs/42",
    "name": "Peter Williams",
    //...
   },
 "billing_address" :     
   {"uri"   : "http://example.com/addrs/24",
    "line1" : "123 Main St",
    //...
   },
 // etc, etc
 "line_items" :
   [{"uri"     : "http://example.com/li/84",
     "quantity": 3,
     "product" :         
       {"uri" : "...",
        "name": "Blue widget",
        "desc": "..."
       },
    },
    // other line items here
   ] 
} 
</code></pre>

This approach is very appealing. All the data needed to display or operate on a invoice is right there at our fingertips which nicely manages the number of requests that need to be made. The data is also arranged in a logical way that makes sense to our human brains.

For all of its upsides, the downsides to this approach are substantial. The biggest issue, to my mind, is that it limits our ability to evolve this message over time. By directly embedding the line item and product data, for example, we are signalling that they are fundamentally part of this representation. Clients will implement code assuming those embedded resources are always there. That means we can never remove them without breaking clients.

There are many reasons we might want to remove those embedded representations. We might start seeing invoices with a lot of line items thereby resulting in excessively large messages. We might add a lot of properties to products and make the messages too large that way. We might move products to a different database and find that looking the all up takes too long. These are just a few of the innumerable reasons that we might want change our minds about embedding.

### How small is too small?

Given that removing a property from a representation is a breaking change are there ways to design representations that reduce the possibility that we will need to remove properties in the future? The only real way is to make representations as small as possible. We will never need to remove a property that was never added in the first place. We already discussed how messages that are too small can result in excessive numbers of requests but is that really true?

Applying the [yagni](http://en.wikipedia.org/wiki/You_ain't_gonna_need_it) principle is in order when thinking about embedding. Embedding is easy to do and very super extremely hard to undo. It should be avoided until it is absolutely necessary. We will know it is absolutely necessary when, and only when, we have empirical evidence showing that now is the time. This will happen quite rarely in practice. Even when we have empirical evidence that our request volume is too high, solutions other than embedding are usually a better choice. Caching, in particular, can ameliorate most of the load problems we are likely to encounter. The fastest way to get a representation is not to embed it into another message that is passed over the wire but to fetch it out of a local cache and avoid the network altogether.

**Embedding one representation inside another is an optimization. Be sure it is not [premature](http://c2.com/cgi/wiki?PrematureOptimization) before proceeding.**

### sometimes &#8211; not often, but sometimes &#8211; i like the idea of embedding

Annoyingly, sometimes optimizations really are required. In those situations where we have clear empirical evidence that the current approach produces too many requests, we have already implemented caching and we cannot think of another way to solve the problem embedding can be useful. Even in these situations embed should not done hierarchically as in the example above. Rather we should sequester the embedded representations off to the side so that it is clear to clients that they are an optimization. If we can signal that clients should not assume they well always be embedded all the better.

The following is an example of how this might be accomplished using our previous example.

<pre><code class="brush: javascript"> 
{"purchase_date"       : "2012-10-29T4:00Z",
 "customer_uri"        : "http://example.com/custs/42",
 "billing_address_uri" : "http://example.com/addrs/24",
 "shipping_address_uri": "http://example.com/addrs/24",
 "line_item_uris"      :
   ["http://example.com/li/84",
    "http://example.com/li/85"],
 "embedded":
   [{"uri" : "http://example.com/custs/42",
     "name": "Peter Williams",
     //...
    },
    {"uri"   : "http://example.com/addrs/24",
     "line1" : "123 Main St",
     //...
    },
    {"uri"     : "http://example.com/li/84",
     "quantity": 3,
     "product_uri" : "http://example.com/prods/12"
    },
    {"uri" : "http://example.com/prods/12",
     "name": "Blue widget",
     "desc": "..."
    },
    // and so on and so forth
   ] 
} 
</code></pre>

The `_uri` and `_uris` properties are links. A client looks for the relationship it needs and then first looks for a representation in the embedded section with the required `uri`. If it finds one then a network communication has been avoided, if not it can make a request to get the needed data. This approach clearly identifies representations that are embedded as an optimization and makes it easy for clients to avoid relying on that optimization to behave correctly.

This flat embedding is the approach taken by both [HAL](http://tools.ietf.org/html/draft-kelly-json-hal) and [Collection+JSON](http://amundsen.com/media-types/collection/) (albeit with some slightly different nuances). I suspect that the developers of both of those formats have experienced first hand the pains of having representations getting too big but not being able to easily reduce their size without breaking clients. If one of those formats work you use them, they have already solved a lot of these problems.

### Other considerations

Avoiding hierarchical embedding also makes documenting your representations easier. With the sidecar style you can keep each representation to a bare mimimum size and only have to document one &#8220;profile&#8221; of representation for each flavor of resource you have. With this approach there is no difference between the representation of a customer when it is embedded vs when it is the root representation.
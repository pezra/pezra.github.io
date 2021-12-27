---
id: 422
title: What are links
date: 2010-01-06T23:54:10-07:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=422
permalink: /blog/2010/01/what-are-links/
openid_comments:
  - 'a:0:{}'
categories:
  - Software Development
tags:
  - hypertext
  - REST
---
When designing hypertext formats is it better to provide links for every available action or to provided links to related resources and let the client use the protocol interface to achieve particular actions on those related resources?

I have leaned in both directions at various times. I have never fully convinced myself either.

To make the issues a bit clearer let me use and example lifted from the [article that got me thinking about this most recently](http://www.subbu.org/blog/2010/01/hypertext-is-the-transaction-engine).<sup id='fnref:1'><a href='#fn:1' rel='footnote'>1</a></sup>

    <cart>
      <!-- some stuff here -->
      <link rel="http://ex.org/rel/abort" 
            href="http://ex.org/cart/cancel;token=987654321"/>
      <link rel="http://ex.org/rel/add-more" 
            href="http://ex.org/cart/add;token=987654321"/>
      <link rel="http://ex.org/rel/buy" 
            href="http://ex.org/cart/buy;token=987654321"/>
    </cart>

I place this example in the &#8220;links for every action&#8221; camp. Each of the links in the example describes exactly one action.

An alternate approach might look something like this.

    <cart>
      <!-- some stuff here -->
      <link rel="http://ex.org/rel/line-items" 
            href="http://ex.org/cart/line-items;token=987654321"/>
      <link rel="http://ex.org/rel/new-order" 
            href="http://ex.org/orders?cart=987654321"/>
    </cart>

From a client perceptive these are a bit different.

Abandoning cart
:   A client that wants to abandon a cart in the first example would make a DELETE or POST &#8211; it&#8217;s a bit hard to tell which from the example &#8211; request to the href of the `http://ex.org/rel/abort` link. In the second example a similar client would just DELETE the cart resource.

Adding item
:   When adding an item in the first example the client would post a www-form-urlencoded document containing the URI of the item to add and the quantity to the href of the `http://ex.org/rel/add-more` link. In the second example, the same document gets posted to href of the `http://ex.org/rel/line-items` link.

Placing order
:   In the first example the client would make a POST request to the href of the `http://ex.org/rel/buy` link. In the second example the client would POST a www-form-urlencoded document containing the cart URI and some payment information to the href of the `http://ex.org/rel/order` link.

### Differences {#differences}

Obviously the to approaches result in quite similar markup. The same behavior is encoded in both. In the first example the links are action oriented. All actions that can be taken on an item are explicitly stated using a link. In the second approach the links are data oriented rather than action oriented. Rather than having separate links to retrieve the current line items and to add a new line item the `http://ex.org/rel/items` link provide both actions using the GET and POST HTTP methods respectively.

The first approach it better at expressing what actions are allowable at any given point in time. For example, once the purchase process has been initiated it does not make sense to abort a cart. So if you GET a cart after POSTing to the `http://ex.org/rel/buy` link the representation would not have the `http://ex.org/rel/abort` link.

The second is more concise because it, at least potentially, provides access to more than one action per link based on the standard HTTP methods. You don&#8217;t need to provide a separate abort link because DELETEing the cart is sufficient. You don&#8217;t need to provide separate get line items and add line item links because a single link that can handle GET and POST requests will work.

The first approach is a bit more flexible with regard to implementation details. If you need for some reason to have different URIs for the retrieve line item request than the add line item request you could easily achieve it. The second example makes that impossible.

### Conclusion {#conclusion}

I am still not entirely convinced but i am leaning toward the more flexible, verbose and explicit approach of a link for every actions.<sup id='fnref:2'><a href='#fn:2' rel='footnote'>2</a></sup> Having links represent actions rather than resources feels a bit odd, but i think it provides more of the benefits we hope to get from a RESTful architecture.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='fn:1'>
      <p>
        I am <a href='http://barelyenough.org/blog/2010/01/unobtrusive-link-info/'>still not a fan of the <code>link</code> element</a>. This example is a good one in every other regard.
      </p>
      
      <p>
        <a href='#fnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='fn:2'>
          <p>
            That counts as at least the third vacillation i have had on this topic. I was leaning the other direction before writing this.
          </p>
          
          <p>
            <a href='#fnref:2' rev='footnote'>&#8617;</a></li> </ol> </div>
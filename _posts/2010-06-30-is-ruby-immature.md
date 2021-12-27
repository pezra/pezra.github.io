---
id: 458
title: Is ruby immature?
date: 2010-06-30T23:17:40-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/blog/2010/06/is-ruby-immature/
permalink: /blog/2010/06/is-ruby-immature/
categories:
  - Software Development
tags:
  - Rails
  - Ruby
---
A friend of mine recently [described why he feels ruby is immature](http://www.evilsoft.org/2010/06/30/why-i-view-ruby-as-immature). I, of course, disagree with him. There is much in ruby that could be improved, but the issues he raised are a) intentional design choices or b) weaknesses in specific applications built in ruby. Neither of those scenarios can be fairly described as immaturity in the language, or the community using the language.

## `Set` {#id1}

Mr. Jones&#8217; main example is one regarding the [`Set`](http://ruby-doc.org/core/classes/Set.html) class in ruby. In practice `Set` is a rarely used class in ruby. I suspect it exists primarily for historical and completeness reasons. It is rather rare to see idiomatic ruby that utilizes `Set`.<sup id='fnref:1'><a href='#fn:1' rel='footnote'>1</a></sup>

This is possible because `Array` provides a rather complete implementation of basic set operations. Rubyist are very accustom to using arrays. So is more common to just use the set operator on arrays rather than converting an array into a sets.

The set operations on `Array` do not have the same performance characteristics mr. Jones found with `Set`. For example,

    $ time ruby -rpp -e &#39;pp (1..10_000_000).to_a & (1..10).to_a&#39;
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    
    real	0m10.152s
    user	0m6.592s
    sys	0m3.515s
    
    $ time ruby -rpp -e &#39;pp (1..10).to_a & (1..10_000_000).to_a&#39;
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    
    real	0m12.410s
    user	0m8.397s
    sys	0m3.860s

Order still matters, but very much less. (That is on 1.8.6, the only version i have handy at the moment. I am sure that 1.9, or even 1.8.7, would be quite a bit faster.)

Libraries that are low traffic areas don&#8217;t get the effort that high use libraries do in any language. Even though `Set` is part of the standard library, it is definitely counts as a low traffic area. Hence, it has never been optimized for large numbers of items. This is appropriate because as we [learned from Ron Pike](http://www.faqs.org/docs/artu/ch01s06.html) &#8220;n is usually small&#8221;. The benefits of handling large sets performantly is not worth the addition complexity for a low traffic library.

## `nil` {#id2}

In his other example mr. Jones implies that the fact that `nil` is a real object is disadvantageous. On this count he is simply incorrect. Having `nil` be an object allows significant reductions in the number of special cases that must exist. This reduction in special cases often results in less code, but is always results in less cognitive load.

Consider the [`#try`](http://api.rubyonrails.org/classes/Object.html#M000027) in ruby. While not my favorite implementation of this concept, it is still a powerful idiom for removing clutter from the code.

`#try` executes the specified method on the receive, unless the receiver is `nil`. When the receive is `nil` it does nothing. This allows code to use a best effort approach to performing non-critical operations. For example<sup id='fnref:2'><a href='#fn:2' rel='footnote'>2</a></sup>,

    def remove_email(email)                                                                                         
      emails.find_by_email(email).try(:destroy)                                                                     
    end  

This is implemented as follows:

    module Kernel
      def try(method, *args, &block)
        send(method, *args, &block)
      end
    end
    
    class NilClass
      def try(*args)
        # do nothing
      end
    end

You could implement something like `#try` in a system that has non-object &#8220;no value&#8221; mechanism. It would be less elegant and less clear, though. (It would probably be less performant too because method calls tend to be optimized rather aggressively.) Have `nil` be an object like everything else is one less the primitive concept that the code and the programmer must keep in mind.

Mr. Jones does bring up the issue of `nil.id` returning 4 and that value being used as a foreign key in the database. This is not a problem i see very often, but i can happen.

This is definitely not a problem with ruby. Rather results from an unfortunate choice of naming convention in rails. Rails uses `id` as the name of the primary key column for database tables. This results in an `#id` method being created, which overrides the `#id` provided by ruby itself for all objects. If rails had chosen to call the primary key column something that did not conflict with an existing ruby core method &#8211; say `pk` &#8211; we would not be having this discussion.

## In general {#in_general}

Mr. Jones asserts that &#8220;ruby is rife with happy path coding&#8221;. I disagree with his characterization. The ruby community has a strong bias towards producing working, if incomplete code, and iterating on that code to improve it. This &#8220;simplest thing that could work&#8221; approach does result in the occasional misstep and suboptimal implementations. In return you get to use a lot of new stuff more quickly and when there are problems they are easier to fix because the code is simpler.

The ruby community has strongly embraced the small pieces, loosely joined approach. This is only accelerating the innovation in ruby. Gems have lowered the fiction of distributing and installing components to previously unimaginable levels. This has allowed many libraries that would have been to small to be worth releasing in the past to come into existence.

[Rack](http://rack.rubyforge.org/), with it&#8217;s middleware concept, is an example of the ruby community taking much of the Unix philosophy and turning it to 11. While rails has much historic baggage, even it is moving to a much more modular architecture with the up coming 3.0 release.

Following these principles does result in some rough edges occasionally, but the benefits are worth the trade. The 80% solution is how [Unix succeed](http://naggum.no/worse-is-better.html). An 80% solution today is better than a 100% solution 3 months from now. (As long as you can improve it when needed.) We always have releases to get to, after all.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='fn:1'>
      <p>
        I, on the other hand, do use set rather more than the average rubyist. <code>Set</code> is a rather performant way producing collections without duplicate entries.
      </p>
      
      <p>
        <a href='#fnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='fn:2'>
          <p>
            Shamelessly copied from <a href='http://ozmm.org/posts/try.html'>Chris Wanstrath</a>.
          </p>
          
          <p>
            <a href='#fnref:2' rev='footnote'>&#8617;</a></li> </ol> </div>
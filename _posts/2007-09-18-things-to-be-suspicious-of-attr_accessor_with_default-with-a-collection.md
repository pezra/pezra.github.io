---
id: 296
title: 'Things to be Suspicious Of &#8212; attr_accessor_with_default with a collection'
date: 2007-09-18T09:01:41-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/09/things-to-be-suspicious-of-attr_accessor_with_default-with-a-collection/
permalink: /blog/2007/09/things-to-be-suspicious-of-attr_accessor_with_default-with-a-collection/
categories:
  - Software Development
tags:
  - Rails
---
My team ran into this problem yesterday where the a particular, very important, request was failing in one of our Rails apps. The failure did did not make much sense and even more confusingly, the same code worked perfectly in the console. As part of debugging the problem we restarted the mongrel cluster, and suddenly everything worked again.

I _hate_ it when the symptoms go away before you have a chance to diagnose the root cause of a problem. It still out there waiting to bite you again, and you have no idea what actually causes the problem. Well after quite a while looking at the code I noticed a bit of code similar to this.

    class Widget < ActiveRecord::Base
      attr_accessor_with_default :merge_queue, []
    
      def merge(thing)
        merge_queue << thing
      end 
    
      def do_pending_merges
        # shift each thing off merge_queue and merge it into self 
      end 
    end

Looks innocent enough. However the `attr_accessor_with_default` is the problem. You can normally think of `attr_accessor_with_default` as short hand for something like

    def merge_queue
      []
    end

However this is not strictly speaking true. As written above the default value for all instances of `Widget#merge_queue` are the exact same Array object. So rather `merge_queue` behaving as a private instance variable it acts like more like a shared class variable. This means that anytime you add something to `merge_queue` one instance of a Widget you are adding it to the default value of `merge_queue` for all current and future instances of Widget.

This turned out to be our problem. Restarting the mongrel cluster made the symptoms go away because `merge_queue`&#8217;s default value was no longer erroneously pre-populated, and the code worked in the console for the same reason. We had never noticed the issue because when `#do_pending_merges` worked correctly it emptied `merge_queue` as it went. However, when more than one merges happened simultaneously or, as in our case, the merging failed, the shared `merge_queue` default value contained some erroneous items.

This attribute accessor like should have been written like

    attr_accessor_with_default(:merge_queue){[]}

In this form, the block is evaluated each time a default value is needed meaning that each instance of Widget would have gotten its very own brand new empty array.

So the moral is, be very, very suspicious if see an `attr_accessor_with_default` with a default value that is a collection. It is possible that it may be correct, but it is not very likely. More likely is that the original author did not realize that the exact same instance would be used as the default value each time the attribute accessors were called.
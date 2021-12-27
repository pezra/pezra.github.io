---
id: 126
title: 'Why Java is Not My Favorite Language â€” Reason #16.'
date: 2005-06-15T17:25:16-06:00
author: Peter Williams
layout: post
guid: 'http://pezra.barelyenough.org/blog/2005/06/why-java-is-not-my-favorite-language-%e2%80%94-reason-16/'
permalink: '/blog/2005/06/why-java-is-not-my-favorite-language-%e2%80%94-reason-16/'
categories:
  - Software Development
tags:
  - Software
---
`UnsupportedOperationException`. Why, in the name of all that is good and right, would you have such a dumb exception? Either the operation is important or it is not. If it is not _don&#8217;t clutter the interface with it_. If it is it ought to actually be implemented.

I have known about `UnsupportedOperationException` for a while now but I had not run into a method that actually threw it until yesterday. That method was `ArrayList.remove()`. I can just hear the lame argument for having this method throw `UnsupportedOperationException` now, &#8220;Removing an item from the underlying array would inefficient and/or hard to write.&#8221; If you feel any of that matters fine, but don&#8217;t lie to me and tell you have a bunch of operations, by claiming you implement the `List` interface, and then blow up in my face when I try to use one of those operation.

If `remove()` is really not a fundamental operation for lists (which I would debate, but whatever) it should _not_ be part of `List`. There should be a `RemoveList`, or some such, interface which implements `List` and adds `remove()`. Interfaces are free, you can have as many of them as you need.

If you do not actually implement _all_ methods of an interface you have no right to claim you implement the interface, regardless of what the javadoc comments say.

<p class='update'>
  {Update: Curt Cox pointed out that I was, in fact, not using the normal ArrayList class. The offending class was actual <code>java.util.Arrays$ArrayList</code> which I can only assume, since there are no Javadocs for it, is an intentionally broken version of ArrayList.}
</p>
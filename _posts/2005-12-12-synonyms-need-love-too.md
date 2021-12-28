---
id: 188
title: Synonyms Need Love Too
date: 2005-12-12T12:29:22-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=188
permalink: /blog/2005/12/synonyms-need-love-too/
categories:
  - Software Development
tags:
  - Software
---
In the [humane vs inhumane&#8230; err, I mean &#8220;minimal&#8221;&#8230; interface debates](http://martinfowler.com/bliki/HumaneInterface.html)<footnote>Joey DeVilla has done a pretty nice [summary](http://farm.tucows.com/blog/_archives/2005/12/9/1443435.html) of the discussion.</footnote> synonyms don&#8217;t seem to get much respect. You would expect this from the minimalist crowd but many otherwise humane people seem to have little regard for method aliases. Here are a couple of examples.

[Charles Miller](http://fishbowl.pastiche.org/2005/12/09/humane_interfaces)

> Having two otherwise equivalent ways to perform the same operation is bad user-interface design, and it's bad library interface design, because the existence of the synonyms actually adds to your cognitive load by making you choose between them.

[Joey DeVilla](http://farm.tucows.com/blog/_archives/2005/12/9/1443435.html)

> I do have one complaint about the Ruby approach &#8212; method aliases. While it&#8217;s convenient for luring in programmers from other languages, I think it actually detracts from code readability to have two method names mean the same thing.

I think that synonyms are a valuable tool. Even if Charles Miller is correct that having multiple names for a method &#8220;adds to your cognitive load by making you choose between them&#8221; I still think that method aliases are great.

Any given piece of code is likely to be read a lot more than it will be written so it is far more important to optimize the reading of the code than the writing of the code. The fact that it takes the original author an extra tenth of a second to decide between `length` and `size` does not really concern me because every time I read the code and see that they choose `size` it indicates that the Array I am looking at is being treated more like a set or bag than an ordered list. Expressing the intent of the code effectively is worth the slight additional cognitive load for the original author.

I would argue that synonyms reduce the overall complexity. From an interface consumers point of view an alias is just another behavior of the interface, the fact that it is exactly the same as some other method is an implementation detail that should be of little or no interest. From the interface producers point of view they are an efficient and low cost way to provide a nuanced variety of similar behaviors. Humans are well suited to handling multiple words with similar meanings. No one argues that we should rid english of it&#8217;s synonyms, because while synonyms increase the number of words one must know they also increase the expressiveness of the language

If you think of method aliasing as an implementation detail to make the implementation of certain methods less costly (and that is exactly how you should think about it), the argument against method aliases sounds pretty much like the argument for minimal interfaces. That the clients don&#8217;t really need those extra methods because they can affect the same outcome using a more limited set of behaviors. While this is true it misses the point. Programming is, first and foremost, about is about expressing the intent of a program to other humans, not performing a mathematical reduction. Humane interfaces allow interface consumers to be written in a more expressive and intentional way.

<p class='update'>
  {Update: Fixed main link to point to Martin Fowler&#8217;s bliki page like I had meant to do the first time.}
</p>

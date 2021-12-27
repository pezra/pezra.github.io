---
id: 111
title: Hungarian Good, Exceptions Bad?
date: 2005-05-11T12:51:36-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2005/05/hungarian-good-exceptions-bad/
permalink: /blog/2005/05/hungarian-good-exceptions-bad/
categories:
  - Software Development
tags:
  - Design
  - Software
---
I think Joel has got a good idea in [Joel on Software &#8211; Making Wrong Code Look Wrong](http://www.joelonsoftware.com/articles/Wrong.html). Unfortunately, his implementation sucks. 

I see his point about Apps Hungarian, in which you tag names with an indication of how the application uses it, but I think it clutters the code significantly. Fundamentally, Apps Hungarian is a kludge &#8212; and a pretty vile one at that &#8212; to get around the fact that common languages lack the really important feature of type aliasing. &#8220;rwMax&#8221; is fine but so is &#8220;maxRow&#8221; and I find the latter a bit easier on the eyes (camel case grips aside). It would be nice is if you could have a type named RowNumber, which was really just another name for an integer, but in this case you probably want to included row or col(umn) in the var name anyway, just to make it obvious. In my experience this is accepted practice, but perhaps not followed as often as it should be. Either you use names that look like &#8220;rwMy&#8221; or &#8220;MyRow&#8221;.

As for his example web app, I have a much better solution than his. Rather than crufting up the names in your code the interfaces you used should be domain specific. The idea of making it obvious when returning text provided by user without HTML encoding it first can easily be achieve by creating an HTML writer class (or module). At it&#8217;s most basic this class would have two methods, `write()`, which HTML encodes the string before writing it, and `writeUnencoded()`, which writes the string with no modifications. With this design using writeUnencoded() with constants or literals is always okay, but using writeUnencoded() with variable is suspicious. So every time you see

<pre class='code'>htmlOut.writeUnencoded("&lt;br /&gt;")</pre>

It is correct.

<pre class='code'>htmlOut.write("&lt;br /&gt;")</pre>

It is probably wrong.

<pre class='code'>htmlOut.write(name)</pre>

It is correct.

<pre class='code'>htmlOut.writeUnencoded(name)</pre>

It is probably very wrong.

This solution is better in several respects: bad code smells bad, it makes doing a dangerous thing more difficult than doing safe things and it is not subject to breakage by newbies that have not learned your naming conventions yet. Using Apps Hungarian gets you bad code smells bad (but only if both the reader and writer are intimately familiar with the prefixes being used), but it does _not_ encourage to you to do the right thing. One of the rules I use is dangerous operations should be indicated as such and the equivalent safe operation should be easier to find and/or use.

As for exceptions, Joel is not completely wrong that throws are a bit like GOTOs. But they are still a better way to program. I think most of this ground has been covered but having to check the result code of every method or function you call is _not_ a good approach to producing code that is understandable, and failing to do that in a non-exception environment is a sure way to end up with unstable code. Fortunately, Joel has already lost this argument because most programmers use exceptions and have found that while they may occasionally cause unexpected behavior they are a big win, with regards to both code understandability and stability.
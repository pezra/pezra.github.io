---
id: 306
title: When To Use Exceptions
date: 2007-11-21T13:52:12-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/11/when-to-use-exceptions/
permalink: /blog/2007/11/when-to-use-exceptions/
categories:
  - Software Development
tags:
  - exceptions
  - Java
  - programming
  - Ruby
  - Software
---
[Marty Alchin recently posted about the &#8220;evils&#8221; of returning `None` (or `nil` or `null` depending on your language of choice)](http://gulopine.gamemusic.org/2007/11/returning-none-is-evil.html). I think he has it basically right. Sure there are situations where returning `nil`<sup id='5e8d841da891b53fffb25133625beda38f47f400fnref:1'><a href='#5e8d841da891b53fffb25133625beda38f47f400fn:1' rel='footnote'>1</a></sup> is appropriate, but they are pretty rare. For example, if a method actually does what the client asked and there is nothing meaningful to return, then by all means return `nil`. If, however, the method was not actually able to do what the client asked, raising an exception is the appropriate thing to do. Unfortunately, I think that most methods that return `nil` in modern libraries and programs actually do so as a way to indicate a failure condition, and that _is_ evil.

[CÃ©dric Beust responded to Mr Alchin](http://beust.com/weblog/archives/000470.html) saying basically, a) &#8220;problems caused by returning `null` are easy to debug&#8221; and b) &#8220;programmers are all knowing, about now and the future, so they can decide when to return `nil` and when to raise an exception.&#8221; (I am, as you might have guessed, taking significant liberties in my para-phrasing. You should go read Mr Beust&#8217;s post if want to know what he actually said.)

## Ease of debugging {#ease_of_debugging}

On the debugging point I would say that Mr Beust is generally correct. It is usually the case that `nil` returns are fairly easy to debug. However, this is not always true. In fact, it is not uncommon, in my experience, to find a `nil` where it is not suppose to be but then to spend a fair bit of time tracking down where that value became `nil`. That time is usually spent walking my way up the call stack trying to find the subtle bug that results in a nil return from a method in only some odd situations.

That sort of debugging is not the end of the world but it is annoying particularly because it is so easily avoidable.

## All knowing programmers {#all_knowing_programmers}

To be fair, Mr Beust did not actually say that he believes that programmers are all knowing. But he did describe a way of using exceptions that would only make sense if programmers were all knowing. From that, one might infer that he does believe that programmers are in fact omniscient. In my experience this misconception is fairly common in the Java community (Actually, this mentality exists to varying degrees in most programming communities) . Java itself includes many decisions that seem only to make sense in the presence of this assumption. But I digress.

The statement to which I am referring is

> Here is a scoop: exception should only be thrown for exceptional situations. Not finding a configuration value is not exceptional. Not finding a word in a document is not exceptional: it can happen, it&#8217;s even expected to happen, and it&#8217;s perfectly okay if it does.

My response to this is: Who exactly are you to decide that not finding a configuration value is a non-exceptional event for my application? Library programmers<sup id='5e8d841da891b53fffb25133625beda38f47f400fnref:2'><a href='#5e8d841da891b53fffb25133625beda38f47f400fn:2' rel='footnote'>2</a></sup> should not be deciding what is and is not an &#8220;exceptional&#8221; event. That is a value judgement they cannot possibly make correctly, unless they understand every way that every program will make use of that library (or class) in perpetuity.

Exceptions are not about &#8220;exceptional situations&#8221;, whatever that means. They are a way for methods tell their caller that it was not able to do what the caller asked it to do. If I say, &#8220;dictionary give me the value associated with a particular key&#8221; and the key does not exist, the appropriate response is a `NoSuchKey` exception. Returning a `nil` in that situation is a lie. A lie with real, and negative, consequences. Consider this, I ask a dictionary for the value associated with a key and it returns `nil`. Does that mean the key does not exist, or that the key does exist and it&#8217;s value is `nil`? Those two are very different but a `nil` returning method conflates them requiring, at the very least, an addition method call figure out which possibility is actually the case.

If having methods actually inform you of their inability to perform the desired action is complicated or hard to understand it is time to upgrade your language or write a better interface. For example, if catching an exception from a dictionary look up complicated for the cases where you just want to use a default value, that exception catching and default value behavior could easily be put in a `get_with_default(key, default_value)` method that either returns the value from the dictionary or the default value. That would certainly be clearer than returning `nil` and having every consumer add an ugly if block after the get. Or you could switch to a language (such as [Ruby](http://ruby-lang.org)) with a compact single line exception handling syntax.

Either way my advice is: Do _not_ use `nil` as a way to indicate that the object was unable to perform the requested operation, that is job of exceptions. If you see a method that returns `nil` demand an [affirmative defense](http://en.wikipedia.org/wiki/Affirmative_defense) for that behavior because it is often incorrect.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='5e8d841da891b53fffb25133625beda38f47f400fn:1'>
      <p>
        <code>nil</code> is the Ruby equivalent of <code>None</code> in Python and <code>null</code> in Java. Since all other programming languages are but pale shadows in comparison to Ruby I shall hence forth be using <code>nil</code> to describe this concept.
      </p>
      
      <p>
        <a href='#5e8d841da891b53fffb25133625beda38f47f400fnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='5e8d841da891b53fffb25133625beda38f47f400fn:2'>
          <p>
            By library programmer I mean someone that is writing code that will be used by someone else at some point in the future. If that does not include you it is because a) you are not a programmer or b) you are writing completely unmaintainable spaghetti code.
          </p>
          
          <p>
            <a href='#5e8d841da891b53fffb25133625beda38f47f400fnref:2' rev='footnote'>&#8617;</a></li> </ol> </div>
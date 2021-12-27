---
id: 235
title: Early impressions of PHP
date: 2006-05-19T15:08:34-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=235
permalink: /blog/2006/05/early-impressions-of-php/
categories:
  - Software Development
tags:
  - Software
  - Web Applications
---
In my new job I am getting a chance to really learn PHP. PHP is the workhorse of the web these days. There are tons of web applications written in PHP, probably more than any other language or framework. This is primarily because it is so productive, as [I found out a while ago](http://pezra.barelyenough.org/blog/2005/07/the-joy-of-programming/). All that being said, I find PHP really weird. I should note that I have only used PHP 4. Because we use PHP 4 in production here, I have not take much time to check out what is different in PHP 5.

The first thing I missed was exceptions. Sure you _can_ write software without exceptions but the result is just so ugly. Often you can barely see what is suppose to be happening because of all the `isError()` checks. Of course, skipping those makes things even worse because you end up with killing the PHP process because calling a undefined method is a fatal error.<sup id='e43edad21a82f5826a46444fd876692a156adb8bfnref:1'><a href='#e43edad21a82f5826a46444fd876692a156adb8bfn:1' rel='footnote'>1</a></sup>

Perhaps the weirdest thing is the way PHP deals with objects, though. I have been working almost exclusively in stronglyagik&#8221;) [OO](http://www.ruby-lang.org/ "Ruby") [languages](http://java.sun.com "Java") for a long time now so I am very used to the parameter passing orthodoxy of &#8220;object reference [by value](http://www.everything2.com/index.pl?node=call%20by%20value)&#8221;.<sup id='e43edad21a82f5826a46444fd876692a156adb8bfnref:2'><a href='#e43edad21a82f5826a46444fd876692a156adb8bfn:2' rel='footnote'>2</a></sup> In most mainstream languages you cannot pass objects directly as parameters at all. Rather, you always pass an object reference.

PHP, on the other hand, not only support passing objects directly but [pass by value](http://www.everything2.com/index.pl?node=call%20by%20value) is the default way to pass an object. Unfortunately, that is almost<sup id='e43edad21a82f5826a46444fd876692a156adb8bfnref:3'><a href='#e43edad21a82f5826a46444fd876692a156adb8bfn:3' rel='footnote'>3</a></sup> never what you want, so object based code ends up with pass by reference indicators sprinkled all over the place.

And then there are the nits&#8230;

I despise explicit statement terminators, they clutter the code without adding any useful information. If you, as a language designer, really feel that you cannot take the time to write a parser smart enough to automatically detect the end of statements at least pick a termination character that is less ugly than the semicolon.

Most of the time I don&#8217;t seem much need in investing effort to reduce &#8220;finger typing&#8221; but the method invocation operator gets typed so much that making is easy to type really _is_ worth the effort. It really should only be one character, and preferably one that does not require the shift key. PHP misses on both counts by making the method invocation operator &#8216;->&#8217;, which is two character one of which requires the shift key.

Still, even with all it&#8217;s issues there is no doubting what you can do with PHP if you are willing to get a little dirty.

<p class='update'>
  {Update: Added note about only know PHP 4.}
</p>

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='e43edad21a82f5826a46444fd876692a156adb8bfn:1'>
      <p>
        Dieing on an undefined method is amazingly inconvenient but probably the only real choice given the lack of exceptions.
      </p>
      
      <p>
        <a href='#e43edad21a82f5826a46444fd876692a156adb8bfnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='e43edad21a82f5826a46444fd876692a156adb8bfn:2'>
          <p>
            Passing the reference by value results in behavior that is quite similar to <a href='http://www.everything2.com/index.pl?node=call%20by%20reference'>pass by reference</a> but there are some subtle differences.
          </p>
          
          <p>
            <a href='#e43edad21a82f5826a46444fd876692a156adb8bfnref:2' rev='footnote'>&#8617;</a></li> 
            
            <li id='e43edad21a82f5826a46444fd876692a156adb8bfn:3'>
              <p>
                I think I am being really generous here. I cannot think of even one situation in my entire career where I thought, &#8220;if only I could pass this object by value this code would be better.&#8221; But I suppose it is possible that pass-by-value might not always be wrong.
              </p>
              
              <p>
                <a href='#e43edad21a82f5826a46444fd876692a156adb8bfnref:3' rev='footnote'>&#8617;</a></li> </ol> </div>
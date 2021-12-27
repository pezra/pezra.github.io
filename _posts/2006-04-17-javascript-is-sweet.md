---
id: 227
title: JavaScript Is Sweet
date: 2006-04-17T21:36:01-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=227
permalink: /blog/2006/04/javascript-is-sweet/
categories:
  - Software Development
tags:
  - JavaScript
  - Software
---
While reading [Oliver Steele&#8217;s article on JavaScript Memoization](http://osteele.com/archives/2006/04/javascript-memoization) this bit jumped out at me.

<blockquote cite='http://osteele.com/archives/2006/04/javascript-memoization'>
  <pre>
<code>
function Angle(radians) {this.setRadians(radians)}
Angle.prototype.setRadians = function(radians) {
  this.radians = radians;
  this.getDegrees.reset();
};
Angle.prototype.getDegrees = function() {
  return this.radians * 180 / Math.PI;
}
memoizeConstantMethod(Angle.prototype, 'getDegrees'); 
</code>
</pre>
</blockquote>

The reason that jumped out is that `getDegrees` is a function that returns a number, but in the above code you see this `this.getDegrees.reset()`. In other languages that would require a `reset` method on number objects, but not in JavaScript. In JavaScript methods are objects and, therefore, can have methods of their own<sup id='54fa3b6e99cb9470f7788b4006d3dc57b5d1e8e9fnref:1'><a href='#54fa3b6e99cb9470f7788b4006d3dc57b5d1e8e9fn:1' rel='footnote'>1</a></sup>. This allows you to get the effect of [high-order messaging](http://nat.truemesh.com/archives/000535.html) without all the fuss.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='54fa3b6e99cb9470f7788b4006d3dc57b5d1e8e9fn:1'>
      <p>
        Is that the exact semantics that allows this functionality in JavaScript?
      </p>
      
      <p>
        <a href='#54fa3b6e99cb9470f7788b4006d3dc57b5d1e8e9fnref:1' rev='footnote'>&#8617;</a></li> </ol> </div>
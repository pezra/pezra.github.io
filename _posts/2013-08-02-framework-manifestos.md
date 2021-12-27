---
id: 967
title: Rails vs Node.js
date: 2013-08-02T07:00:00-06:00
author: Peter Williams
layout: post
guid: https://barelyenough.org/?p=967
permalink: /blog/2013/08/framework-manifestos/
categories:
  - Software Development
tags:
  - express
  - Rails
---
That title got your attention, didn&#8217;t it? While trying to make this decision myself recently I really wished some [agile manifesto](http://agilemanifesto.org/) style value statements existed for these two platforms. Now that I have my first production deploy of a Node.is app I&#8217;m going give it a stab.

##### The [Ruby on Rails](http://rubyonrails.org) community prefers:

  * **Speed of development** over runtime performance
  * **Clarity of intent** over clarity of implementation
  * **Ease of getting started** over ease of personal library choice
  * **Freedom to customize** over ease of debugging

##### The [Express](http://expressjs.com)/[Node.js](http://nodejs.org) community prefers:

  * **Runtime scalability** over speed of development
  * **Less code** over covering every use case
  * **Freedom of library choice** over ease of getting started

I am not suggesting that these communities don&#8217;t care about the things on the right, just that they care more about the things on the left. When faced with a tradeoff between the two values they will most often optimize for the value on the left. Of course not everyone in these communities will agree with these values and this is a great thing. A loyal opposition is invaluable because it keeps community from going off the deep end. It is also possible that i am wrong and there is not even general consensus about some of these. There is a particular risk of that with the Express/Node.js principles as I am quite new to that community.

Some of these values spring, i think, from the respective languages used by the platforms. For example, clarity of intent and speed of development are strong values of the Ruby language and that mentality has made its way into Rails also. On the Javascript side freedom and simple base constructs are strong values of the language. Is it that the language we are writing in influences how we think or that people who prefer certain values choose a language that reflects their values? One supposes that once the linguist settle that whole [linguistic relativity](http://en.m.wikipedia.org/wiki/Linguistic_relativity) thing we might have an answer to this question.

For what it is worth we chose to use Node.js. We had a problem domain almost perfectly suited for Node.js (IO bound and limited business logic on the server side) and we wanted to try out something new. I think the latter argument was actually the more powerful for our team.
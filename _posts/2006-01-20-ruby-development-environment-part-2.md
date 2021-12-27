---
id: 199
title: Ruby Development Environment, Part 2
date: 2006-01-20T16:17:05-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=199
permalink: /blog/2006/01/ruby-development-environment-part-2/
categories:
  - Software Development
tags:
  - Rails
  - Software
---
After finishing my [previous post](http://pezra.barelyenough.org/blog/2006/01/my-ruby-development-environment/) in response to [Stephen O&#8217;Grady&#8217;s ruby tools question](http://www.redmonk.com/sogrady/archives/001217.html) it occurred to me that I might be able to shed some light onto the other options that available. While I use Emacs some of the other developers here do not. I will try to give my impression of the couple of other options we have used.

### [RadRails](http://www.radrails.org/)

We tried radrails for a while. It was buggy, but you expect that from from product with a version number that is less than one. In the end we decided not to use it because it is not done and even if it where it has some serious limitations. The not doneness that cause the most problem was the inability to create a &#8220;server&#8221; (RadRails terminology for shortcut/script that starts the Rails application inside the IDE). RadRails creates one for you when you create the project (if you mutter just the right incantations). But if you need to create a new one you are out of luck. Unfortunately, if you import and existing Rails project (like, say, the one you just checked out of source control) RadRails does not create a &#8220;server&#8221; for you (or at least I did not figure out how to make it do so). This mean that you cannot start a Rails application inside RadRails if the project preexists the RadRails project.

Additionally, I dislike the idea of having an IDE that exclusive to just Rails. It seems far to limiting. We (will) have a non-trivial part of our system which is not a Rails application (it synchronizes data between the legacy system and the Rails application). It would be unfortunate to have two separate, but equal, IDEs for the two parts of this project.

### [Arachno](http://www.ruby-ide.com/ruby/ruby_ide_and_ruby_editor.php)

We have settled on Arachno for our windows based developers who like graphical IDEs. It has less direct support for Ruby on Rails than RadRails, but it seems to actually work quite well. The developers who are using it were, most recently, doing C# in Visual Studio and Arachno is similar enough to that experience that they are quite comfortable in it. (I have not actually used it because the Linux version is ancient, by comparison to the windows version.)

### [TextMate](http://macromates.com/)

My now ex-boss [Donald Marino](http://donaldmarino.blogspot.com/) was a strong proponent of TextMate on Mac. I never really played with it or even watched him use it so I cannot comment on it directly but it is the choice of the Cool Kids.
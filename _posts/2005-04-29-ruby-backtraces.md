---
id: 106
title: Ruby Backtraces
date: 2005-04-29T13:52:47-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2005/04/ruby-backtraces/
permalink: /blog/2005/04/ruby-backtraces/
categories:
  - Software Development
tags:
  - Software
---
There is something uniquely happy making about coding in Ruby. However, there are a few things about Ruby that bug me. I think the one I find most annoying is the backtrace. This might seem odd, because it appears to be such a minor thing, but I think the current backtrace noticeably reduces my productivity.

I spent that last 6 years coding in a dynamic language that included argument data in backtraces. Now that I am using languages like Ruby, and Java, that do not include this information I really miss it. Consider the following standard Ruby backtrace. 

<pre>NoMethodError: undefined method `capitalize' for nil:NilClass
	from /home/pwilliams/projects/tmp/test.rb:18:in `greet_user'
	from /home/pwilliams/projects/tmp/test.rb:14:in `third'
	from /home/pwilliams/projects/tmp/test.rb:10:in `second'
	from /home/pwilliams/projects/tmp/test.rb:4:in `first'
	from (irb):41
	from :0
</pre>

The only thing you know from this backtrace is that the greet\_user method tried to use a nil when it should have used an object that responded to capitalize. You do not even know on what classes these methods are defined. To debug this you have to start by looking at greet\_user, if it looks good you move on to third and repeat that process until you find the problem. Or restart the app with debugging and then try to recreate the state that caused the problem.

Now consider the same backtrace with argument information.

<pre>NoMethodError: undefined method `capitalize' for nil:NilClass
	from /home/pwilliams/projects/tmp/test.rb:18:in main.greet_user(nil, "williams")
	from /home/pwilliams/projects/tmp/test.rb:14:in main.third(nil, "williams")
	from /home/pwilliams/projects/tmp/test.rb:10:in main.second("williams, peter")
	from /home/pwilliams/projects/tmp/test.rb:4:in main.first("Peter Williams")
	from (irb):41
	from :0
</pre>

There is, obviously, a lot more information in the second backtrace. Enough, in fact, that you can reliably guess the root cause of the problem. You can probably tell, even without seeing the code, that the bug is in Object#second. It is very unlikely that Object#third was suppose to be called with nil as it&#8217;s first argument. It is obvious that the bug is in Object#second, or something that method calls, even though the exception was raised in Object#greet_user. With the current backtrace you are forced to read all the code between the place that raises an exception and the place where the actual bug is located. In many cases this is a lot of code.

The vast majority of bugs can be localize to the correct method using this type of informative backtrace. This means less time spent firing up a debugger or instrumenting the code with write or log statements. It also means that if something goes wrong in the wild you can get a pretty good idea of the problem, simply from the backtrace in the log file.

This is especially frustrating because backtraces are created from an array of data that is passed to Exception object right after they are raised. This array is just a bunch of dumb data extracted from the call stack by the VM, not the call stack, like it should be. This means that argument information cannot be added to backtraces &#8212; as far as I can tell, at least &#8212; without modifying the VM, which beyond my abilities at the moment. The information in a backtrace ought not be hard coded in the VM. Would it really have been that difficult to reify the call stack and pass that to raised Exceptions? 

<p class='update'>
  Added receiver to alternate backtrace format.
</p>
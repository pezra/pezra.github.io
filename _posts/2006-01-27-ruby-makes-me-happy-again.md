---
id: 202
title: Ruby Makes Me Happy, Again
date: 2006-01-27T13:54:34-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=202
permalink: /blog/2006/01/ruby-makes-me-happy-again/
categories:
  - Software Development
tags:
  - Software
---
I have mentioned it the past that I now to use Ruby for many tasks that in the past I used Bash. Today that happened again. I wrote some little script to create and destroy ssh tunnels. This is pretty straight forward except for one little problem. <samp>ssh -fNL &#8230;</samp> will launch the tunnel and put it in the background after collecting your password, but because the ssh command puts itself into the background `$!` is not set by the shell. That means that when it comes time to shutdown the tunnel you have to figure out the pid from <samp>ps</samp>. Writing the ps and grep command to get you the line describing the process of interest is easy and sed can extract the appropriate column value without any problems. The problem is that I have to go searching the Internet every time I use sed because the only command I often enough to remember reliably is substitute.

So today, instead of spending 30 minutes figuring out how to get sed to extract the pid from the ps output I send less than ten writing this 

<pre class='code'>local_port = "55#{port_to_forward}"

if pid = `ps -C ssh -o pid,cmd`[/[[:digit:]]+(?=.*L\s+#{local_port}:)/]
  `kill -9 #{pid}`
else
  puts "There is no ssh tunnel on #{local_port}."
  exit 1
end
</pre>

Nothing too complicated there but it is a easier for me write than the equivalent sh using grep, sed, etc.

One thing I do want to point out, because it make me giddy every time I use this feature, is the use of a regular expression as the argument to the #[] method. Most other languages in which I have worked supported some syntax like <samp>&#8220;my string&#8221;[2]</samp> that yields the character specified by the number inside the square brackets. Ruby allows that usage but, as an alternative, you can pass in a regular expression and the #[] method will return the portion of the string the regular expression matches. So what the &#8220;<samp>if pid = &#8230;</samp>&#8221; line says is list all the ssh commands running and from that extract first number that is followed by the arguments used to create the ssh tunnel. That first number is always pid of the process that needs to be killed in order to shutdown the tunnel.

One of the things I really appreciate about Ruby is the fact that is useful for everything from 10 line scripts to highly scalable and reliable web applications. That is what I call a general purpose language.
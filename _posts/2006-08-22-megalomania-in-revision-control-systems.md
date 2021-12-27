---
id: 253
title: Megalomania in Revision Control Systems
date: 2006-08-22T11:22:17-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2006/08/megalomania-in-revision-control-systems/
permalink: /blog/2006/08/megalomania-in-revision-control-systems/
categories:
  - Uncategorized
tags:
  - Perforce
  - SCM
  - Software
---
The more I work with Perforce the more annoyed I get with it&#8217;s belief that it is the center of the software development process. Not that Perforce is not unique in believing this. In fact, many commercial revision control systems have this same megalomania.

So, commercial revision control vendors, let me clue you in. Revision control systems are _not_ the center of the software development universe, the file system on the developer&#8217;s machine is. Period. The primary job of a revision control system is to figure out what the developer did, after the developer has already done it, and to save that so that it can be applied to the other developers local file systems.

I am complaining about this because out of the box Perforce does not include a way to examine your working directories and get a list of what has changed since the last time you synchronized with the depot (Perforce&#8217;s word for a repository). This means that you must declare any changes you make to the source code tree before, or at the same time as, you make them. If you do not declare the changes up front it is almost certain that some required changes will be forgotten and will not get checked in.

The main problem with this approach is that it compels you to [break your flow](http://www.joelonsoftware.com/articles/fog0000000022.html). You might not think that just having to execute one additional command would really be that bad. After all, executing a command could probably be incorporated into your personal process and thereby have little or on impact on your flow. In practice, however, it really does break the flow.

The true cost of declaring that a new file will exist with a particular name is not in the act of declaration. Rather, it is in the thought and commitment that such a declaration requires. Every day, I created new classes only to notice a couple of minutes later that it has morphed in to something quite different than what I originally intended. Declaring the name of a new file to the revision control system before I have even created it dramatically raises the cost of such emerging behaviors. To name a file well I need to understand it&#8217;s purpose and to do that I usually need to create it and put some code in it to see how it turns out.

Having to declare the name of the file first requires a significant amount of thought and, worse of all, it prematurely solidifies the design. This premature solidification of design is costly both in terms of the time it takes and the quality risks it introduces. Paul Graham makes a compelling case for a fluid approach to software design in [Hackers and Painters](http://www.paulgraham.com/hp.html). Forcing stability on an immature design merely locks in its deficiencies and that is exactly the result of requiring the declaration a file before it is created.

The need to declare modifications and deletions up-front is less onerous. Deletions are easier because, generally, it is easy to understand the consequences of deleting a file because there is only one. Modifications are easier because marking a file at &#8220;to be edited&#8221; is quite forgiving. Even if you failed to actually modify the file checking it in will have no functional impact. None the less, having to declare you intentions up-front is still annoying, and wasteful since they could be determined after the fact.

If you are a commercial revision control system vendor please take note. You are not, nor will you ever be, the center of the software development universe. Pretending that you are just costs your users time and effort that could be better spent solving real problems.
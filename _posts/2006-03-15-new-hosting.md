---
id: 214
title: New Hosting
date: 2006-03-15T17:22:50-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=214
permalink: /blog/2006/03/new-hosting/
categories:
  - Miscellaneous
---
If you are reading this your looking at my shiny new hosting. I finally had to leave shieldhost because their support was simply inadequate. It took a long time (days usually) and I had to explain the issue multiple times for almost every single ticket I submitted. And then yesterday my host ran out of space in the tmp directory which caused my blog to be down for most of the day<footnote>WordPress queries the database in such a way that it often has to write the data to disk due to the size</footnote>. 

Anyway, I signed up with [Site5](http://www.site5.com/) this morning and now everything is humming along. So far I am very pleased. Getting shell access is the only issue I have had so far and it approximately 2 minutes from the time I sent my email requesting shell access until I had it. It was was so fast that at first I assumed that it was an auto-response<footnote>Shieldhost often took 4+ hours before the first response.</footnote>. Even better Site5 supports RoR so if I ever get around to writing some cool rails app in my copious free time I can deploy them on barelyenough.org.

So far my only complaint is that the IMAP server does not support STARTTLS so I still have to get my mail insecurely. But apparently no one actually supports this functionality.
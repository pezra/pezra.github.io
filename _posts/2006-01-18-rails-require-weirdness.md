---
id: 197
title: Rails Require Weirdness
date: 2006-01-18T18:30:03-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=197
permalink: /blog/2006/01/rails-require-weirdness/
categories:
  - Software Development
tags:
  - Rails
---
Can someone please explain why adding `require 'xmlmarkup'` to the beginning of application_helper.rb causes the file to not be loaded? Instead you get this error message in the log, <samp>ApplicationController: missing default helper path application_helper</samp>, as if the file simply did not exist. And, of course, a failure to render any pages that make use of any of the application helper methods, since they are not available.

I strongly suspect that it is related to the auto-loading magic that Rails has but it is not clear to me exactly how it is related. I _do_ know, now, at least, that it is unnecessary to explicitly require this file in order to use `Builder::XmlMarkup`, but it seems like it should be allowed. After all, requiring the file seems like the most obvious choice if you know Ruby better than Rails. The non-obviousness of this problem combined with the fact that none of the error messages generated specify the actual problem make this problem a bit annoying.
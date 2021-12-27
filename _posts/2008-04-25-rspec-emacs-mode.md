---
id: 327
title: RSpec Emacs Mode
date: 2008-04-25T09:52:21-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=327
permalink: /blog/2008/04/rspec-emacs-mode/
categories:
  - Software Development
tags:
  - bdd
  - Emacs
  - RSpec
  - Ruby
---
I just released a small Emacs minor mode, [rspec-mode](http://pezra.barelyenough.org/projects/rspec-mode) that provides some convenience functions related to dealing with [RSpec](http://rspec.info).

So far this minor mode provides some enhancements to ruby-mode in the contexts of RSpec specifications. Namely, it provides the following capabilities:

  * toggle back and forth between a spec and it&#8217;s target (bound to `\C-c so`)

  * verify the spec file associated with the current buffer (bound to `\C-c ,`)

  * verify the spec defined in the current buffer if it is a spec file (bound to `\C-c ,`)

  * ability to disable the example at the point (bound to `\C-c sd`)

  * ability to reenable the disabled example at the point (bound to `\C-c se`)

Try it out ([download and repo details are here](http://pezra.barelyenough.org/projects/rspec-mode)) and let me know if you find any problems or make any improvements.
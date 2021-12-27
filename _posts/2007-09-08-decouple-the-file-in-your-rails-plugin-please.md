---
id: 293
title: Decouple the File in your Rails Plugin, Please
date: 2007-09-08T22:39:43-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/09/decouple-the-file-in-your-rails-plugin-please/
permalink: /blog/2007/09/decouple-the-file-in-your-rails-plugin-please/
categories:
  - Software Development
tags:
  - plugin
  - Rails
  - Ruby
---
Defining new behavior for core Rails classes in mixins is a common pattern in Rails plugins. This allows for a separation of concerns that improves maintainability and digestability. However, it raises a bit of question about where the mixin inclusion step should take place. Should it happen in the plugin&#8217;s `init.rb` or in the same file as the mixin module is defined?

I recently had cause to use a couple of Rails plugins outside of a rails project. This experience has allowed me to resolve this question, and the answer is: include the new behavior in the core classes in the same file that you define the behavior. That way if you need to use a subset of the behaviors defined in a plugin you can just require that file. The people who use your plugin in ways you don&#8217;t yet anticipate will thank you.

Oh yeah, and please don&#8217;t rely on the automatic module creation that dependency base autoloading in ActiveSupport provides. It is a lot simpler for your users if you explicitly define all the modules you need.
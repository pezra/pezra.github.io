---
id: 268
title: Even the errors are happy making
date: 2007-03-07T10:44:51-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/03/even-the-errors-are-happy-making/
permalink: /blog/2007/03/even-the-errors-are-happy-making/
tags:
  - Rails
---
This has got to be my favorite RoR error message:

> 1) Error: test_truth(Admin::EscalationMgt::EscalationViewsControllerTest): ArgumentError: Admin::EscalationMgt is not missing constant EscalationViewsController!

Only in Rails would the fact that you are trying to use a constant that _does_ exist be an error.
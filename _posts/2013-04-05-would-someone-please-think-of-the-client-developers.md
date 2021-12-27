---
id: 952
title: Would someone please think of the client developers?!?
date: 2013-04-05T05:00:32-06:00
author: Peter Williams
layout: post
guid: https://barelyenough.org/?p=952
permalink: /blog/2013/04/would-someone-please-think-of-the-client-developers/
categories:
  - Software Development
tags:
  - api design
---
It seems that most APIs &#8212; particularly internal ones &#8212; are not designed for ease of use but rather to be easy to implement. No one would expect a human facing product designed that way to be successful. We should not expect APIs to be any different. 

Web APIs are products in their own right. That means all those rules for building great products, like understanding your users and their use cases, apply. APIs are not just high latency, bandwidth hogging database connections. Rather an API should expose an application and the business value it provides. This means understanding what clients want to accomplish and then affording those uses in easy, intuitive ways. 

Communication with the users is the key to designing a great API. As with other types of products, it is often necessary to build the first version of an API before there are any developers using it. We are on shaky ground until our design is validated actual clients. As soon as there are actual, or even potential, client developers listening to, and integrating their feed back should be priority number one.

Listening doesn&#8217;t mean reflexively implementing every whim of users &#8212; users are not always right about the details &#8212; but by understanding what they are trying to accomplish we as API designers can build systems that afford those goals with a minimum of effort on the part of client developers. Facilitating that value creation should be our main goal as API designers.
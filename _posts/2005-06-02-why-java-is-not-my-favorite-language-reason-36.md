---
id: 116
title: 'Why Java is Not My Favorite Language &#8212; Reason #36'
date: 2005-06-02T08:40:57-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2005/06/why-java-is-not-my-favorite-language-reason-36/
permalink: /blog/2005/06/why-java-is-not-my-favorite-language-reason-36/
categories:
  - Software Development
tags:
  - Software
---
Member access modifiers apply that the class level not the object level. For example, the following perfectly legal in Java.

<pre class='code'>class Foo
{
    private String name;

    public String someOneElsesName(Foo another)
    {
        return another.name;
    }
}
</pre>

The method `someOneElsesName()` returns the value of a private instance variable of an object that is _not_ one on which the method was executed. This is suppose to promote good encapsulation?
---
id: 248
title: Namespaces
date: 2006-07-16T20:00:41-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=248
permalink: /blog/2006/07/namespaces/
categories:
  - Software Development
tags:
  - Software
---
[Alex Bunardzic has written an interesting article about namespaces](http://jooto.com/blog/index.php/2006/07/14/the-namespaces-controversy/). He calls into question the usefulness of the canonical approach of hierarchically structured namespaces. I think he&#8217;s onto something but his example are pretty weak. It is easy to pick on the Java package names because they are often over the top. Unfortunately, Java package names are the namespaces that are most familiar. Most of the weirdness of Java package names come the fact that they tend to be used as way to categorized entities<sup id='2acc4bbfbc7d81de449d8b9d05bec855b778689bfnref:1'><a href='#2acc4bbfbc7d81de449d8b9d05bec855b778689bfn:1' rel='footnote'>1</a></sup>. Using namespaces as a categorization mechanism is misguided and generally results in package names that are both bad namespaces and bad categories.

The primary use of namespaces is name disambiguation. This is a vital feature of programming languages. Any language without integrated namespace support has a gaping hole. Name disambiguation is provided by allowing a name in particular namespace and that same name in another namespace to co-exist simultaneously and independently .

Name disambiguation is necessary when multiple implementations of the same logical class exist. Name disambiguation is rarely required within the application layer. Developers generally have enough visibility into their own code base to avoid implementing duplicate classes. When naming conflicts occur it is usually between two independent libraries or between the application and the libraries it uses.

Namespaces should be used exclusively to prevent name conflicts. Class names should always include enough information so that a reasonably information person will know the purpose of the class. This means if you have an accounts payable object it is called &#8220;AccountPayable&#8221; regardless of what namespace in which it lives.

Name conflicts are a real issue. Code that will be used in an unknown environment should be in a non-default namespace. That means if you are writing a reusable library it should should have it&#8217;s own namespace. Namespaces should usually match independently installable components. Any namespace that you can only get by installing some other package probably should not exist.<sup id='2acc4bbfbc7d81de449d8b9d05bec855b778689bfnref:2'><a href='#2acc4bbfbc7d81de449d8b9d05bec855b778689bfn:2' rel='footnote'>2</a></sup>

Just to be clear, namespaces are pure overhead. They are necessary in many situations but namespaces add no value to the application. One way to minimize the overhead is to put your application classes in the default (or null or root or whatever your language calls it) namespace. That means that the classes that you spend most of your time using do not require any namespace overhead. You will be responsible for ensuring that your code base does not include multiple classes with the same name. That is a constraint you should embrace because having multiple classes with the same name is just confusing.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='2acc4bbfbc7d81de449d8b9d05bec855b778689bfn:1'>
      <p>
        I use the word entity because in Java the things you name are not really objects.
      </p>
      
      <p>
        <a href='#2acc4bbfbc7d81de449d8b9d05bec855b778689bfnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='2acc4bbfbc7d81de449d8b9d05bec855b778689bfn:2'>
          <p>
            An interesting effect of this is that in a system with installation type packaging built in at the core you could treat namespaces as a aspect of the package management system. I wonder if you could retrofit that on top of <a href='http://rubygems.org/'>RubyGems</a> such that loading a gem resulted in all the classes being contained in a module with the same name as the gem?
          </p>
          
          <p>
            <a href='#2acc4bbfbc7d81de449d8b9d05bec855b778689bfnref:2' rev='footnote'>&#8617;</a></li> </ol> </div>
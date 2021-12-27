---
id: 275
title: Rake is Sweet
date: 2007-04-27T09:31:24-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/04/rake-is-sweet/
permalink: /blog/2007/04/rake-is-sweet/
categories:
  - Software Development
tags:
  - Rails
  - Software
---
[Rake](http://docs.rubyrake.org/) is a really excellent [build tool](http://en.wikipedia.org/wiki/Build_tool). It is basically [Make](http://en.wikipedia.org/wiki/Make) on steroids (and minus a few of the annoying inconveniences of make). If you build software of any sort you owe it to yourself to check out Rake.

The source of my Rake related euphoria today is that I just used a feature of Rake that is not available in any other build tool that I know. Namely, I added an action to an existing task<sup id='cae4fe0f8cf322726a2f90050be73d3dbfffda04fnref:1'><a href='#cae4fe0f8cf322726a2f90050be73d3dbfffda04fn:1' rel='footnote'>1</a></sup>. This feature allows you to extend the behavior of a task that you do directly own. Say for example, ones defined by the [framework](http://rubyonrails.org) you are using.

My particular situation was this. I have some data that is absolutely required for the application to function (permissions data in this case). Changes to this data don&#8217;t happen at run-time and the code explicitly references these records. Which means that while this information is stored in the database it is more akin to code and the data model than it is to the data managed by the application.

Given that this data is reference explicitly by the code it must reside in source control. [Rails migrations](http://wiki.rubyonrails.org/rails/pages/UnderstandingMigrations) are and excellent way to manage changes to the data model of an application and, as it turns out, the foundation data too. If you need to add or change some of this foundation data you can just write a migration to add, update or delete the appropriate records.

There is one slight issue with using migrations to manage foundation data, though. Only the structure of the development database gets automatically copied to the test database. So the code that requires the foundation data will fail its tests because that data does not exist. I have [run into this problem before](http://pezra.barelyenough.org/blog/2006/03/structural-data-in-rails/). That time I solved it by changing the way Rails creates the test database such that it used the migrations rather than copying the development database&#8217;s structure. It is a very nice approach but unfortunately it does not work for my current project.

To solve my problem this time I simply added an action to the `db:test:prepare` task to copy the data from the table. The standard `db:test:prepare` task provided by [Rails](http://rubyonrails.org) dumps the development database&#8217;s structure and then creates a clean test database using that dump. For our project it still does but then it follows that up by dumping the data from roles table and loading that into to the test database also.

Extending the `db:test:prepare` task means that all the tasks get an appropriate test database when they need it. And without me having to go around and added a dependency to all of them. I love it when my tools let me solve my problems easily.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='cae4fe0f8cf322726a2f90050be73d3dbfffda04fn:1'>
      <p>
        A Rake task is equivalent to a target in Make and Ant
      </p>
      
      <p>
        <a href='#cae4fe0f8cf322726a2f90050be73d3dbfffda04fnref:1' rev='footnote'>&#8617;</a></li> </ol> </div>
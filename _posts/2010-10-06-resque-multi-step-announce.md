---
id: 520
title: Resque-multi-step
date: 2010-10-06T05:08:46-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=520
permalink: /blog/2010/10/resque-multi-step-announce/
aktt_notify_twitter:
  - 'yes'
aktt_tweeted:
  - "1"
categories:
  - Software Development
tags:
  - resque
  - Ruby
---
I&#8217;ve been developing using asynchronous jobs quite a bit lately.<sup id='fnref:1'><a href='#fn:1' rel='footnote'>1</a></sup> There is only one reason to do work asynchronous. It takes too long to do it synchronously.

Fortunately, it turns out that many of these very large work loads are [embarrassingly parallel problems](http://en.wikipedia.org/wiki/Embarrassingly_parallel). And look, you have several (dozen) workers just waiting to do your bidding. It just makes sense to break up these large blocks of work into many smaller chunks so that it can be processed in parallel.

Breaking a task up into many small parts comes with some issues. Any task that takes long enough to run asynchronously is probably going to take long enough that you need to track its progress.

Also the problem is probably not completely parallelizable. Most problems seem to have a large portion of easily parallelized work followed by a bit of work that can only happen after all the parallel work has been complete.

These patterns show up often enough that i have gotten tired of repeating myself. Hence was born [resque-multi-step](http://github.com/pezra/resque-multi-step). Resque-multi-step is a [Resque](http://github.com/defunkt/resque) plugin that provides compound job support complete with progress tracking, error handling, and a completely serial finalization sequence.

## Example {#example}

Say you want to reindex all the posts in a blog. However, committing solr for each post would be excessively slow. (Trust me, it really is.)

    Resque::Plugins::MultiStepTask.create("reindex-#{blog.name}") do |task|
      blog.posts.each do |post|
        task.add_job ReindexWithoutCommit, post
      end
      
      task.add_finalization_job CommitSolr
    end

This reindexs all the posts in parallel. Any available workers will pick up a job to reindex a specific blog post. Once all those reindex jobs have completed, the finalization job will be executed.

If you have more that one finalization job, they are executed serially in the order they were added to the task.

## Administrivia {#administrivia}

If these issues sound familiar give resque-multi-step a try. It is available as a gem so installing is just

    gem install resque-multi-step

If you want to contribute head on over to the [github project](http://github.com/pezra/resque-multi-step) and hack away. If you come up with something useful i&#8217;ll integrate it post haste.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='fn:1'>
      <p>
        <a href='http://github.com/pezra/resque-fairly'>resque-fairly</a> was one of the first public out comes of such work. The fair scheduling it provides the basis for <a href='http://github.com/pezra/resque-multi-step'>this effort</a>.
      </p>
      
      <p>
        <a href='#fnref:1' rev='footnote'>&#8617;</a></li> </ol> </div>
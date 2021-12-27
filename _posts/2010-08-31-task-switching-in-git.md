---
id: 508
title: Task switching in Git
date: 2010-08-31T05:00:57-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=508
permalink: /blog/2010/08/task-switching-in-git/
aktt_notify_twitter:
  - 'yes'
aktt_tweeted:
  - "1"
categories:
  - Uncategorized
---
This thing happens to me pretty often: i start a story, work on it for a while then something urgent comes up.<sup id='fnref:1'><a href='#fn:1' rel='footnote'>1</a></sup> The urgent thing needs to be fixed right away but i have a lot of changes in my working directory. Unfortunately, the changes i have made are incomplete and non-functional.

The usually suggested way to handle this is with [`git-stash`](http://www.kernel.org/pub/software/scm/git/docs/user-manual.html#interrupted-work). For a long time, i used stash in this situation myself. However, i often found myself lost in the stash queue. If you use stash to store unfinished work your stash queue can become quite long. It is easy to forget you have stashed work. It is also easy to do a `git stash clear` and lose that work.

There are lots of situations in which it can be quite a while before you get back to your stashed changes. For example, if you switch tasks because the business deprioritized the feature. Or if the urgent issue gets interrupted by an emergency issue.

It recently occurred to me that git provides a much more elegant way to deal with unfinished work.

## The steps {#the_steps}

First, always work in a feature branch. You should be doing this anyway but it is required for this technique to work.

  1. `git add -A` (on the feature branch)
  2. `git commit -m &#39;WIP&#39;`
  3. Switch branches and fix that urgent issue. Using git like you always do.
  4. `git checkout <feature-branch>`
  5. `git reset HEAD~1`
  6. Continue where you left off. Once you are ready, commit.

This approach commits you in-progress work on the branch to which it belongs, keeping it safe.

## How it works {#how_it_works}

Once you do your WIP commit your history will look something like:

![](http://barelyenough.org/blog/uploads/task-switching-in-git/git-commits-wip.png) 

That is great for temporarily storing your in-progress work. We definitely don&#8217;t want that nasty &#8220;WIP&#8221; commit in our history long term, though. The `git reset HEAD~1` command changes the HEAD pointer of the feature branch back to the commit immediately before the &#8220;WIP&#8221; commit. That leave a commit graph something like:

![](http://barelyenough.org/blog/uploads/task-switching-in-git/git-commits-reset.png) 

Once you have completed your changes and committed the HEAD pointer of the feature branch will be updated to point the new commits. This leaves the &#8220;WIP&#8221; commit out of the commit history of the branch forever.

![](http://barelyenough.org/blog/uploads/task-switching-in-git/git-commits-final.png) 

The &#8220;WIP&#8221; commit is now &#8220;unreachable&#8221; because no objects or references in the system point to it. It will be removed the next time you do a `git gc`.

`git stash` definitely has it place but i reserve it for situations where i am going to pop the stash very quickly (eg, i stash, the checkout a different branch, then pop).

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='fn:1'>
      <p>
        I do a lot of customer integration. Once a customer starts testing it is important to keep the turn around on their blocking issues to a minimum. If you don&#8217;t they get distracted and it&#8217;s no telling how long you&#8217;ll have to wait before they start testing again.
      </p>
      
      <p>
        <a href='#fnref:1' rev='footnote'>&#8617;</a></li> </ol> </div>
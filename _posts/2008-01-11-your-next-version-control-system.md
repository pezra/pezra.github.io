---
id: 311
title: 'I &hearts; DVCS'
date: 2008-01-11T23:53:53-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2008/01/your-next-version-control-system/
permalink: /blog/2008/01/your-next-version-control-system/
categories:
  - Software Development
tags:
  - dvcs
  - git
  - mercurial
  - subversion
  - tools
---
This week I worked on a project that uses [Subversion](http://subversion.tigris.org/) and, man, what a difference a year makes. [Back then I dreamed of being able use Subversion instead of Perforce](http://pezra.barelyenough.org/blog/2006/08/megalomania-in-revision-control-systems/). Now using svn feels a bit like walking around waste deep in water.

I have been using [Git](http://git.or.cz/) almost exclusively for the last couple of months. I am now firmly convinced that distributed version control systems, such as Git and [Mercurial](http://www.selenic.com/mercurial/wiki/), are the way of the future. The basic model of [dVCSs](http://en.wikipedia.org/wiki/Distributed_revision_control) matches with real world software development much more cleanly that the model imposed by most (if not all) centralized [VCSs](http://en.wikipedia.org/wiki/Revision_control).

Consider the scenario I ran into, I checked out an svn project and made my changes, then I svn up-ed and found one of the files I had edit had been changed in a way that resulted in a merge conflict, and a fairly complicated one at that. I manually resolved the conflict, merging the files by hand, and the commit the merged file, but what if I had gotten it wrong? My version of the file was never stored. Which means that after doing the `svn resolved` the merge cannot ever undone or fixed.

`svn up` is a branch merge which throws away the entire history of the target branch. Working directories are branches whether the VCS acknowledges it or not. Not acknowledging it simply results in branches in which the history is not tracked. A branch that does not track its history sounds silly. Because it is. None the less, that is the model that is forced by most VCSs.

Distributed VCSs, on the other hand, treat your working directory as the branch it really is. And that makes all the difference in the world.
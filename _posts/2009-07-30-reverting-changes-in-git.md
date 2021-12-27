---
id: 391
title: Reverting changes in git
date: 2009-07-30T10:42:15-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=391
permalink: /blog/2009/07/reverting-changes-in-git/
categories:
  - Software Development
tags:
  - git
---
Note to self: If you need to revert commits that have already been pushed, or otherwised merged with any other repo or branch, use `git<br />
revert`.

`git reset` is exclusively for undoing commits in your local working tree that have not seen the light of day. Attempting to use `git<br />
reset` to undo changes that exist in multiple trees (changes that have been pushed or merged in to another branch or repo) will result in pain and suffering.
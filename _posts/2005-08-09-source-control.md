---
id: 146
title: Source Control
date: 2005-08-09T13:57:41-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2005/08/source-control/
permalink: /blog/2005/08/source-control/
categories:
  - Software Development
tags:
  - Software
---
I am currently engaged in a battle with my source control system, [CMSynergy](http://www.telelogic.com/products/synergy/cmsynergy/index.cfm). Generally speaking source control is an invaluable bit of kit, but this one is crap. I am beginning to believe that I would be better off with just keeping this code on my local, failure prone, hard drive. I would definitely be better off with something like CVS or Subversion.

For those of you lucky enough to unfamiliar, CMSynergy is described on it&#8217;s website as a &#8220;a task-based configuration management solution&#8221;. Basically that means that it views the world as a series of tasks. All changes must be associated with a task. However, a task it not change set, a task cannot be committed or rolled-back atomically, it is merely a traceability device.

The problem with CMSynergy is that is not designed to facilitate software development. It is designed to give managers a warm comfortable feeling that they are controlling all the changes to the system. So all changes must be associated with a task, which should (and optionally must) be associated with CR which is associated with a requirement. In agile environments, which we claim to have, maintaining traceability is pretty close to meaningless busy work because I am going to refactor quite a bit of code that is only tangentially related to the feature/bug I am working on.

The policy enforcement role of CMSynergy is embedded deep in the systems design. It truly believes that it is the center of the software development process. For example, it you modify a file and then do an update, so that you can test your teammates changes with your own, CMSynergy will happily, and without asking you, replace your modified file with the version of the file it thinks is most appropriate. Now there is a way to tell CMSynergy that the local copy of the file is the appropriate version but you have to take an explicit step for every file you modify to prevent it from being overwritten.

If you develop a source control system please realize that it is not the center of the development process and that it&#8217;s primary job is to facilitate the development of software. Pretending otherwise will cause your users significant amounts of pain and suffering.
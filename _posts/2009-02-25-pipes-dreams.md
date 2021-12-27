---
id: 378
title: Pipes Dreams
date: 2009-02-25T23:22:30-07:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=378
permalink: /blog/2009/02/pipes-dreams/
categories:
  - Software Development
---
As you know, i am [on a job hunt](http://barelyenough.org/blog/2009/02/need-job/). Like everyone else i use a variety of sources to find potential opportunities. I also have a wide breadth of skills so i search on a lot of keywords. Going to 10 different websites and doing 2 to 20 searches each gets old really fast. Worse yet some of these sites don&#8217;t have particularly sophisticated search capabilities so you end up having to look at a bunch of information that is not relevant.

So, over the weekend i decided to play with [Yahoo Pipes](http://pipes.yahoo.com) to aggregate and filter all the job posting feeds. Pipes is quite impressive. The UI is very slick. The drag and drop visual connect-the-boxes presentation really puts the user at ease. Creating a basic version of the pipe i wanted was surprising easy. There was definitely some learning to be done but i had a working prototype up in very short order.

Almost immediately i was disappointed in the lack of a textual representation of the data extraction and transformation rules that comprise a &#8220;pipe&#8221;. Being a programmer by trade i always feel happier when i can use my favorite text editor, not to mention source control, for tasks like these. (You mean you don&#8217;t edited text fields in you browser with Emacs? [Why not](https://addons.mozilla.org/en-US/firefox/addon/4125)?) However, my [wife](http://pinkasparag.us), an [ETL](http://en.wikipedia.org/wiki/Etl) expert, assured me that visual editing is the norm for tools such as Pipes.

Once i started expanding from the initial prototype the dream really started collapsing around me. My pipe had a fairly complex set of filtering rules that i wanted to apply to each of a set of feeds. My initial thought was to extract those rules so that i could reuse them in multiple pipes, each handling data from difference sources. At it turns out, Pipes does not allow that. You can use other pipes as the input to a pipe, which is nice, but you cannot embed one pipes in the middle of another pipe. The inability to create custom reusable transformations is a real missed opportunity.

With that option off the table i decide to create one ginormous pipe that aggregated all the feeds before running their contents through the filters. That works, in principle, though it raised some factoring challenges in my case. (Some feeds required specified processing that other did not.) Once i had constructed my very large pipe i tried to save it, and this is what i got

![Problem Saving: problem parsing response](/blog/uploads/pipe-dreams-failed-save.png) 

Which is a little weird because it actually does seem to save when this happens. If i close the editor and reopen the pipe it has the changes. Unfortunately, it cannot seem to run the very-large-pipe reliably. Or at all, most of the time. Most of the time that pipe fails altogether. Sometimes it returns only a subset of the data that it should, but most of the time i get a &#8220;Pipes engine request failed (malformed engine data) (2)&#8221; error message. Regardless of the outcome, it always takes an unacceptable amount of time to run.

So now i am back at square one. I am faced with creating an expansive set of pipes which are largely duplicates on one another, but without even cut-and-paste reusability. (Did i forget to mention that the pipe editor does not support cut-and-paste?) So if i change my mind about the filtering criteria i would have to go change each of them by hand. I don&#8217;t think it would be worth the effort.

So, i guess i will go run my 40 searches manually every day and take the first job that comes along just to make the pain stop. Which really sucks because Pipes has real promise. After creating a small pipe i thought it was pretty much the coolest thing since sliced bread.
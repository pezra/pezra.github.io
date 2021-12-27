---
id: 250
title: Mercurial and Perforce
date: 2006-08-11T21:50:41-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=250
permalink: /blog/2006/08/mercurial-and-perforce/
tags:
  - Software
---
I have been learning two different source control systems in the last couple of weeks. Source control systems are a vital tool in any software development activity so I want to share my impressions of these tools.

### Mercurial {#mercurial}

I finally got around to setting up a [repository for my personal projects](http://pezra.barelyenough.org/hg)<sup id='40fabf91536f4644be1424077421e896d99a89f0fnref:1'><a href='#40fabf91536f4644be1424077421e896d99a89f0fn:1' rel='footnote'>1</a></sup>. For these projects, I chose to use [Mercurial](http://www.selenic.com/mercurial/). I have been hearing a bit of a buzz around Mercurial for a while and I decided it would do me good to try out one of these newfangled distributed source control systems<sup id='40fabf91536f4644be1424077421e896d99a89f0fnref:2'><a href='#40fabf91536f4644be1424077421e896d99a89f0fn:2' rel='footnote'>2</a></sup>. So far, I am impressed. Mercurial is easy to setup and use and it seems quite capable.

So far my only real complaint is that Mercurial does not version directories. It only versions files and the directory structure are treated as meta-data about those files. This makes it impossible to have empty directories in the source tree. Not a huge deal but it is annoying at times.

Distributed source control is a bit of a different mindset than I am use to. The sort of source control systems I am familiar with are built around the idea that there is one-true-version of the code and that the one-true-version lives in the central repository. I think it may take a while to get completely comfortable with a source control system that believes that there are lots of versions of a project all of which are equally valid.

My unfamiliarity aside, I like the distributed approach because I think it accepts the reality of software development. There are usually multiple useful and meaningful versions of a particular project at any given time and a tool that embraces that fact ought to work better than one that does not. I need to use Mercurial quite a bit more to be sure it lives up to that promise, but so far it is looking good.

### Perforce {#perforce}

At work, the SCM team recently settled on [Perforce](http://www.perforce.com/) as our standard source control system and I get to be the first person in my group to use it for a real project. I have been using it for a couple of weeks now and I can say that it is cross platform<sup id='40fabf91536f4644be1424077421e896d99a89f0fnref:3'><a href='#40fabf91536f4644be1424077421e896d99a89f0fn:3' rel='footnote'>3</a></sup>, reliable, fast. Those are all virtues, unfortunately Perforce falls way short on the ease of use.

For example, you might think that something like `p4 rename old_name.rb<br />
new_name.rb` would rename the file but in fact it just prints this

> See &#8216;p4 help rename&#8217; for instructions on renaming files.

And when you run that it prints a bit of text outlining a multi-step process, involving branching the file and then deleting the original, that will result in the file being renamed.

If you want to edit a file, don&#8217;t even think about just opening it with favorite editor and going to town. No, you need to explicitly inform Perforce that you are going to edit the file by running `p4<br />
edit myfile.txt`. As far as I can tell, Perforce does not have a way to list what you have changed since the last check-out (or sync in Perforce terminology).<sup id='40fabf91536f4644be1424077421e896d99a89f0fnref:4'><a href='#40fabf91536f4644be1424077421e896d99a89f0fn:4' rel='footnote'>4</a></sup> This means that you cannot just edit and change to your hearts and then notify perforce of the changes in a batch, no you have to interrupt your flow to inform the source control system that you are about to change a file.

The requirement to manually &#8220;open files for editing&#8221; and similar issues would probably be easier to deal with using the Perforce Emacs integration but I really despise tools that require IDE integration to be usable. Variety&#8217;s the spice of life. Sometimes Emacs will do it, sometimes &#8211; not often, but sometimes &#8211; I like the idea of using vi.

I have used [much worse source control systems](http://pezra.barelyenough.org/blog/2005/08/source-control/) than Perforce but I would never choose it over the veritable cornucopia of better, and free, source control systems available.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='40fabf91536f4644be1424077421e896d99a89f0fn:1'>
      <p>
        So far there is just one public project, <a href='http://pezra.barelyenough.org/hg/php-markdown-fn'>PHP Markdown (with footnotes)</a>.
      </p>
      
      <p>
        <a href='#40fabf91536f4644be1424077421e896d99a89f0fnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='40fabf91536f4644be1424077421e896d99a89f0fn:2'>
          <p>
            Well, all that <em>and</em> it is written in Python.
          </p>
          
          <p>
            <a href='#40fabf91536f4644be1424077421e896d99a89f0fnref:2' rev='footnote'>&#8617;</a></li> 
            
            <li id='40fabf91536f4644be1424077421e896d99a89f0fn:3'>
              <p>
                I have used it successfully on Windows, Linux, and FreeBSD with no problems.
              </p>
              
              <p>
                <a href='#40fabf91536f4644be1424077421e896d99a89f0fnref:3' rev='footnote'>&#8617;</a></li> 
                
                <li id='40fabf91536f4644be1424077421e896d99a89f0fn:4'>
                  <p>
                    I find it really hard to believe that there is no Perforce equivalent to the status command in svn, cvs, hg, etc. I look for it in the documentation every couple of day, but so far any way to do this has eluded me.
                  </p>
                  
                  <p>
                    <a href='#40fabf91536f4644be1424077421e896d99a89f0fnref:4' rev='footnote'>&#8617;</a></li> </ol> </div>
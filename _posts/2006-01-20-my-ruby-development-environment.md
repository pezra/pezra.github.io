---
id: 198
title: My Ruby Development Environment
date: 2006-01-20T13:57:25-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=198
permalink: /blog/2006/01/my-ruby-development-environment/
categories:
  - Software Development
tags:
  - Rails
  - Software
---
Recently, [Stephen O&#8217;Grady asked what is included in my Ruby tool set](http://www.redmonk.com/sogrady/archives/001217.html) so I will attempt to answer that question, and describe why I choose these tools<footnote>I speak of this in the present tense because the set of tools I use is always evolving. Everyday I choose afresh the tools I am going to use.</footnote>.

### Text Editor/IDE

A text editor or IDE is probably a software developers most important tool. I use the One True IDE (say that with lots of reverb for full effect), by which I mean Emacs, of course. (Several of my current and former colleagues are now throwing things at me, or wishing they were near enough to do so. Hi Charlie.) I choose Emacs as my primary development environment for a number of reasons, some technical some personal.

The most important reason I choose Emacs is that as ruby development environments go Emacs, with ruby-mode and the other related modes, is the most powerful and complete set of tools for doing Ruby development. I disagree with Mr. O&#8217;Grady&#8217;s characterization of the Emacs extensions as &#8220;simple&#8221;. They are, in fact, quite sophisticated, though certainly not the most sophisticated set I have seen. That fact, combined with the wide availability of other high quality Emacs modes for editing [XML, HTML](http://www.lysator.liu.se/projects/about_psgml.html "PSGML"), [CSS](http://www.garshol.priv.no/download/software/css-mode/ "css-mode") and [Javascript](http://emacsen.org/2005/09/26-javascript "Several choices") it makes an extremely powerful and general purpose development environment<footnote>If you are setting up an environment for doing web development [Darren Brierton has done a great writeup](http://www.dzr-web.com/people/darren/projects/emacs-webdev/).</footnote>.

Emacs is not without it&#8217;s downsides, however. The primary disadvantage of Emacs is that the learning curve is quite steep. The non-graphical nature of Emacs is a bit intimidating for many people and it does mean that it is difficult to be productive in Emacs until you have learned quite a bit. Graphical IDEs make it relatively easy to be somewhat productive right away and more powerful uses of the tool can be learned more slowly.

Personally, I find this learn slowly approach a disadvantage of graphical IDEs. Because the allow you to learn slowly the make you less productive over a longer period of time. For example, keyboard short-cuts radically improve your productivity. However, because I _can_ do any action with the mouse in a graphical IDE I rarely learn the short-cuts. In Emacs you learn the short-cuts because you are either typing the command long-hand (in which case it informs you that you could have just typed &#8220;C-c l&#8221;, or whatever) or you are typing the short-cut so learning the short-cut feels more practical. 

Another important factor in my decision to use Emacs is that none of the other Ruby IDEs support [IRB](http://poignantguide.net/ruby/expansion-pak-1.html). It is impossible to over value IRB. I consider any development environment that does not support IRB to be shamefully incomplete. So incomplete, in fact, that I will generally not use it beyond the point that I realize it does not support one of the most important tool in a rubyist&#8217;s arsenal.

### IRB

[IRB](http://poignantguide.net/ruby/expansion-pak-1.html) is basically just an interactive shell on top of the normal Ruby interpreter. IRB is a bit like [try ruby!](http://tryruby.hobix.com/) except with your application code available. It can be used integrated with your IDE if you are lucky enough to use an IDE that supports it or you can use it stand alone. Programmers who new to dynamic languages tend to not utilize IRB very much because it is a foreign concept, but it is so powerful that no rubyist should be with out it.

IRB allows you to greatly improve your productivity by continuously performing micro-spikes. These micro-spikes can be used to understand the behavior of third party (or your own) objects or to test potential approaches to solving a problem. IRB can also be extremely useful when testing because you can directly manipulate the objects in your application. For example, if you have some bug that only shows up when your application is in a state that is difficult, or time consuming, to get to you can often use IRB to directly manipulate your application into that state, by-passing the complicated set of actions it would take to achieve that state manually.

### Browser

Most of the software I am currently developing is web based. When doing web application development your choice of browser is almost as important as your choice of editor/IDE. I use Firefox exclusively, except when I am testing for browser compatibility. The primary advantage of Firefox, in my mind, is the wide availability of extensions that support web development. I should point out, that until my current job my focus has been on the server side of web applications so my knowledge of client side (browser) programming is somewhat more limited, but I am learning fast. I use the following extensions to Firefox

<ul class='xoxo'>
  <li>
    <a href='https://addons.mozilla.org/extensions/moreinfo.php?id=60'>Web Developer</a> <p>
      An absolutely brilliant set of tools that allow inspection and manipulation of web pages in the browser. Of particular use to me lately is the CSS editor which allows you edit and apply CSS rules to the current web page without reloading it.
    </p>
  </li>
  
  <li>
    <a href='https://addons.mozilla.org/extensions/moreinfo.php?id=655'>View Rendered Source Chart</a> <p>
      A replacement for the standard view source functionality what provides nicer styling and visually groups the members of a block. It is especially nice when doing Rails development because it collapses the large block of generated JavaScript that Rails inserts at the beginning of forms. This make the forms much easier to read.
    </p>
  </li>
  
  <li>
    JavaScript Console <p>
      While not technically an extension (I think) this is a valuable tool that presents the errors generated by the JavaScript on a page and allows for ad-hoc bits of JavaScript to be executed on demand.
    </p>
  </li>
  
  <li>
    <a href='https://addons.mozilla.org/extensions/moreinfo.php?id=216'>JavaScript Debugger</a> <p>
      I have heard that this does not work well on Firefox 1.5 but I have not had need of it since I upgraded so I cannot comment from experience. Theoretically, it allows relatively easy debugging of the JavaScript on a web page.
    </p>
  </li>
</ul>

### Conclusion

So there you have it. Those combined with the standard Unix tools (ie, find, grep, less, sed, curl, etc) are what I use to do Ruby (and Rails) development. I also find my self writing quite a few small scripts in Ruby if I need some behavior that is fairly simple but too complex to achieve with just one line in bash.

Hope this is what you were looking for Steve.

<p class='update'>
  {Update: Fixed link to Steve&#8217;s post}
</p>
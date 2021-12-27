---
id: 258
title: Speaking of little happy making things
date: 2006-10-05T23:47:04-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2006/10/speaking-of-little-happy-making-things/
permalink: /blog/2006/10/speaking-of-little-happy-making-things/
categories:
  - Software Development
---
When, exactly, did bash become smart enough to handle command completion for sub-commands? I noticed this today on my Ubuntu (Dapper) box. I, unintentionally<sup id='f796deaf56b725e05033d68b42775eee336cbde1fnref:1'><a href='#f796deaf56b725e05033d68b42775eee336cbde1fn:1' rel='footnote'>1</a></sup>, typed `hg <tab><tab>`. Rather than getting the &#8220;Display all 100 possibilities? (y or n)&#8221; I expected, I got

    add         copy        hgext/hgk]  manifest    rawcommit   tag
    addremove   cp          history     [module     recover     tags
    annotate    ct          id          mv          remove      tip
    bundle      diff        identify    out         rename      unbundle
    cat         export      import      outgoing    revert      undo
    checkout    forget      in          parents     rm          up
    ci          grep        incoming    patch       root        update
    clone       heads       init        paths       serve       verify
    co          help        locate      pull        st          version
    commit      hgext/hct]  log         push        status      view
    pwilliams@pwilliams:~$ hg 

Which is the list all the valid sub-commands for hg. And if I do `hg<br />
d<tab>` it auto-completes the sub-command to `hg diff`. Same thing for `cvs` and `apt-get`. It seems that bash now support full auto-completion of sub-commands for most programs<sup id='f796deaf56b725e05033d68b42775eee336cbde1fnref:2'><a href='#f796deaf56b725e05033d68b42775eee336cbde1fn:2' rel='footnote'>2</a></sup>.

That is just cool.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='f796deaf56b725e05033d68b42775eee336cbde1fn:1'>
      <p>
        I unconsciously double tap the tab key whenever I stop typing because I am addicted to auto-complete and Emacs contextually correct indentation.
      </p>
      
      <p>
        <a href='#f796deaf56b725e05033d68b42775eee336cbde1fnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='f796deaf56b725e05033d68b42775eee336cbde1fn:2'>
          <p>
            It does not work for <code>gem</code>, though.
          </p>
          
          <p>
            <a href='#f796deaf56b725e05033d68b42775eee336cbde1fnref:2' rev='footnote'>&#8617;</a></li> </ol> </div>
---
id: 247
title: PHP Markdown with Footnotes
date: 2006-07-12T10:16:12-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=247
permalink: /blog/2006/07/php-markdown-with-footnotes/
categories:
  - Miscellaneous
tags:
  - Software
  - Web Applications
---
I have been using [Markdown](http://daringfireball.net/projects/markdown/syntax) to write my blog posts for quite some time now and I really like it. I was a little surprised how much I liked it when I first switched because before I had been writing in HTML which works well. But it really is easier to focus on the content when you don&#8217;t have to worry about all the minutiae involved with XML based formats.

The only real problem I had with Markdown (the syntax and [PHP Markdown](http://www.michelf.com/projects/php-markdown/)) was that it does not support footnotes. I really like footnotes so that was sad. Many months ago I noticed the [footnote support](http://fletcher.freeshell.org/wiki/MultiMarkdown#footnotes) in [MultiMarkdown](http://fletcher.freeshell.org/wiki/MultiMarkdown). It is really clean and the intent is obvious from the text. I liked it so much so I started using it in places that I never intended to convert to another format (readme files, comment blocks in code, etc). The whole time since I noticed the footnote syntax I have been waiting for someone to add that feature to [PHP Markdown](http://www.michelf.com/projects/php-markdown/) so that I can use it with WordPress.

Well, I finally got tired of waiting, so yesterday I implemented footnote support in PHP Markdown<sup id='ccdacaab1be1ae3e33a0a002db690fc14bbaa504fnref:1'><a href='#ccdacaab1be1ae3e33a0a002db690fc14bbaa504fn:1' rel='footnote'>1</a></sup>. The result is a drop-in replacement for the original PHP Markdown<sup id='ccdacaab1be1ae3e33a0a002db690fc14bbaa504fnref:2'><a href='#ccdacaab1be1ae3e33a0a002db690fc14bbaa504fn:2' rel='footnote'>2</a></sup>. You can download it [here](/code/phpmarkdownextra1_0_1_fn.tar.gz).

Feel free to use it to your hearts content but be aware that it is &#8211; how shall I put this &#8211; fairly minimally tested<sup id='ccdacaab1be1ae3e33a0a002db690fc14bbaa504fnref:3'><a href='#ccdacaab1be1ae3e33a0a002db690fc14bbaa504fn:3' rel='footnote'>3</a></sup>.

### Defining footnotes {#defining_footnotes}

A footnote is defined like this

    [^my-footnote]: Explain the tangential thing that just occurred to me.

This will product the following at the end of the output (if it is referenced).

    <ol class="footnotes">
    <li id="footnote-my-footnote-(markdown document unique id)">
    <p>Explain the tangential thing that just occurred to me.</p>
    </li>
    </ol>

The &#8220;markdown document unique id&#8221; is an md5 hash of the original Markdown text we are converting. This is needed to keep footnotes with the same name but in different articles separate from one another when they appear on the same HTML page, as they do in a blog.

The text content of the footnote is processed as a normal Markdown block so you can use all your favorite markdown syntax, both block and span elements, inside of it. A footnote is all the text from the `[^footnote-name]:` bit until the first line with a non-space character in it&#8217;s first column following a blank line. For example

    [^my-footnote]: Something tangential just 
        occurred to me.
        
        This is a second paragraph in my footnote.
    
    This is just another paragraph in the document, *not* part of the
    footnote.

Code in is also supported in footnotes but it must be indented eight spaces, or two tabs, similar to having code in a list.

### Referencing footnotes {#referencing_footnotes}

Footnotes that are never references are stripped so to get the above output you need to reference the footnote somewhere else in the document. This is done like

    I have some stuff to say[^or-not].

If there is an &#8216;or-not&#8217; footnote defined, the following output will result

    <p>I have some stuff to say<a href="#footnote-or-not-(markdown document unique id)" class="footnote-ref">1</a>.</p>

References to undefined footnotes are ignored and the `[^name]` text is left in the output.

The numbering of footnotes is based on the order of the references, not on the order of definition. So the first footnote you define may end up with a number other than 1 if it is not the first footnote referenced.

Happy Marking down.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='ccdacaab1be1ae3e33a0a002db690fc14bbaa504fn:1'>
      <p>
        I started from version &#8220;Extra 1.0.1&#8221; and added a few functions to handle footnotes.
      </p>
      
      <p>
        <a href='#ccdacaab1be1ae3e33a0a002db690fc14bbaa504fnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='ccdacaab1be1ae3e33a0a002db690fc14bbaa504fn:2'>
          <p>
            I have only tested this standalone and as a WordPress plug-in. I did not change the codes related to other systems, nor the signatures of existing functions so it should work just like the original version. YMMV.
          </p>
          
          <p>
            <a href='#ccdacaab1be1ae3e33a0a002db690fc14bbaa504fnref:2' rev='footnote'>&#8617;</a></li> 
            
            <li id='ccdacaab1be1ae3e33a0a002db690fc14bbaa504fn:3'>
              <p>
                I the footnotes behavior is unit tested relatively well &#8211; if you interested in the tests just let me know and I will send them to you &#8211; but the rest of the behavior of the code is untested so there might be some non-obvious interactions problems.
              </p>
              
              <p>
                <a href='#ccdacaab1be1ae3e33a0a002db690fc14bbaa504fnref:3' rev='footnote'>&#8617;</a></li> </ol> </div>
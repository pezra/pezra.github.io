---
id: 257
title: Nicer phpDoc Comments
date: 2006-10-05T23:23:35-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2006/10/nicer-phpdoc-comments/
permalink: /blog/2006/10/nicer-phpdoc-comments/
tags:
  - Emacs
  - PHP
---
Some days it is the little things that make you happy. This is a story of how a little elisp<sup id='a495e87bcf68da6afbde3c39c40e65937bef3e9bfnref:1'><a href='#a495e87bcf68da6afbde3c39c40e65937bef3e9bfn:1' rel='footnote'>1</a></sup> made me happy today.

We use [phpDocumentor](http://www.phpdoc.org/) to extract the documentation from our PHP code. It is a workable solution but the format is a bit hard to read in plain text. The [examples](http://manual.phpdoc.org/HTMLSmartyConverter/HandS/phpDocumentor/tutorial_tags.param.pkg.html#example) on the phpDocumentor website are all syntax highlighted which makes them easy to read. Without the fancy syntax highlighting, however, it is quite difficult to detect where the meta-data ends and where the descriptions start. To see what I mean look at this example ([copied from the phpDocumentor tutorial](http://manual.phpdoc.org/HTMLSmartyConverter/HandS/phpDocumentor/tutorial_tags.param.pkg.html#example)):

    /**
     * Example of unlimited parameters.
     * Returns a formatted var_dump for debugging purposes
     * @param string $s string to display
     * @param mixed $v variable to display with var_dump()
     * @param mixed $v,... unlimited number of additional variables to display with var_dump()
     */
    function fancy_debug($s,$v)

That does, in fact, document the method but, man, is it hard to read in plain unformated text. Even when you gussy it up with some pretty colors it is not really all that readable. I think some separator characters that where printable would have been a really useful addition to the format, say like a &#8217;:&#8217; between the meta-data and the description. However, you can make that block a lot easier to read just by inserting some newlines, which `phpdoc` handles well.

    /**
     * Example of unlimited parameters.
     * Returns a formatted var_dump for debugging purposes
     *
     * @param string $s 
     *   string to display
     *
     * @param mixed $v 
     *   variable to display with var_dump()
     *
     * @param mixed $v,... 
     *   unlimited number of additional variables to display 
     *   with var_dump()
     */
    function fancy_debug($s,$v)

Much more readable. The problem is, I am an `fill-paragraph` addict. I type M-q about as often as I breath when I am writing text. `fill-paragraph` (M-q is the keyboard shortcut for `fill-paragraph`) is the Emacs function that will optimally layout a paragraph of text. It splits lines that are too long and merges lines that are too short, removes superfluous spaces, etc. The problem was that fill-paragraph thinks that the &#8221;`@param ...`&#8221; bit is part of the same paragraph as the description so it (un)helpfully merges those two lines leaving the same jumble of text we started out with.

This has been bugging me for weeks, and today I fixed it. See, `fill-paragraph` detects the boundaries between paragraphs by using a couple regular expressions that you can configure. So I buckled down and created a paragraph boundary pattern that make Emacs correctly treat `@param`, or any other phpDocumentor tag that appears at the beginning of the line, as a paragraph separator line. Now `fill-paragraph` leaves the description on line following the meta-data where it belongs.

Here is the code<sup id='a495e87bcf68da6afbde3c39c40e65937bef3e9bfnref:2'><a href='#a495e87bcf68da6afbde3c39c40e65937bef3e9bfn:2' rel='footnote'>2</a></sup>. You can just paste this into your `.emacs` file any place after `(require &#39;php-mode)` line. Now that I think about it, this same regexp pattern should work for Javadoc, and probably a lot of other document extraction tool formats. You would, of course, need to change the hook, though.

    (defun php-doc-paragraph-boundaries () 
      (setq paragraph-separate "^[ \t]*\\(\\(/[/\\*]+\\)\\|\\(\\*+/\\)\\|\\(\\*?\\)\\|\\(\\*?[ \t]*@[[:alpha:]]+\\([ \t]+.*\\)?\\)\\)[ \t]*$")
      (setq paragraph-start (symbol-value &#39;paragraph-separate)))
    
    (add-hook &#39;php-mode-user-hook &#39;php-doc-paragraph-boundaries)

Having M-q do the right thing in PHP documentation blocks makes me happy. Probably inordinately so. But like I said, sometimes it is the little things.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='a495e87bcf68da6afbde3c39c40e65937bef3e9bfn:1'>
      <p>
        elisp is the Lisp dialect in which GNU Emacs is written. It is also the language used to customize Emacs.
      </p>
      
      <p>
        <a href='#a495e87bcf68da6afbde3c39c40e65937bef3e9bfnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='a495e87bcf68da6afbde3c39c40e65937bef3e9bfn:2'>
          <p>
            I have to say that I despise the way Emacs handles regexen. The profusion of backslashes make regular expressions really hard to understand. And having some regexp operators require escaping and others not is really annoying. The only other issue I ran into is that the paragraph-start and paragraph-separator patterns must match the entire line. Just matching part of the line is not sufficient. The <a href='http://www.gnu.org/software/emacs/manual/html_node/Paragraphs.html#Paragraphs'>documentation on paragraphs in Emacs</a> does not explicitly state that.
          </p>
          
          <p>
            <a href='#a495e87bcf68da6afbde3c39c40e65937bef3e9bfnref:2' rev='footnote'>&#8617;</a></li> </ol> </div>
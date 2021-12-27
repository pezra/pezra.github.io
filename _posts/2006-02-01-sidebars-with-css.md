---
id: 205
title: Sidebars With CSS
date: 2006-02-01T17:39:43-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=205
permalink: /blog/2006/02/sidebars-with-css/
categories:
  - Software Development
tags:
  - Software
  - Web Applications
---
<p class='update'>
  Update: Due to a misconfiguration some of my blog entries from the month of Feburary recently lost. This is merely a repost of the original content.
</p>

Being  
relatively new to CSS I recently attempted to layout a page with a  
sidebar to the left of the main content using only CSS. I spent several  
hours trying to get this to work to my satisfaction before giving up.  
In the end I just added a two column table to encompass the sidebar and  
content which works well but makes me feel dirty. That table is  
obviously just about layout and therefore should not be in the markup.

While I was on that wild goose chase I complained to one of my friends and today he pointed me to an [article that describes one technique for column layouts](http://www.alistapart.com/articles/holygrail).  
The technique is fairly elegant but it, and all the other techniques I  
have seen, have one major flaw. Namely, it requires explicitly  
declaring the widths of the columns.

Any techniques that require explicitly declaring the width of a  
block level element that will contain text is fundamentally broken. I  
say this because it forces you to make a choice with no options. You  
can either trust the users configuration and accept that your page is  
going to look crappy (lines will get wrapped that do not really need to  
be wrapped) for any user that have changed the default values for their  
font and font size or you can explicitly set the font in the CSS, in  
which case I will curse you every single time I use your website.

These days it seems that web designers generally choose the second  
choice, ie explicitly setting the font in the CSS. This is the _worst_  
possible choice. Sure it makes your site look pretty but the world does  
not look at your site as if it were a picture. They actually try to  
read that text whose size is 4px and in a font that is particularly  
ugly on their system. By changing the font, or size, you are forcing  
your users into a sub-optimal environment. Not to mention the people  
with actual visual impairments.

Anyway, I guess I am just trying to say that I am disappointed with  
the current state of CSS. All these kludges that get created just to  
account for the fact that CSS is missing the ability to change the flow  
direction. Normally, flow for block level elements is top-to-bottom and  
that is appropriate most of the time but fairly often you really want  
the flow of block level elements to be left-to-right. If we had that  
single feature this whole sidebar/column issue would go away.
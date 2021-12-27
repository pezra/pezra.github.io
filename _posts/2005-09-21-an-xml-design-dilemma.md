---
id: 163
title: An XML Design Dilemma
date: 2005-09-21T13:51:30-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=163
permalink: /blog/2005/09/an-xml-design-dilemma/
tags:
  - Design
  - Software
---
Recently the following XML was proposed during a design session of which I was part. This document basically says that something has happened that cannot be automatically handled. A program receives this XML and asks a human which of the available actions to take.

<pre class='code'>&lt;event&gt;
    &lt;identifier&gt;1234&lt;/identifier&gt;
    &lt;type&gt;ForkReached&lt;/type&gt;
    &lt;description&gt;
      There is a fork in this path.  Which way do you want to go?
    &lt;/description&gt;
    &lt;action name="TakeMoreTraveledPath"&gt;
      ...
    &lt;/action&gt;
    &lt;action name="TakeLessTraveledPath"&gt;
      ...
    &lt;/action&gt;
    &lt;action name="ReturnHome"&gt;
      ...
    &lt;/action&gt;
  &lt;/event&gt;
</pre>

I responded by proposing encapsulating the action elements in a availableActions element like this.

<pre class='code'>&lt;event&gt;
    &lt;identifier&gt;1234&lt;/identifier&gt;
    &lt;type&gt;ForkReached&lt;/type&gt;
    &lt;description&gt;
      There is a fork in this path.  Which way do you want to go?
    &lt;/description&gt;
    <span class='notable'>&lt;availableActions&gt;</span>
      &lt;action name="TakeMoreTraveledPath"&gt;
        ...
      &lt;/action&gt;
      &lt;action name="TakeLessTraveledPath"&gt;
        ...
      &lt;/action&gt;
      &lt;action name="ReturnHome"&gt;
        ...
      &lt;/action&gt;
    <span class='notable'>&lt;/availableActions&gt;</span>
  &lt;/event&gt;
</pre>

To me the second is obviously better. However, I found that I was unable to come up with a single concrete reason why the second form is better than the first. Both are easy fairly to understand. Iterating over all of the actions is not hard (the same number lines of code) in either case. I can think of no functional difference between the two. So why, then, does the first form make me feel dirty?

While pondering this, I found this in a post on an unrelated topic.

<blockquote cite='http://codebetter.com/blogs/jeremy.miller/archive/2005/09/20/132290.aspx'>
  <p>
    I think thereâ€™s a watershed moment in every developerâ€™s career when they start look more at their code as a structure and less as a bag of statements. &#8212; <a href='http://codebetter.com/blogs/jeremy.miller/archive/2005/09/20/132290.aspx'>Jeremy Miller</a>
  </p>
</blockquote>

I agree with Jeremy that there is a lot to be gained by focusing less on the the details and more on the structure. ([The smalltalk people have know that for years, apparently.](http://www.notarianni.org/index.php/2005/09/17/object_orientation_enlightenment)) Since I was still pondering my XML dilemma I immediately thought, &#8220;I like the availableActions element because it results in a document with a better structure.&#8221; But I did not manage to completely convinced myself. We usually think that one structure is better than another for some fairly concrete reason, like it will be more reliable or extensible. But in this case I cannot put my finger on why one of the options feels so much better than the other.

I wonder if this is common for developers to feel that one design is distinctly better than another without being able to articulate, in concrete terms, why. Does that happen to you often? If so how do you deal with it, especially in an XP environment where the design that feels better may, at least arguably, be a little bit less simple?

In this particular case am I wrong that the second flavor feels better than the first flavor? If I am correct about the second flavor feeling better is there some concrete reason why that I am overlooking?
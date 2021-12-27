---
id: 301
title: The power of hypermedia remains non-obvious
date: 2007-10-12T11:22:08-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/10/the-power-of-hypermedia-remains-non-obvious/
permalink: /blog/2007/10/the-power-of-hypermedia-remains-non-obvious/
tags:
  - REST
  - Web Applications
---
[Patrick Mueller contemplates whether or not we really need URIs in our documents](http://www-128.ibm.com/developerworks/blogs/page/pmuellr?entry=on_links)<sup id='58798edfd6bd020493466eff1e90cd28c39b0b8cfnref:1'><a href='#58798edfd6bd020493466eff1e90cd28c39b0b8cfn:1' rel='footnote'>1</a></sup>. This is a pretty common question in my experience. This question comes up because it is not always immediately obvious just how powerful embedding links in documents is.

What Mr. Mueller suggests is that if you have a client that needs account information for a particular person that is could simply take the account numbers found in the person representations and based on some out of band information, construct the account URIs. For example, if you got a person representation that looked like

    <person>
      <accounts>
        <account><id>3242</id></account>
        <account><id>5523</id></account>
      </accounts>
    </person>

The client could then make get requests to `http://bank.example/accounts/3242` and `http://bank.example/accounts/5523` to get the persons account information. The client would have constructed those URIs based on some configuration or compile time information about the structure of account URIs. This is a very common approach. Hell, it is even the one use by the [ActiveResource library in Rails](http://ryandaigle.com/articles/2006/06/30/whats-new-in-edge-rails-activeresource-is-here). But common does make it good.

Magically creating URIs out of the ether would work at first but say this bank we work for buys another bank. There are some people that have accounts at both banks. Now, if a persons accounts where referenced by URI, rather than just number, you could just add them to the list like this:

    <person>
      <accounts>
        <account href="http://bank.example/accounts/3242"/>
        <account href="http://bank.example/accounts/5523"/>
        <account href="http://other-bank.example/accounts/9823"/>
      </accounts>
    </person>

The fact that some accounts are served by the original system and some are served by the other banks system is unimportant. However, if the client is constructing URI based on out of band information this approach fails completely. This is just one example of the sort of problems that disappear when you reference resources by URI, rather than some disembodied id.

One of the potential advantages of using just ids, rather than a URI is that it will require less work on the server to generate the document. I suppose ids are less costly, in a strict sense, if the server generating the document is also serves the account resources. But how much faster? Building a URI like the ones above could be as cheap as a single string concatenation. As far I am concerned, that is not really enough work to spend any time worrying about. On the other hand, if the server generating the document does not also serve the account resources, then the accounts should be being referenced by URI internally anyway so using the URI should be cheaper (not to mention safer).

Mr Mueller suggests, as a proof point, that Google Maps must work by URI construction based on a priori knowledge of the shape of tile URIs. It may well, for all I know, but it certainly would not have to. For example, the server could pass the client a tile [URI template](http://www.ietf.org/internet-drafts/draft-gregorio-uritemplate-01.txt) and the client could then calculate the x and y offsets of the required tiles based on the x and y values of the tiles it already has. Or each tile could include links to the tiles that touch it (which would allow arbitrary partitioning of the tiles which would be nice). No doubt there are other reasonable RESTful choices too.

[The more I work with REST based architectures the more enamored of hypermedia](http://pezra.barelyenough.org/blog/2007/05/hypermedia-as-the-engine-of-application-state/). Links make your representations brightly lit, well connected spaces and that will benefit you application in ways you probably have not even imagined yet.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='58798edfd6bd020493466eff1e90cd28c39b0b8cfn:1'>
      <p>
        BTW, Mr Mueller, I was unable to post a comment on your blog. After pressing <code>Post</code> I was brought back to the comments page with the message <code>Comment authentication failed!</code> after the <code>Comments</code> text area.
      </p>
      
      <p>
        <a href='#58798edfd6bd020493466eff1e90cd28c39b0b8cfnref:1' rev='footnote'>&#8617;</a></li> </ol> </div>
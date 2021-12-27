---
id: 279
title: Hypermedia as the Engine of Application State
date: 2007-05-23T12:18:30-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/05/hypermedia-as-the-engine-of-application-state/
permalink: /blog/2007/05/hypermedia-as-the-engine-of-application-state/
tags:
  - REST
  - Software
  - Web Applications
---
One of the least well understood core tenets of the REST architectural style is that &#8220;hypermedia is the engine of application state&#8221;. Which basically means that responses from the server will be documents that include URIs to everything you can do next. For example, if GET a blog post the response document will have URIs embedded in it that allow you to create a comment, edit the post and any other action that you might want to do. Basically, this allows you to think of your application as [a state machine with every page representing a state and links representing every possible transition from the current state](http://pluralsight.com/blogs/tewald/archive/2007/04/26/46984.aspx).

This approach means that to correctly be access your application the only things a client needs to know is _a)_ a well know starting point URI and _b)_ how to parse one of the document formats (representations) your application supports. For human facing web applications this approach is the one that is always used. Browsers understand how to parse HTML and extract the links to the next action and the user provides starting point URIs. This has become so ingrained in the industry&#8217;s culture that most people never really even think about it in these explicit terms.

However, I am now working with a system in which there are many independent automated processes which interact with each other. It was not immediately obvious to me, or my colleagues, that we should be following the same pattern even when there are no humans involved. After all, a program can easily remember how to construct a URI from a couple of pieces of information that it knows. In fact, URI construction is almost always easier to implement than the REST approach of requesting a well known resource, parsing the returned representation and extract the URI you want.<sup id='0e4f5b35ac7c6492e4b249dc1aae5e0de0e053d2fnref:1'><a href='#0e4f5b35ac7c6492e4b249dc1aae5e0de0e053d2fn:1' rel='footnote'>1</a></sup>

After pondering this for a while I did reach the, rather unsurprising, conclusion that [Roy Fielding is correct](http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm) and that hypermedia should be the core of our distributed application. My epiphany came when I decided that I really wanted to change the shape of a URI. I realized that if the system were only slightly bigger (and it will get there soon) there would be the strong probability that I would not know all the places that accessed the resources whose URIs I wanted to change. Therefore, I would not be able to change the shape of those URIs.

A URI that is constructed by a client constitutes a permanent, potentially huge, commitment by the server. Any resource that may be addressed by the constructed URIs must forever live on that particular server (or set of servers) and the URI patterns must be supported forever.<sup id='0e4f5b35ac7c6492e4b249dc1aae5e0de0e053d2fnref:2'><a href='#0e4f5b35ac7c6492e4b249dc1aae5e0de0e053d2fn:2' rel='footnote'>2</a></sup> Effectively, you are trading a small one time development cost on the client side for an ongoing, and ever increasing, maintenance cost on the server side. When it is stated like that it becomes obvious that URI construction introduces an almost absurd level of coupling between the client and server.

With a truly REST based architecture you are free to change just about anything about the disposition and naming of resources in the system, except for a few well known start point URIs. You can change the URI shapes (for example, if you decide that you really hate the currently scheme). You can relocate resources to different servers (for example, if you need to partition you data). Best of all, you can do those things without asking any ones permission, because clients will not even notice the difference.

Using hypermedia as the engine of application state has the effect of preserving the reversible for a huge number of decisions that are irreversible in most other architectural styles. As the section 9 of &#8220;The Pragmatic Programmer&#8221; points out, reversibility is one of the goals of any design should strive for. No one knows for sure what will be needed in the future, so having the ability to easily change your mind is invaluable. Particularly when it can be had so cheaply.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='0e4f5b35ac7c6492e4b249dc1aae5e0de0e053d2fn:1'>
      <p>
        It can get even more off-putting. Just imagine a situation where you would need to request several intermediate resources before you get to the URI of the resource for which you are looking.
      </p>
      
      <p>
        <a href='#0e4f5b35ac7c6492e4b249dc1aae5e0de0e053d2fnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='0e4f5b35ac7c6492e4b249dc1aae5e0de0e053d2fn:2'>
          <p>
            You could setup redirects/rewrites or proxy the requests if you really needed to move the resource but for high volume URIs having to redirect or proxy would probably eat a significant part the benefits you would get from moving the resource. Worse, though, is the fact that those remedies increase the maintenance requirements significantly because then you have to administrate both the real resources and the redirection/rewriting or proxying rules.
          </p>
          
          <p>
            <a href='#0e4f5b35ac7c6492e4b249dc1aae5e0de0e053d2fnref:2' rev='footnote'>&#8617;</a></li> </ol> </div>
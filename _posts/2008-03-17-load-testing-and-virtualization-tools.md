---
id: 317
title: Load Testing and Virtualization Tools
date: 2008-03-17T21:29:30-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2008/03/load-testing-and-virtualization-tools/
permalink: /blog/2008/03/load-testing-and-virtualization-tools/
categories:
  - Software Development
---
I really enjoy finding and using good tools. There are a couple of tools I have been using lately that give me that warm fuzzy feeling in spades, so I thought I would share.

## curl-loader {#curlloader}

The first one is [curl-loader](http://curl-loader.sourceforge.net/). This is a really nice tool for load testing web applications. It is based on [cURL](http://curl.haxx.se/)<sup id='1cc7b74004373b834abcbd2a1946d234af539a97fnref:1'><a href='#1cc7b74004373b834abcbd2a1946d234af539a97fn:1' rel='footnote'>1</a></sup>, which [as we all know, is one the best ways to test web applications](http://www.tbray.org/ongoing/When/200x/2005/09/21/Atom-Protocol). Being based on libcurl means that you can load test applications that require HTTP authentication<sup id='1cc7b74004373b834abcbd2a1946d234af539a97fnref:2'><a href='#1cc7b74004373b834abcbd2a1946d234af539a97fn:2' rel='footnote'>2</a></sup>, redirection following, SSL or pratically any other of the myriad of features supported by cURL.

The only issue I have run into is that test scripts must be explicit about the exact URIs to operate against. This is not really surprising, but if you have multiple setups all with different host names it can get a little tedious modifying the scripts for each environment. Fortunately, the script a simple text format that is amiable to being generated. I knocked to together a couple of Ruby scripts to generate various scripts for our application in just a couple of hours &#8211; and that included learning the curl-loaders script format.

## VirtualBox {#virtualbox}

The other tool have been having some success with is [VirtualBox](http://www.virtualbox.org/). VirtualBox is a open source virtual machine. It is available via `apt-get` in Ubuntu and works really well<sup id='1cc7b74004373b834abcbd2a1946d234af539a97fnref:3'><a href='#1cc7b74004373b834abcbd2a1946d234af539a97fn:3' rel='footnote'>3</a></sup>. I mostly use virtualization to verify that our software really does function correctly when there are several servers in a cluster. You cannot really extract much information about performance or scalability from a completely virtualized environment (at least not when all the pieces are running on my laptop) but you can tell if it is going to work or not. VirtualBox seems quite fast and it is dead simple to setup new virtual machine instances, all of which make it a joy to work with.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='1cc7b74004373b834abcbd2a1946d234af539a97fn:1'>
      <p>
        To be precise it uses libcurl, which is the functionality of the <code>curl</code> command made available as a native library.
      </p>
      
      <p>
        <a href='#1cc7b74004373b834abcbd2a1946d234af539a97fnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='1cc7b74004373b834abcbd2a1946d234af539a97fn:2'>
          <p>
            The ability to handle HTTP authentication is surprisingly uncommon in load testing frameworks.
          </p>
          
          <p>
            <a href='#1cc7b74004373b834abcbd2a1946d234af539a97fnref:2' rev='footnote'>&#8617;</a></li> 
            
            <li id='1cc7b74004373b834abcbd2a1946d234af539a97fn:3'>
              <p>
                The only problem I have had so far is that I put my machine to sleep when VirtualBox had control of the keyboard and when I resumed the keyboard was completely non-functional.
              </p>
              
              <p>
                <a href='#1cc7b74004373b834abcbd2a1946d234af539a97fnref:3' rev='footnote'>&#8617;</a></li> </ol> </div>
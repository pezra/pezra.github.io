---
id: 309
title: 'OpenID 2.0&#8217;s Killer Feature'
date: 2007-12-18T13:31:42-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/12/openid-20s-killer-feature/
permalink: /blog/2007/12/openid-20s-killer-feature/
openid_comments:
  - 'a:1:{i:0;s:5:"40275";}'
categories:
  - Software Development
tags:
  - identity
  - openid
  - single-sign-on
---
The [OpenID 2.0](http://openid.net/specs/openid-authentication-2_0.html) spec has been finalized. On the surface, it does not seem to be very different from the 1.1 spec but it does include at least one sweet new feature. It provides protocol support for directed identity.

Directed identity is the concept of having a single identity that appears to be a different identity for every relying party (ie, an application that wants to verify your identity). The identity provider would, of course, understand that all these single use identities are really all part of the same identity. This would, theoretically, prevent unscrupulous people from building a profile about all the things you do online, because each website you visited would know you by a different identity. However, you could still log into all those websites using the same credentials since the identity provider would know that all those single use identities belong to you.

The change that was made to the OpenID protocol to support directed identity is brilliantly simple. It amounts just defining that if a provider receives a normal authentication request with a predefined URI<sup id='148efbf65c7856324fed515ed93b8df066c835b2fnref:1'><a href='#148efbf65c7856324fed515ed93b8df066c835b2fn:1' rel='footnote'>1</a></sup> as the identity that the request is a directed identity request. The provider would then verify the user&#8217;s credentials and respond with the appropriate identity for the relying party. The OpenID provider responses always include the identity being verified so it turned out to be a very minor change to support that in our provider.<sup id='148efbf65c7856324fed515ed93b8df066c835b2fnref:2'><a href='#148efbf65c7856324fed515ed93b8df066c835b2fn:2' rel='footnote'>2</a></sup>

Now, just to be clear, I don&#8217;t actually have any use for directed identity in the applications on which I work. However, the protocol mechanism that supports directed identity can also be used to implement multi-application single sign-on. And that is a killer feature for the stuff on which I work.

For maintainability, we have divided our application into five<sup id='148efbf65c7856324fed515ed93b8df066c835b2fnref:3'><a href='#148efbf65c7856324fed515ed93b8df066c835b2fn:3' rel='footnote'>3</a></sup> separate components. This works great as a way to keep the code simple, the architecture comprehensible and the system distributable. However, in the past, it left a bit to be desired with regards to the user experience because a user was forced to login five different times just to use the different sections of the application. Well, actually, we have been using OpenID for a while so the user did not actually have to login five time, but the did have to type in their user name five times. And that is not any better.

The directed identities support in OpenID 2.0 provides a solution to this repeated challenge problem. Each supplementary application discovers the trusted OpenID provider for the system and the performs a direct identity authentication request against that provider. The provider figures out who the user is and conveys that information to the supplementary application in the `id_res` response.

This means that once you have logged into any component of our system you will never the asked for your identity info again. We may make half a dozen requests to determine and verify your identity when you navigate to a component for the first time but all that work it is completely seamless from the users point of view. To the user it seems like just another page in the application.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='148efbf65c7856324fed515ed93b8df066c835b2fn:1'>
      <p>
        The predefined URI is <<code>http://specs.openid.net/auth/2.0/identifier_select</code>>.
      </p>
      
      <p>
        <a href='#148efbf65c7856324fed515ed93b8df066c835b2fnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='148efbf65c7856324fed515ed93b8df066c835b2fn:2'>
          <p>
            It did take a bit more work in the consumers, but that is because <a href='http://openidenabled.com/ruby-openid/'>ruby-openid</a> does not support directed identity authentication yet. It does not like the fact that the identity in the <code>id_res</code> response does not match the identity of the initial authentication request. However, a bit of consulting the source lead me to a way of tricking it into accepting the responses even though they appear, on the surface, to be unrelated to the initial authentication requests.
          </p>
          
          <p>
            <a href='#148efbf65c7856324fed515ed93b8df066c835b2fnref:2' rev='footnote'>&#8617;</a></li> 
            
            <li id='148efbf65c7856324fed515ed93b8df066c835b2fn:3'>
              <p>
                That is five so far. All new functionality is implemented as a new supplementary application so this number is on an ever increasing trajectory.
              </p>
              
              <p>
                <a href='#148efbf65c7856324fed515ed93b8df066c835b2fnref:3' rev='footnote'>&#8617;</a></li> </ol> </div>
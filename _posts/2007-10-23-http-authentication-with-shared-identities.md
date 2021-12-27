---
id: 302
title: HTTP Authentication with shared identities
date: 2007-10-23T00:24:05-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/10/http-authentication-with-shared-identities/
permalink: /blog/2007/10/http-authentication-with-shared-identities/
categories:
  - Software Development
tags:
  - REST
  - Software
  - Web Applications
---
Authentication has been bane of my existence lately. By which I mean, it is complicated and interesting and I am loving every minute of it (but, as you can see, I am not going to let that stop me from complaining about it). However tonight I have run into an authentication problem that I am not sure how to solve. I am hoping someone out there can point me toward a solution.

So, Dear Lazyweb, here is my question: is there a mechanism available that allows an HTTP client to have a single identity for several applications and to be able to authenticate itself to each of those applications in such a way that even a malicious application would be unable to impersonate the actor to the other applications in the system?

Oh yeah, and it would be really nice if this were already implemented for [libcurl](http://curl.haxx.se/) and in [Ruby](http://www.ruby-lang.org).)

## Some background {#some_background}

I have a system composed of a set applications which communicate with one another using RESTful web services. This system supports the addition of arbitrary new applications to system. However,some of these applications maybe written by (relatively) untrusted parties.

All actors, both end user and components of the system, have a single system wide identity. This identity is managed by the one trusted component in the system. This component is responsible for, amongst other things, authentication of actors.

We settled on [OpenID](http://openid.net) as the mechanism for end user authentication. Other than having one of the worst specs I have ever read OpenID is really nice. OpenID solves this problem by forwarding the user&#8217;s browser to the identity provider (our trusted component) and the identity provider verifies the user&#8217;s identity claim. The application that requested the authentications is then notified of success of failure of the authentication process. This approach has the advantage that the user&#8217;s password, even in an encrypted form, never passes though the untrusted components of the system.

Unfortunately, end user authentication is only a subset of the authentication required for this system. There are many automated actors that also make have use of the resources exposed by components in the system. These actors need to the authenticated also, but OpenID is rather unsatisfactory for this purpose. So another solution to delegated authentication is required.

My initial thought was to use MD5-sess based HTTP Digest auth. The spec explicitly mention that it could be used to implement authentication using a third party identity provider. Upon further study, however it only works if the application doing the authentication is trusted. This is because to verify the requester&#8217;s identity the application must have the hash of the users account, realm and password. With that bit of information it would quite easy for the application to impersonate the original requester. In my environment of limit trust that is unacceptable.

Another potential, if naive, option is to use HTTP digest auth but to pass the authentication credentials though to the identity provider. The identity provider could then response with an indication of whether the requester proved that they new the password. Unfortunately, the additional load placed on the identity provider by having to verify the requester&#8217;s identity for every single request handled by any part o the system is just too great. Not to mention the additional lag this would impose on response times.

Now, the astute reader will by now be fairly yelling something about how this problem was solve by [Kerberos](http://web.mit.edu/Kerberos/) years ago. Not only is this true but theoretically, the [negotiate HTTP auth scheme](http://rfc.net/rfc4559.html) supports Kerberos based authenitication. However, I have yet to find any Ruby libraries that support that scheme. Tomorrow, I will probably dive into the RFC to determine if I can implement support myself. If you know of a library that implements this scheme please let me know.

I have also looked at [OpenID HTTP authentication](http://wiki.openid.net/OpenID_HTTP_Authentication). It looks a bit simpler than the Kerberos based negotiate auth scheme, but it seems a bit under cooked for a production system. On the other hand, it does have potential. If there are no other options it might be workable. It would be pretty easy to implement on the Ruby side of the house, particularly since I have spent the last couple of days coming to terms with OpenID, but on the C++ side it might be a bit more of a problem.

Anyway, it is late now so I am going to go to sleep and await your answers.
---
id: 328
title: mod_proxy_balancer Is A Two Timing Hussy
date: 2008-04-28T08:00:33-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=328
permalink: /blog/2008/04/mod_proxy_balancer-is-a-two-timing-hussy/
categories:
  - Software Development
---
The canonical way to deploy a [Rails](http://rubyonrails.org) application is using [Apache](http://httpd.apache.org/) and [mod\_proxy\_balancer](http://httpd.apache.org/docs/2.2/mod/mod_proxy_balancer.html) to act as a reverse proxy request to a cluster of [Mongrel](http://mongrel.rubyforge.org/) processes running your application. It is easy to setup, debug and monitor. As it turns out the only problem with this setup is mod\_proxy\_balancer.

To see what I mean lets start with [what](http://blog.codahale.com/2006/06/19/time-for-a-grown-up-server-rails-mongrel-apache-capistrano-and-you/) [Google](http://www.webmasterwords.com/ruby-rails-mongrel-apache-easy) [suggests](http://webonrails.com/2007/02/04/apache-proxy-balancer-mongrel-clusters-and-deploying-application-with-capistrano/). The reverse proxying gets setup something like this:

    <Proxy balancer://my-application>
      BalancerMember http://localhost:8000
      BalancerMember http://localhost:8001
      BalancerMember http://localhost:8002
    </Proxy>
    
    ProxyPass /my-application balancer://my-application

That works. Particularly if the load is not very high and all the requests to the application take about the same amount of work. If, however, the responses time for the application vary much you will start seeing some odd artifacts in the response times. Specifically, you will start seeing requests that should be fast taking a long time to complete.

This is because in mod\_proxy\_balancer seems to apply a rather simple minded round robin dispatch algorithm. This algorithm means that a single mongrel can end up with one or more low cost requests queued up behind a very expensive request. Even when there are other backend processes that are doing nothing at all. Because Rails is single threaded it means that each request that is sent to a particular mongrel process has to wait until all previous requests to that process are completely finish before work can begin on it. Those small requests end up taking a very long time.

Imagine a series of requests arriving, one per second. The first request is expensive, taking five seconds to complete, each subsequent request takes one second to complete.

![](http://pezra.barelyenough.org/blog/wp-content/uploads/2008/04/rrlb-stacking-pathology.png) 

From that you can see that the final request in our scenario will take 3 seconds to complete even though it is only one second of work. Worse yet there are two idle backends that could have complete it in one second had it been dispatched optimally.

## Maybe Monogamy {#maybe_monogamy}

A quick scan through the Apache docs reveals the [`max` parameter for ProxyPass](http://httpd.apache.org/docs/2.2/mod/mod_proxy.html#ProxyPass). Sweet, a nice simple solution. All that is needed is to tweak the Apache configuration to look like this:

    <Proxy balancer://my-application>
      BalancerMember http://localhost:8000 max=1
      BalancerMember http://localhost:8001 max=1
      BalancerMember http://localhost:8002 max=1
    </Proxy>
    
    ProxyPass /my-application balancer://my-application

That says that Apache mod\_proxy\_balancer should make no more than 1 connection to each Rails backend at any given time. If a request comes in and all connections are busy that request will be queued in Apache waiting for the next available backend. Which is _exactly_ what we want. Any Rails process handling a long request will not have further requests dispatched to it until it has finished what it is currently working on.

(It is worth noting that this is not completely optimal resource usage &#8211; the backends go idle for a moment between each request &#8211; but from a responsiveness point of view it far better than the alternative.)

## Broken Trust {#broken_trust}

Unfortunately, upon deploying the configuration above into a high load environment it will rapidly become obvious that it does not solve the problem. Nominally short requests continue taking really excessive amounts of time. Running `netstat` in such an environment will yield something like the following.

    Proto Recv-Q Send-Q Local Address       Foreign Address         State
    ...      
    tcp      741      0 127.0.0.1:8000      127.0.0.1:62322         ESTABLISHED 
    tcp      741      0 127.0.0.1:8000      127.0.0.1:53214         ESTABLISHED 
    tcp        0      0 127.0.0.1:8000      127.0.0.1:61024         ESTABLISHED 
    ...

There you can see that there are still multiple connections being made to the same Rails backend.

Apache&#8217;s mod\_proxy\_balancer seems to have a race condition that allows it to establish more than the configured `max` number of connections to a single backend in high load conditions. I suspect it goes something like this, multiple requests arrive at about the same time they are each dispatched to their own Apache worker. The multiple Apache workers then each look at their shared data and see that the next available backend is the same one. Then each of those workers, simultaneously, create a connection to the same backend. This means that in low load situations everything is fine because you are unlikely to have multiple requests to Apache arrive at the exact same time. In high load situations, however, when multiple requests are practically guaranteed to arrive simultaneously you _will_ end up with more than `max` connections to individual backends. (BTW, I have seen way more that three simultaneous connection to single backend with `max=1`.)

## Rebound {#rebound}

So now you know that you cannot trust Apache&#8217;s mod\_proxy\_balancer. You can use [Pen](http://siag.nu/pen/). It is easy to configure, fast and it works great. Oh, except for the fact that it does not queue requests if all the backends are busy. You could handle that by setting a `max` connections on the proxy in Apache, but we already know that we cannot trust mod\_proxy\_balancer.

It seems that [HAProxy](http://haproxy.1wt.eu/) might be the best choice. I&#8217;ll let you know how it works out.
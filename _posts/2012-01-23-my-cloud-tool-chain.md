---
id: 623
title: 'My &#8220;cloud&#8221; tool chain'
date: 2012-01-23T08:45:56-07:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=623
permalink: /blog/2012/01/my-cloud-tool-chain/
categories:
  - Software Development
---
Recently Mike Amundsen posted a [list of the tools](http://www.amundsen.com/blog/archives/1116) he uses for developing cloud applications. He also asked for others to provide their lists. So here goes:<section> 

### [Emacs](http://www.gnu.org/software/emacs/)

The one true development environment that all others aspire to be like when they grow up.

Mike has been using [Cloud9](http://c9.io/) and really seems to like it. I have had my eye on the browser based dev environments for a while. Maybe it is time to to give them a go.</section> <section> 

### [Ruby on Rails](http://rubyonrails.org)

RoR is still the best way to build web applications ever created.

(I know node.js is getting a lot of buzz these days, but it optimizes for the wrong thing. It optimizes IO performance, but what is really important is developer productivity. Except for a few very niche situations runtime performance is way less important that getting stuff done.)</section> <section> 

### [CouchDB](http://couchdb.apache.org/)

For small to medium size data sets CouchDB is the best option for cloud based data storage. It is easy to use, there are good libraries for it, and you can setup it up to be very reliable even when running on unreliable virtual instances.

We have been using [Cloudant](http://cloudant.com) for a while now. It has been reliable, but it is a bit slow. However, that problem is easily overcome with the violent application of caching. You communicate with CouchDB over HTTP so it is very easy to setup caching.</section> <section> 

### [Git](http://git-scm.com/)

We are using Git without the [hub](http://github.com). It is _great_ tool even without all the fancy gui collaboration support. If you are using any SCM tool that is not a DVCS you are missing out. Go switch to Git (or [Mercurial](http://mercurial.selenic.com/)) right now.</section> <section> 

### [Chef](http://www.opscode.com/chef/) (Solo)

Working in the cloud really drives home the impermanence of all things. Machine disappear, or lock up, APIs become unresponsive, etc. Being able to build a replacement instances automatically, and quickly, is vital.</section> <section> 

### [Fog](http://fog.io/1.1.2/index.html)

Fog is an adapter, in ruby, for the various cloud providers which gives them a similar interface. This is invaluable if you plan on working with more than one cloud provider.</section> <section> 

### [Amazon EC2](http://aws.amazon.com/ec2/) and [Rackspace Servers](http://www.rackspace.com/cloud/cloud_hosting_products/servers/)

The product we are developing is an open platform-as-a-service so we use the IaaS offerings to provide the compute power we need.</section> <section> 

### Conclusion

My tool chain for developing in the cloud is pretty similar to the one i used for developing apps for dedicated hardware. The biggest change is definitely the change to a document store. Traditionally relational databases just don&#8217;t work in the dodgy environment of the cloud yet. I expect that will change over time, but for now the ease of replication with document stores make them compelling for clusters of unreliable virtual machines.</section>
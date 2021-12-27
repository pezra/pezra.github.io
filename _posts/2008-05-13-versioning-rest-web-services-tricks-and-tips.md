---
id: 336
title: Versioning REST Web Services (Tricks and Tips)
date: 2008-05-13T14:47:56-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=336
permalink: /blog/2008/05/versioning-rest-web-services-tricks-and-tips/
openid_comments:
  - 'a:1:{i:0;s:5:"85654";}'
categories:
  - Software Development
tags:
  - Rails
  - REST
  - rest-versioning
---
In my [previous post on this subject](http://pezra.barelyenough.org/blog/2008/05/versioning-rest-web-services/) I described an approach to versioning the API of a REST/HTTP web service. This approach has significant advantages over the approach that is currently most common (i.e. embedding a version token in the URL). However, it does have some downsides. This post is an attempt to outline those and to present some ways to mitigate the negative impacts.

### Nonstandard MIME media types {#nonstandard_mime_media_types}

Using content negotiation to manage versions requires, by definition, the introduction of nonstandard media types. There is really no way around this. I personally don&#8217;t feel this is a Bad Thing. The new, nonstandard, media types do a much better job describing the sort of media the client is requesting. It does, however, mean that browsers &#8211; and perhaps some HTTP tools &#8211; will work less well with the web service.

The browser not working is a pretty big issue. They are almost certainly not the target consumer of the services, but having the browser not work raises the level of effort for exploring the API. If you have created a cool new service you want as few barriers to entry as possible. Personally, I always use [curl](#curl) when I am exploring but I know several people who would prefer to use a browser.

Unfortunately, I don&#8217;t really have a great general solution for browsers. That being said, in many situations a much can be done to make life better. For example, if the resources in question do not have HTML representations you could serve the current preferred format with a generic content type that browsers can render &#8211; e.g. `text/plain` or `application/xml` &#8211; to browsers.

### Curl<a id='curl' />  {#curl_}

One advantage of having the version token directly in the URL is that it makes it _really_ easy to use [curl](http://curl.haxx.se/) against the service. By default curl makes requests with the Accept header field set to `*/*`. For a reasonably designed service this would result in a response in the current preferred format. If you want to change to Accept header you need to invoke curl like this

    curl --header &#39;Accept: application/vnd.foo.myformat-v1+xml&#39; http://api.example/hello-world

That is not too horrible, really. It is a bit much to type all the time, but I have curl rc files for all the formats I deal with on a daily basis. If your service is implemented in Rails there is an even easier way. With Rails you give each format you support a short name that may be used as an &#8220;extension&#8221; for URLs. For example, if we define the short name for `application/vnd.foo.myformat-v1+xml` to be `mf1` we can say this

    curl http://api.example/hello-world.mf1

That is equivalent, from the point of view of a Rails based service, to the previous example. I imagine similar functionality could be implemented in most web frameworks. This effectively puts you back to having the version embedded in the URL, which is convenient for debugging and exploration. (It is still unsuitable for production use, though, for all the same reasons as other approaches to embedding the version in the URL.)

### Nonobviousness {#nonobviousness}

Another potential downside of using content negotiated versioning is that the various versions my be less discoverable, compared to a version-in-the-URL approach. I am not entirely sure this is true &#8211; after all there is a version token in the media type &#8211; but if it is true it would be a Good Thing.

Do you really want people &#8220;discovering&#8221; a version of the API that was deprecated a year ago? I think it might be better, in either approach, to use version tokens that are not readily guessable. Obviously, previous versions of and API will be documented and remain accessible, but raising some barriers to entry on depreciated parts of a system seems appropriate to me.

### Unfamiliarity {#unfamiliarity}

This may be the biggest issue of all. People are just not very familiar, and therefore comfortable, with content negotiation. This in spite of the fact that it has been a fundamental part of HTTP since forever. I think this features obscurity is waning now, though, because it is such a powerful feature.

Two years ago [Rails got content negotiation support](http://www.loudthinking.com/arc/000572.html). (That link seems to be broken at the moment. You can see part of the post I am talking about by going [here](http://www.loudthinking.com/arc/2006_04.html) and searching for &#8220;The Accept header&#8221;.) As frameworks like Rails keep adding and improving their support for this powerful feature the community of developers will become more familiar and comfortable with it. What is needed now is more education in the community on how best to utilize this feature.

* * *

#### Related Posts {#related_posts}

If you&#8217;re interested in REST/HTTP service versioning be sure not to miss the [rest of the series.](/blog/tag/rest-versioning/)
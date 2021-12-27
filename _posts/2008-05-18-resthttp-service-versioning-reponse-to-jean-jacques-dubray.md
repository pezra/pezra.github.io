---
id: 341
title: REST/HTTP Service Versioning (Response to Jean-Jacques Dubray)
date: 2008-05-18T23:54:39-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=341
permalink: /blog/2008/05/resthttp-service-versioning-reponse-to-jean-jacques-dubray/
aktt_notify_twitter:
  - 'yes'
categories:
  - Software Development
tags:
  - REST
  - rest-versioning
---
[Jean-Jacques Dubray takes issue](http://www.ebpml.org/blog/89.htm) with [my approach of using content negotiation to manage service versioning](http://pezra.barelyenough.org/blog/2008/05/versioning-rest-web-services/) in HTTP. I actually hesitate to respond to Mr. Dubray because the overall tone of his piece is rather off putting. On the other hand, he raises a couple of interesting questions which I have been really looking for and excuse to talk about. So I will give it a go.

### Handling obsolescent service providers {#handling_obsolescent_service_providers}

Mr. Dubray asks how we deal with version skew between the client and server.

> Backwards compatibility is when the consumer comes in with a &#8220;newer&#8221; request version than the service provider can provide. This is common when a consumer uses different providers for the same type of service. So ultimately, you need to provide some room to define the version of both the consumer and the version of the service provider that it is targeting. Your mechanism only supports &#8220;one version&#8221;.

Not true, the versioning mechanism I describe easily handles multiple versions. First, lets be clear, a service provider cannot provide capabilities that where not conceived of until after it was written. So Mr. Dubray must be interested in is the ability of a single consumer to successfully communicate with multiple versions of the service provider. I agree with him that this is an absolutely vital feature of any versioning mechanism.

Fortunately, content negotiation deals with this issue quite handily. I left this out of the original post for simplicities sake but it well worth talking about. HTTP allows user agents &#8211; or service consumers, if you prefer &#8211; to specify more than one acceptable response format. For example, the following is a perfectly legal HTTP conversation.

    ===>
    GET /accounts/42
    Accept: application/vnd.myapp-v2+xml, application/vnd.myapp-v1+xml;q=0.8
    
    <===
    200 OK
    Content-Type: application/vnd.myapp-v1+xml
    
    <account>
      <name>Inigo Montoya</name>
    </account>

The [Accept header field](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) in the request indicates that the consumer can operate using either version 1 or 2 of the API but it prefers version 2. Accept headers can include any number of MIME media types along with preference indicators (the q=number part). This allows consumers to inform the server of all acceptable dialects of the API with which it can work. In the example, the server obviously did not support version 2 of the API and therefore responded using version 1.

### Resource deprecation {#resource_deprecation}

Further along Mr. Dubray asks this question,

> Another flaw of your versioning strategy is that URIs are by default part of the versioning strategy. I have often pointed out that &#8220;Query-by-examples&#8221; are encoded by members of the REST community (MORCs) in a URI syntax, for instance:

>     /customer/{id}/PurchaseOrders ...

> Peter, how do you express that a particular QBE belongs to one version and not to the other?

I don&#8217;t. The set of purchase orders associated with a particular customer is not version specific. The customer has agreed to purchase the same things regardless of which version of the service you are talking to.

Perhaps the question Mr. Dubray is really trying to ask is, what happens if you want to deprecate such resource?

(One reason to do so might be that the purchase order collections become too big to reasonably render in a single response. There are other, better ways to solve that particular problem but it is a nice concrete use case for resource deprecation.)

Resource deprecations is easily handled in REST using media types to handle versioning. First some ground rules, user agents should _never_ be constructing such a URI. Doing so should be a gross violation of the HATEOAS constraint of REST. Rather they would be extracting that URI from the representation of the customer provided by the server. In such a case, an HTTP conversation getting the purchase orders for a customer might look like this.

    ===>
    GET /customer/42
    Accept: application/vnd.myapp-v1+xml
    <===
    200 OK
    Content-Type: application/vnd.myapp-v1+xml
    
    <customer>
      <purchase-orders href="http://service.example/customer/42/purchase-orders"/>
    </customer>
    
    
    ===>
    GET /customer/42/purchase-orders
    Accept: application/vnd.myapp-v1+xml
    <===
    200 OK    
    Content-Type: application/vnd.myapp-v1+xml
    
    <purchase-orders>
      ...
    </purchase-orders>

At version 2 of the API we deprecate the all-purchase-orders-for-customer resource &#8211; removing all references to it in the customer representations &#8211; and replace it with a purchases-order-by-month-by-customer resource. A similar HTTP conversation with a client capable of handling version 2 of the API would look like this.

    ===>
    GET /customer/42
    Accept: application/vnd.myapp-v2+xml
    <===
    200 OK
    Content-Type: application/vnd.myapp-v2+xml
    
    <customer>
      <purchase-orders-by-month href-template="http://service.example/customer/42/purchase-orders?in_month={xmlschema-gYearMonth}"/>
    </customer>
    
    
    ===>
    GET /customer/42/purchase-orders?in_month=2008-05
    Accept: application/vnd.myapp-v2+xml
    <===
    200 OK    
    Content-Type: application/vnd.myapp-v2+xml
    
    <purchase-orders>
      ...
    </purchase-orders>

Notice that in version 2 of the API the all-purchase-orders-for-customer resource is no longer exposed in any way. As a human you might guess that it still exists, and indeed it would need to in order to handle requests to version 1 of the API. However, a version 2 consumer will never make a request to that resource because it is not mentioned in the version 2 representations. Indeed, any requests for the all-purchase-orders-for-customer by a version 2 consumer would be met with a `406 Not Acceptable` response because it is not part of the version 2 API.

### Wrap up {#wrap_up}

Toward the end Mr. Dubray gets into full rant mode with these bits,

> You will soon start realizing that resources do have a state that is independent of the &#8220;application&#8221; since by definition a resource can participate in multiple &#8220;applications&#8221;. This is the essence of SOA, i.e. the essence of reuse.

Resources certainly may participate in multiple &#8220;applications&#8221;. There is nothing in the REST principles that prevent that. I don&#8217;t really claim to be an SOA expert. I just make systems work using REST principles. So far I have not found a problem reusing my resources in multiple applications. In fact, REST seems to excel at that very thing.

> At least, some of the MORCs <span>Member Of the REST Community</span> could have the courtesy to acknowledge that they are indeed building a programming model on top of REST, that this programming model needs clear semantics and that these semantics are not intrinsically part of REST (nor always RESTful).

I, for one, will readily acknowledge that we have built, and are continuing to build, programming models on top of REST. REST is merely a set of principles, articulated as constraints, that facilitate the creation of useful network based architectures. I would be very surprised if many in the REST community would disagree with me. These programming models do, for the most part, adhere to the REST principles.

Building REST/HTTP web services is certainly not fully understood yet. That does not make it special, hardly any sort of system design or architecture is fully understood. However, REST seems, to me at least, to be a better fit for today&#8217;s applications and technologies than any of the alternatives.

* * *

#### Related Posts {#related_posts}

If you&#8217;re interested in REST/HTTP service versioning be sure not to miss the [rest of the series.](/blog/tag/rest-versioning/)
---
id: 312
title: RESTful Service Discovery and Description
date: 2008-01-22T15:00:30-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2008/01/restful-service-discovery-and-description/
permalink: /blog/2008/01/restful-service-discovery-and-description/
categories:
  - Software Development
tags:
  - REST
  - web services
---
There has been a great deal of discussion [regarding](http://www.tbray.org/ongoing/When/200x/2008/01/18/Service-Doc-as-Interface-Def) [RESTful](http://www.25hoursaday.com/weblog/2008/01/17/MythRESTfulWebServicesDontNeedAnInterfaceDefinitionLanguage.aspx) [web](http://home.badc.rl.ac.uk/lawrence/blog/2008/01/22/whither_service_descriptions) [service](http://www.mnot.net/blog/2008/01/21/wadl_watching) [description](http://tomayko.com/weblog/2008/01/18/ws-vs-rest-sucks-nowadays) languages this week. The debate is great for the community but I think [Steve Vinoski has it basically right](http://steve.vinoski.net/blog/2008/01/16/idls-vs-human-documentation/)

> never once â€” not even once â€” have I seen anyone develop a consuming application without relying on some form of human-oriented documentation for the service being consumed

When you start writing an application that makes use of some services you are not writing some sort of generic web services consumer. You are writing a consumer of one very specific web service and the semantics of a service, as with everything else, turn out to be a lot more complicated, subtle and interesting than the syntax.

Human-oriented documentation necessary because only human can understand the really interesting parts of a service description. Based on my experience, it also seems to be sufficient. Sure we could all jump on the full fledged service description language band wagon but I don&#8217;t think that service consumers would get much, if any, value out of it.<sup id='6354b2fa5c2dba65b06b5acf4059bf582b9d3808fnref:1'><a href='#6354b2fa5c2dba65b06b5acf4059bf582b9d3808fn:1' rel='footnote'>1</a></sup>

## Discoverability {#discoverability}

Discoverability is the most important capability that interface definition languages bring to the table. However, most service description languages provide discoverability almost as a side effect, rather than it being their primary purpose.

I think it would be better to promote discoverability by working on a more focused capabilities publishing mechanism. To that end, I want to describe what my team has done on this front. It is not entirely suitable general use, but useful standards often emerge from extracting the common parts of many bespoke solutions.

First I want to be clear about the terminology I am using just to make sure we all understand one another

service
:   A cohesive set of resources that are exposed via HTTP.

resource
:   An single entity which exposes a RESTful interface via HTTP.

service provider
:   A process or set of processes that implement one or more services.

container
:   Another name for a service provider.

#### Background {#background}

We needed the ability to discover the actual URIs of resources at runtime from very early in our project because of our basic architecture. Our system is composed of at least four services<sup id='6354b2fa5c2dba65b06b5acf4059bf582b9d3808fnref:2'><a href='#6354b2fa5c2dba65b06b5acf4059bf582b9d3808fn:2' rel='footnote'>2</a></sup>. The containers that provide these services may be deploy in various (and arbitrary) ways. Maintaining the list of top level resources of other services in configuration files became unmanageable long before we every actually deployed the system in production.

We need a way that any component in the system could discover the URIs of resources exposed by other components in the system. We handled this by providing a system wide registry of all the services that are available and a description resource for each service that provides link to the resources contained with that service.

### Service Description {#service_description}

Containers that provide a service are responsible for exposing a &#8220;service description&#8221; for that service.

A service description is a resource that provides links to all the top level resources in service. Currently we support just one type of representation (format) for service description, a JSON format that looks like this

    {
      "_type":        "ServiceDescriptor",
      "service_type": "http://mydomain.example/services/something-interesting",
      "resources": [
        {
          "_type": "ResourceDescriptor",
          "name":  "OpenIdProvider",
          "href":  "http://core.ssbe.example/openid"
        },
        {
          "_type": "ResourceDescriptor",
          "name":  "AllAccounts",
          "href":  "http://core.ssbe.example/accounts"
        }
      ]
    }

`service_type` is the globally unique name for the type service that is being described. It should be a URI that is owned by the creator of the service. Each top level resource that is exposed as part of this service has a resource descriptor in the `resources` set.

If you wanted know about all the accounts of the system you would

1. `GET` the service descriptor resource 2. iterate over the resources collection until you found the `AllAccounts` resource descriptor 3. `GET` the URI found in the `href` pair of the resource descriptor (`http://core.ssbe.example/accounts` in this example)

One important thing to note is that each resource is really exactly one resource, and not a type of resource. If you are looking for a particular account you have to get the AllAccounts collection and find the account you are looking for in that set.

### Capabilities {#capabilities}

The Capabilities resource is the only well known entry point for our system. If a program wants to interact with our system it always starts with the capabilities service and the works it&#8217;s way down, using the links in documents, to the resource it actually cares about.

The JSON representation we support looks like

    {  
      "_type": "SystemCapabilities",
      "services": [
        {
          "_type":        "ServiceDescriptor",
          "href":         "http://alarm.ssbe.example/service_descriptors/escalations",
          "service_type": "http://mydomain.example/services/something-interesting"
        }
      ]
    }

To discover the URI of a particular top level resource a consumer must

1. `GET` the capabilities document 2. iterate though the objects in `services` until if it finds the one of the correct `service_type` 3. `GET` the full service descriptor using the URI in the `href` pair 4. iterate of the resources until it finds the one with the correct name 5. extract the URI from it&#8217;s `href` pair

Services are registered with the capabilities resource, by `POST`ing a service description to it, when the containers that provide those services are started.

### Issues {#issues}

#### No supported methods or format information {#no_supported_methods_or_format_information}

This approach only provides a way to discover the URIs of top level resources. It makes no attempt to describe the representations (formats) or methods those resources support. That sort of thing would not be hard add but so far I have had absolutely not need for it. That information is provided by the human-oriented documentation and since it does not change in each deployment there is no need for it included in the dynamic resource discovery mechanism.

#### Non-top level resources are not represented {#nontop_level_resources_are_not_represented}

Resources that are not top level &#8211; by which I mean resources that not listed in a service description document &#8211; are not represented at all. This is a feature, really, but it makes extending this format to include method and data format information less compelling becayse only a relatively minor subset of the resources in the system are surfaced in the service descriptions.

#### Encourages large representations {#encourages_large_representations}

The fact that only singleton resources are supported can lead to top level documents that are excessively large. In fact, we have already had to deal with this issue. We have basically punted on the issue but I think the correct approach would be to introduce a ResourceTypeDescriptor that would operate much like a ResourceDescriptor except that the link would be a [URI template](http://bitworking.org/projects/URI-Templates/) rather than a concrete URI.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='6354b2fa5c2dba65b06b5acf4059bf582b9d3808fn:1'>
      <p>
        On the other hand service providers might get some value. Something like WADL does give you a way to declaratively define a suite of regression tests. On the other hand, you might be better off using a tool specifically built for that purpose.
      </p>
      
      <p>
        <a href='#6354b2fa5c2dba65b06b5acf4059bf582b9d3808fnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='6354b2fa5c2dba65b06b5acf4059bf582b9d3808fn:2'>
          <p>
            That is the base number of services. Additionally functionality is added to the system in the form of additional services so the actually number of services varies based on what you need the system to do.
          </p>
          
          <p>
            <a href='#6354b2fa5c2dba65b06b5acf4059bf582b9d3808fnref:2' rev='footnote'>&#8617;</a></li> </ol> </div>
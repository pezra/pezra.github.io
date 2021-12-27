---
id: 1296
title: Services Questions
date: 2016-11-01T21:39:08-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=1296
permalink: /blog/2016/11/services-questions/
categories:
  - Software Development
tags:
  - REST
  - web services
---
I recently had a colleague ask me several questions about service oriented architectures and breaking monoliths apart. This is an area in which i have a good deal of experience so i decided to publish my answers here.

**What is a &#8220;service&#8221;?**

A &#8220;service&#8221; is a discrete bit of functionality exposed via well defined interface (usually a standardized format over a standardized network protocol) that can be utilized by clients that are unknown and/or unanticipated at the time of the service&#8217;s implementation. Due to the well defined interface, clients of a service do not need to understand how the service is implemented. This style of software architecture exists to overcome the [diseconomies of scale suffered by software software](http://allankelly.blogspot.com/2015/10/software-has-diseconomies-of-scale-not.html)

**How has the services landscape changed in the last 5-10 years?**

In the mid-2000s it became clear that WS-\*, the dominate service technology at the time, was dramatically over complicated and led to expensive to maintain systems. WS-\*&#8217;s protocol independence combined with the RPC style adopted by most practitioners meant that clients generally ended up being very tightly coupled with a particular service implementation.

As WS-*&#8217;s deficiencies became clear, REST style architectures gained popularity. They reduce complexity and coupling by utilizing an application protocol such as HTTP, and often using simpler message formats such as JSON. The application protocol provides a uniform interface for all services which reduces the coupling between client and service.

Microservices are a relatively recent variant of service oriented architectures. As the name suggest the main thrust of microservices is the size of the components that implement them. The services themselves could be message queue bases or REST style APIs. The rise of devops, automation around deploy and operations, raises the practicality of deploy a large number of very small components.

Message queue based architectures are experiencing a bit of a resurgence in recent years. Similar architectures where popular in the early 2000&#8217;s but where largely abandoned in favor of WS-*. Queue based architectures often provide throughput and latency advantages over REST style architecture at the expense of visibility and testability.

**What do modern production services architectures look like?**

It depends on the application&#8217;s goals. Systems that need high throughput, low latency and extreme scalability tend to be message queue based event driven architectures. Systems that are looking for ease of integration with external clients (such as those developed by third parties) tend to be resource oriented REST APIs with fixed URL structures with limited runtime discoverability. Systems that are seeking long term maintainability and evolvability tend to be hypermedia oriented REST APIs. Each style has certain strengths and weaknesses. It is important to pick the right one for the application.

**How granular is a service?**

I would distinguish a service from a component. Services should be small, encapsulating a single, discrete bit of functionality. If a client wants to perform an action, that action is an excellent candidate for being a service. A component, on the other hand, is a pile of code that implements one or more services. A component should be small enough that you could imagine re-writing it from scratch in a different technology. However, there is a certain fixed overhead for each component. Finding the balance between component size and the number of components is an important part of designing service architectures.

**What is the process to start breaking down a large existing application?**

Generally organizations start by extracting authentication and authorization. It is an area that is fairly well standardize ([oauth](https://oauth.net/), [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language), etc) and also necessary to support the development of other services. Once authentication and authorization are extracted from the monolith, another bit of functionality is chosen for implementation outside of the monolith. This process is repeated until the monolith is modularized enough to meet the organizational goals. The features implemented outside of the monolith are often new functionality but to really break apart a monolith you eventually have to target some of the existing features.

Starting small and preceding incrementally are the keys to success. Small successes make it easier to build consensus around the approach. Once consensus is reached existing, customer facing features can be extracted more readily.

**What are the organization / team structure impacts?**

It is generally superior to construct a (rough, high level) design of the services needed and to form vertically integrated teams to implement business features across the set of components. Empowering all teams to create new components and, where appropriate, new services, increases the likelihood of success and shortens the time to implement a service oriented architecture.

Alternatively, component structures can follow the organizational structure ([Conway&#8217;s law](https://en.wikipedia.org/wiki/Conway%27s_law)), resulting in one component per group in the organization. This approach can be practical in organizations where vertically integrated teams are politically unacceptable.

**What are needed / helpful tools? To run services? To manage services?**

  * Service discovery. Without it an inordinate amount of configuration is required for each component to interact with the ecosystem of services.
  * Instrumentation and monitoring. Without this it is impossible to detect and remediate issues that arise from the integration of an ecosystem of services.

**How do companies document their services, interfaces and versions?**

There are not any popular tools for creating documentation for hypermedia and message queue based APIs. However, [general guidance can be found](http://blog.parse.com/learn/engineering/designing-great-api-docs/) for creating documentation, but in general you are own your own.

For more resource oriented APIs tools like [swagger](http://swagger.io/) and [api blueprint](https://apiblueprint.org/) can be helpful.

**How can we speed developer ramp up in a service architecture?**

To speed developer ramp up it is important to have well maintained scripts to build a sandbox environment including all services. Preferably a single script that can deploy components to virtual machines or docker containers on the developers machine. Additionally it is important to maintain searchable documentation about the API so that developers can find information about existing services.

**How to deploy new versions of the ecosystem and its components?**

Envisioning an ecosystem of services as a single deployable unit is contrary to the goals of service oriented architectures. Rather each component should be versioned and deployed independently. This allows the components to change, organically, as needed. This approach increases costs on the marketing, product management and configuration management fronts. The benefits gained on the development side by avoiding the diseconomies of scale are worth it.

**How to manage API compatibility changes?**

A service oriented architecture makes breaking API changes more damaging. It is often difficult to know all the clients using a particular API. The more successful an API the harder, and more time consuming, it is to find and fix all clients. This difficultly leads to the conclusion that all changes should be made in a backwards compatible way. When breaking changes are unavoidable (which is rare) server-driven content negotiation can enable components to fulfill requests for both the old API and the new one. Once all clients are transitioned to the new API the old API can be removed. Analytics in the old API can help identify clients in need of transitions and to determine when it is no longer needed.

**How to version the ecosystem and its components?**

This is a marketing and project management problem more than anything. The ecosystem will not have a single version number. However, when a certain set of, business meaningful, features have been completed it is convenient for marketing to declare that a new &#8220;version&#8221;. Such declarations are of little consequence on the development side so they should be done whenever desirable.
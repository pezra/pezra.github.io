---
id: 1326
title: Web API best practices â€” forward compatibility patterns
date: 2017-06-06T06:00:33-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=1326
permalink: /blog/2017/06/web-api-best-practices-forward-compatibility-patterns/
categories:
  - Software Development
---
Designing APIs and clients for forward compatibility is the best way to achieve evolvability and maintainability in a microservice architecture. [Wikipedia defines forward compatibility as &#8220;a design characteristic that allows a system to accept input intended for a later version of itself.&#8221;](https://en.wikipedia.org/wiki/Forward_compatibility) Designing APIs and clients to be forward compatible allows each side of that relationship to change independently of the other. Forward compatibility is an exercise in reducing [connascence](https://en.wikipedia.org/wiki/Connascence_(computer_programming)). We want a system in which we can modify any component (service) without breaking the current behavior of the system. Without such independence any microservice architecture is doomed to collapse under its own maintenance cost.

Change is inevitable. Any successful architecture must accept, even welcome, change. Building avenues for adaptation into our designs allows our systems to survive and thrive in the face of change. This set of best practices has proven to provide superior forward and backward compatibility. As described here these practices are specific to HTTP and web APIs but the general ideas apply to any application protocol.

Forward compatibility is largely the responsibility of clients or document consumers. However, each forward compatible pattern is supported by backward compatibility in the servers or document producers. The following list of patterns describes the responsibilities of both parties. Compatibility will suffer if either side fails to uphold their end of the contract.

## Redirection {#redirection}

Clients _must_ follow [HTTP redirects](https://developer.mozilla.org/en-US/docs/Web/HTTP/Redirections).

URLs change over time. Companies rebrand their products, get acquired, change their name, etc. Developers relocate services to more optimal components. New features sometimes require changes to URLs. Redirection allows servers to change URLs without breaking existing clients by providing runtime discovery of the new URLs.

### Supporting backward compatibility {#supporting_backward_compatibility}

A URL, once given to a client, represents a contract. To honor these contract servers _must_ provide redirection of existing URLs when they change. Even in hypermedia APIs clients will bookmark URLs so Hypermedia is no excuse for breaking existing URLs.

## Must ignore semantics {#must_ignore_semantics}

Document consumers _must_ ignore any features of documents they don&#8217;t understand.

Document formats change over time. New features require new properties and links. New information illuminates our understanding of cardinalities and datatypes. Ignoring unrecognized document features allow producers to add new features without fear of breaking clients.

### Supporting backward compatibility {#supporting_backward_compatibility_2}

Document producers _must_ not remove, change the syntax or change the semantics of existing document features. Once a document feature has been released it is sacrosanct. Idiosyncrasy is better than carnage.

## Hypermedia APIs {#hypermedia_apis}

  * Clients _must_ discover available behavior at runtime.
  * Clients _must_ handle the absence of links gracefully.
  * Clients _must not_ construct URLs.

Available behavior changes over time. We discover new business rules. We realize that existing features are ill-conceived. [Hypermedia APIs](http://apievangelist.com/2014/01/07/what-is-a-hypermedia-api/) provide a mechanism for clients to discover supported behavior by embedding links to behavior in API responses.

### Supporting backward compatibility {#supporting_backward_compatibility_3}

The API _must_ be defined using a hypermedia format. The exact format does&#8217;t matter. ([HAL](http://stateless.co/hal_specification.html), [JSON API](http://jsonapi.org/), [Hydra](https://www.markus-lanthaler.com/hydra/), [HTML](https://developer.mozilla.org/en-US/docs/Web/HTML), etc are all good.) What does matter is that all behavior of the API _must_ be discoverable at runtime.

## Server content negotiation {#server_content_negotiation}

Clients _must_ inform the server of what document formats they can process via the [`Accept` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept).

Document formats have a half life. Sometimes the idiosyncrasies build to the point that a new document format is worth the effort. Sometimes you realize that a particular representation is too bulky or too terse. [Server driven content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation#Server-driven_content_negotiation) allows servers to introduce new, improved representations of existing resources while continuing to support existing clients.

### Supporting backward compatibility {#supporting_backward_compatibility_4}

Servers _must_ continue honoring requests for existing document formats. Document formats _may_ be phased out once they are no longer requested by any clients.

## Conclusion {#conclusion}

This set of best practices and patterns allows the construction of sophisticated distributed systems that can evolve and be maintained with reasonable effort. Without these patterns it is almost inevitable that a microservice architecture will end up in [dependency hell](https://en.wikipedia.org/wiki/Dependency_hell).
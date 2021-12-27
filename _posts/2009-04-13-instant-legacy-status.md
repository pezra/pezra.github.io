---
id: 387
title: Instant Legacy Status
date: 2009-04-13T09:13:10-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=387
permalink: /blog/2009/04/instant-legacy-status/
aktt_notify_twitter:
  - 'yes'
categories:
  - Software Development
---
<img src='http://barelyenough.org/blog/uploads/msgp-api.jpg' alt='MS Great Plain API diagram' style='float:right;' />

That is what you gain when you allow more than one module of software to access any single database. Any [integration database](http://martinfowler.com/bliki/IntegrationDatabase.html) confers legacy status on all modules that access it.

For the sake of this discussion we will define a module as unit of software that is highly cohesive, logically independent and implemented by a single team. A module is usually a single code base. Practically speaking, module boundaries usually depend on [Conway&#8217;s Law](http://en.wikipedia.org/wiki/Conway%27s_Law). However, disciplined teams can gain some advantage from sub-dividing their problem space into multiple modules.

## Pardon me while i digress&#8230; {#pardon_me_while_i_digress}

I recently attended [Mountain West RubyConf](http://mtnwestrubyconf.org/2009/). While there i had the pleasure of hearing a brilliant talk by [Jim Weirch](http://onestepback.org/) about [connascence in software](http://docs.rubyrake.org/articles/connascence/softwareconnascence.html). ([Here](http://urgetopunt.com/2009/03/27/sor-connascence.html) [are](http://deadprogrammersociety.blogspot.com/2009/04/larubyconf-2009-jim-weirich-grand.html) some other write ups of the same talk, but from different conferences.) A unit of software, A, is connascent with another, B, if there exists some change to B that would necessitate a change in A to preserve the overall correctness of the system.

Connascence has various flavors. Some of these forms cause more maintenance problems than other. Connascence of Name is linking code by name. For example, calling some method `foo`. Calling methods by name is a form of connascence but one which we regularly accept. Connascence of Algorithm, in contrast, is the linking of code by requiring both pieces use the same algorithm. When we see this in practice we generally run from the room screaming &#8221;[DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself)&#8221;.

Many of the practices we like in software &#8211; DRY, Law of Demeter, etc &#8211; reduce connascence. Likewise, many of the things we regard as [code smells](http://c2.com/xp/CodeSmell.html) result in increased connascence. Lower levels, and degree, of connascence are desirable because it reduces the effort required to change software. Mr Weirch posited that connascence might be a basis for a unified theory of software quality. It is definitely the most comprehensive framework i am aware of for thinking, and communicating, about the quality of so software.

## Back on point {#back_on_point}

It is clear that a database makes all the modules that access it connascent with one another. Many changes to an application will require changes to the database; any changes to the database will require changes to the other modules to maintain overall correctness. All changes to the database require that the correctness of all accessing modules be verified and such verification is often non-trivial.

The integration database is particularly problematic because it forces a high degree of a variety of forms of connascence. It causes Connascence of Name. All modules involved need to agree on the names of the columns, tables, database, schema and host. Right off the bat you should start to wonder about this approach. The modules are weakly connascent at many points.

If the data model is normalized it may well avoid introducing significant Connascence of Meaning. If, on the other hand, there are enumerable values without look-up tables; logically boolean fields stored as numeric types; or a wide variety of other common practices you will have introduced some Connascence of Meaning. Connascence of Meaning requires that the units agree on how to infer meaning from a datum (e.g., in this context 1 means true). It is a stronger form of connascence than Connascence of Name.

While we are on this point, remember that if your mad data modeling skills did manage to avoid significant Connascence of Meaning you did it by adding significant amounts of Connascence of Name. That is what a lookup-table is.

So far not so good, but that was the good part. The really disastrous part is that by integrating at the database level you are required to introduce staggering levels of Connascence of Algorithm. Connascence of Algorithm is very strong form a connascence. Allowing one module to interact with another&#8217;s data storage means that both modules have to understand (read: share the same algorithm) the business rules of the accessed module. If the business rules change in a module, the possibility exists that any other module that accesses the data might now operate incorrectly.

## Only [application databases](http://martinfowler.com/bliki/ApplicationDatabase.html) need apply {#only_application_databases_need_applyappdb}

I fall squarely on the side of not using databases as integration vectors. The forms and degree of connascence that such an approach necessitate make it recipe for disaster. Such high level of connascence will raise the costs of change (read: improvment) in all the modules involved. Application systems tend to get more integrated over time so the cost of improvement rises rapidly in systems that use integration databases.

After a while the cost of change will become so high that only improvements that provide huge value to the business will be worth the cost. For weak, undisciplined teams this will happen very rapidly. For strong, smart and highly disciplined teams it will take a bit longer, but it will happen. Once you allow more than one module to access your app&#8217;s database forever will it dominate your destiny.
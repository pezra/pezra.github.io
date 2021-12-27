---
id: 1575
title: 'Why i don&#8217;t love schemas'
date: 2018-09-20T21:05:35-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=1575
permalink: /blog/2018/09/why-i-dont-love-schemas/
categories:
  - Software Development
tags:
  - api design
---
Once, long ago, i thought that schemas were great and that they could provide much value. Schemas hold the promise of declaratively and unambiguously defining message formats. Many improvements would be â€œeasyâ€ once such a definition was available. Automated low-code validations, automagically created high quality, verifiably correct documentation, clearer understanding of inputs, etc. I tried fiercely to create good schemas, using ever better schema languages ([RelaxNG compact syntax](http://www.relaxng.org/compact-tutorial-20030326.html), anyone) and ever more â€œsophisticatedâ€ tools to capture the potential benefits schemas.

At every turn i found the returns on investment disappointing. Schema languages are hard for humans to write and even harder to interpret. They are usually unable to express many real world constraints (you can only have that radio with this trim package, you can only have that engine if you not in California, etc). High quality documentation is super hard even with a schema. Generation from schema solves the easy part of documentation, explaining why the reader should care is the hard part. Optimizing a non-dominate factor in a process doesnâ€™t actually move the needle all that much. Automated low-code validations often turned out hamper evolution far more than they caught real errors.

I wasnâ€™t alone in my disappointment. The whole industry noticed that XML, with its schemas and general complexity was holding us back. Schemas ossified API clients and servers to the point that evolving systems was challenging, bordering on impossible. Understanding the implications of all the code generated from schemas was unrealistic for mere mortals. Schema became so complex that it was impossible to generate human interpretable documentation from them. Instead people just passed around megs of XSD files and call it â€œdocumentationâ€.

JSON emerged primarily as a reaction to the complexity of XML. However, XML didnâ€™t start out complex, it accreted the complexity over time. Unsurprising the cycle is repeating. JSON schema is a thing and seems to be gaining popularity. It is probably a fools errand to try steming the tide but iâ€™m going to try anyway.

![doomed to watch everyone else repeat history](https://pbs.twimg.com/media/DdhLJQJWkAEf4Qr.jpg) 

What would it take for schemas to be net positive? Only a couple of really hard things.

## Humans first {#humans_first}

Schema languages should be human readable first, and computer readable second. Modern parser generators make purpose built languages easy to implement. A human centered language turns schemas from something that is only understandable by an anointed few into a powerful, empowering tool for the entire industry. RelaxNG concise syntax (linked above) is a good place to look for inspiration. The XML community never adopted a human readable syntax and it contributed to the ultimate failure of XML as a technology. Hopefully we in the JSON community can do better.

## Avoid the One True Schema cul de sac {#avoid_the_one_true_schema_cul_de_sac}

This one is more of a cultural and education issue than technical one. This principle hinges on two realizations

  1. A message is **only** valuable if at least one consumer can achieve some goal with that message.
  2. Consumers want to maximize the number goals achieved

However, consumers donâ€™t necessarily have the same goals as each other. In fact, any two consumers are likely to have highly divergent goals. Therefore, they are likely to have highly divergent needs in messages. A message with which one consumer can achieve a goal may be useless to another. Therefore, designing a schema to be used by more than consumer is an infinite-variable optimization problem. Your task is to minimize the difference between the set of valid messages and the set of actually processable messages for _every possible_ consumer! ([See appendix A for more details](#appendix-a)) A losing proposition if there ever was one.

To mitigate this schema languages should provide first class support for â€œpersonalizingâ€ existing schemas. A consumer should able to a) declare that it only cares about some subset of the properties defined in a producerâ€™s schema and b) that it will accept and use properties not declared in a producerâ€™s schema. This would allow consumers to finely tune their individual schemas to their specific needs. This would improve evolvability by reducing incidental coupling, increase the clarity of documentation by hiding all irrelevant parts of the producerâ€™s schema, and improve automated validation by ignoring irrelevant parts of messages.

We as a community should also educate people in the dangers of The One True Schema pattern.

## Conclusion {#conclusion}

Designing schemas for humans and avoiding the One True Schema are both really hard. And, unfortunately, i doubt our ability to reach consensus and execute on them. Given that i think most message producers and basically all message consumers are better avoiding schemas for anything other than documents.

* * *

### Appendix A: Where in i get sorta mathy about why consumers shouldnâ€™t share schemas unless they have the same goals {#appendix_a_where_in_i_get_sorta_mathy_about_why_consumers_shouldnt_share_schemas_unless_they_have_the_same_goals}

I donâ€™t know if this will help other people but it did help me clarify my thinking on this issue.

    M = set of all messages
    
    mC = {m | m âˆˆ M, m contains info needed by consumer C}

A particular consumer, C, needs messages to contain certain information to achieve its goal.

    mV = {m | m âˆˆ M, m is valid against schema S}

For any particular schema there is some subset of all messages that are valid.

    mC = lim mV as S->perfectSchemaFor(C)

As the schema approaches perfection for consumer C the set of valid messages approaches the set of messages actually processable by the consumer.

    mC â‰  mV
    mC âŠ„ mV
    mC âŠ… mV

In practice, however, there is always some mis-match between the set of valid messages and the set of messages actually processable by the consumer. Some technically invalid messages contain enough information for the consumer to achieve its goal. Some technically valid messages will contain insufficient information. The mis-match may be due to bugs, a lack of expressiveness in the schema language or just poor design. The job of a schema designer is to minimize the mis-match.

Now consider a second consumer of these messages.

    mD = {m | m âˆˆ M, m contains info needed by consumer D}

A particular consumer, D, needs messages to contain certain information to achieve its goal.

    mC â‰  mD (in general)

The information needed by consumer D will, in general, be different from the information needed by consumer C. Therefore, the set of messages processable by C will, in general, not equal the set of messages processable by D.

    perfectSchemaFor(C) â‰  perfectSchemaFor(D)

This is the kicker. The perfect schema for consumer C is, in general, different from the perfect schema any other consumer. Minimizing the difference between `mV` and `mC` will tend to increase the difference between `mV` and `mD`.
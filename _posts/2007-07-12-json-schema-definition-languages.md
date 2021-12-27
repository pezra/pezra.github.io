---
id: 282
title: JSON Schema Definition Languages
date: 2007-07-12T21:33:45-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/07/json-schema-definition-languages/
permalink: /blog/2007/07/json-schema-definition-languages/
openid_comments:
  - 'a:1:{i:0;s:5:"40228";}'
categories:
  - Software Development
tags:
  - REST
  - Software
  - Web Applications
---
We recently settled on using [JSON](http://json.org) as the preferred format for the REST-based distributed application on which I am working. We don&#8217;t need the expressiveness of XML and JSON is a lot cheaper to generate and parse, particularly in [Ruby](http://ruby-lang.org). Now we are busy defining dialects to encode the data we have, which is happy work. The only problem is there is not a widely accepted schema language for describing JSON documents.

I am not entirely sure a schema language for JSON is necessary in any strict sense. I think that validating documents against a schema is overrated. And, people do seem to be getting along just fine with examples and prose descriptions. Still, the formalist in me is screaming out for a concise way to describe the JSON documents we accept and emit.

I have a weakness for formal grammars. I often write [ABNF](http://en.wikipedia.org/wiki/ABNF) grammars describing inputs and outputs of my programs, even in situations where most people would just use a couple of examples. I learned XML schema very early in it&#8217;s life and I had a love/hate relationship with it for years. Even though it is ugly and complicated I continued to use it because it let me write formal descriptions of the XML variants I created.<sup id='1e113fbf89c0f8c35b702a278b975e173a300c93fnref:1'><a href='#1e113fbf89c0f8c35b702a278b975e173a300c93fn:1' rel='footnote'>1</a></sup>

There are a couple of relatively obscure schema languages for JSON. Unfortunately, I don&#8217;t find either of them satisfactory.

## Cerny {#cerny}

The [Cerny schema](http://www.cerny-online.com/cerny.js/documentation/schema/) validator seem quite functional and while it is not intended as a JSON document schema language, it could be used as one.<sup id='1e113fbf89c0f8c35b702a278b975e173a300c93fnref:2'><a href='#1e113fbf89c0f8c35b702a278b975e173a300c93fn:2' rel='footnote'>2</a></sup> Unfortunately, `CERNY.schema` requires a complete Javascript interpreter and run-time to perform validation. This requirement stems from the fact that `CERNY.schema` allows parts of the schema to be defined as procedural Javascript code. This approach is completely unacceptable for a language independent data transfer language like JSON.

Such procedural checking is misguided, even beyond the practical problems with requiring a Javascript run-time. Procedural validation code is a powerful technique for validating documents. However, this procedural code greatly reduces the usefulness of schema documents as an informational tool. Schema languages should be designed to communicate the structure of documents to humans, and only incidentally to validator programs.

## Kwalify {#kwalify}

Another JSON schema language I have tried is [Kwalify](http://www.kuwata-lab.com/kwalify/). Kwalify seems reasonably capable also but it has some warts that really bother me. My main issue with Kwalify is that it is super verbose. This is primarily due to the fact that Kwalify schema documents are written in [YAML](http://yaml.org/) (or JSON). Schema definitions can be encoded in a generic hierarchical data language, but it is not a very good idea. I find both XSD and the XML variant of RelaxNG to be excessively noise and Kwalify shows a similarly poor signal to noise ratio. Schema language designers should look to [RelaxNG&#8217;s compact syntax](http://relaxng.org/compact-20021121.html) for inspiration and forget trying to encode the schema in the language being described.

## Conclusion {#conclusion}

I think JSON could benefit from a schema language that is completely declarative and has a compact and readable syntax. If anyone is working on such a thing I would love to know about it. I could roll my own but I would really rather not unless it is absolutely necessary.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='1e113fbf89c0f8c35b702a278b975e173a300c93fn:1'>
      <p>
        Later I learned of <a href='http://relaxng.org'>RelaxNG</a>. Now am able to have my cake and eat it too. RelaxNG is a much simpler and elegant way to describe XML documents. And if you really need XML schema for some part of you tool chain you can mechanically convert the RelaxNG into XML schema.
      </p>
      
      <p>
        <a href='#1e113fbf89c0f8c35b702a278b975e173a300c93fnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='1e113fbf89c0f8c35b702a278b975e173a300c93fn:2'>
          <p>
            Updated to clarify that <code>CERNY.schema</code> is not intended as a JSON validator. I originally made the incorrect logical leap that it was. It is a easy step from &#8220;validator for JavaScript objects&#8221; to JSON validator because JSON documents are just serialized JavaScript object. However, the author of <code>CERNY.schema</code> informed me that JSON validation is not the intended use of <code>CERNY.schema</code>.
          </p>
          
          <p>
            <a href='#1e113fbf89c0f8c35b702a278b975e173a300c93fnref:2' rev='footnote'>&#8617;</a></li> </ol> </div>
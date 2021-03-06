---
id: 566
title: RDFa as interchange format
date: 2011-04-04T05:00:33-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=566
permalink: /blog/2011/04/rdfa-as-interchange-format/
aktt_notify_twitter:
  - 'yes'
aktt_tweeted:
  - "1"
categories:
  - Software Development
tags:
  - rdf
  - spdx
---
The tension between human and machine readability is never greater than when developing interchange formats. Formats that are easy and efficient for computers to read tend to be rather difficult for people to understand. When developing an interchange format you know that there will be few tools supporting it when it is released tools so it needs to be useful even with limited tooling. However, the format must support the development of sophisticated tools if it is to succeed in the long run.

A large part of the appeal of XML based languages is XML provides a reasonable balance between those two factors &#8211; for programmers. It can be read easily by computers and understood reasonably well by a programmer with very limited tooling.

For the non-programmer the story is somewhat different. Most XML based languages are complete gibberish to people without significant technical expertise. A business person will need non-trivial tools to allow them to consume the information locked up in an XML file.

Data interchange formats implicitly value the wider distribution of information. Why else would you be exchanging data. It is disappointing that so many of these formats are based on technology the excludes all but those with sophisticated tools or deep technical knowledge. Data interchange formats should be designed first for people, and second for computers. A properly designed data interchange format should be consumable, using commonly available tools, by any person who is familiar with the domain.

This means that XML is pretty much right out.

Fortunately there is [HTML+RDFa](http://www.w3.org/TR/xhtml-rdfa-primer/). RDFa allows [RDF](http://www.w3.org/TR/rdf-primer/) data sets to be serialized into HTML documents. The information can then be consumed by humans using any web browser. The raw data can be readily extracted by tools.

Consider the following two examples. Each is part of an SPDX<sup id='rdfa-as-interchange-formatfnref:1'><a href='#rdfa-as-interchange-formatfn:1' rel='footnote'>1</a></sup> file. In first example, HTML+RDFa, both groups are easily supported. The information is displayed in a way that it can be understood by most human and the data is machine readable. In the second the information is machine readable but quite difficult for humans to interpret.

<div style='border:solid 2px gray; overflow:auto;'>
  <h2 style='margin-top:0.25em; margin-bottom:0.25em;'>
    Files in zlib 1.2.5
  </h2>
  
  <table style='border-collapse:collapse;'>
    <tr>
      <th>
        Name
      </th>
      
      <th>
        Type
      </th>
      
      <th>
        License
      </th>
      
      <th>
        Checksum
      </th>
      
      <th>
        Copyright
      </th>
    </tr>
    
    <tr about='https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&package_version_id=3690#adler32.c' typeof='spdx:File'>
      <td property='spdx:Name'>
        adler32.c<br /> <span resource='https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&package_version_id=3690' rev='spdx:MemberFile' />
      </td>
      
      <td property='spdx:Type'>
        source
      </td>
      
      <td>
        <a href='http://spdx.com/licenses/zlib' rel='spdx:License'>Zlib</a>
      </td>
      
      <td datatype='spdx:sha256' property='spdx:mac'>
        gWAPnq8fV6sVKdiYkgJQ1nFoTaXXSqoVfJbMCr9Kzd0
      </td>
      
      <td property='spdx:Copyright'>
        unknown
      </td>
    </tr>
    
    <tr about='https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&package_version_id=3690#amiga/Makefile.pup' typeof='spdx:File'>
      <td property='spdx:Name'>
        amiga/Makefile.pup<span resource='https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&package_version_id=3690' rev='spdx:MemberFile' />
      </td>
      
      <td property='spdx:Type'>
        other
      </td>
      
      <td>
        <a href='http://spdx.org/licenses/Zlib' rel='spdx:License'>Zlib</a>
      </td>
      
      <td datatype='spdx:sha256' property='spdx:mac'>
        plyzzUCxuOx34oiXTdncU9ke14u.SV6UzMhN3UI.3&#215;8
      </td>
      
      <td property='spdx:Copyright'>
        unknown
      </td>
    </tr>
    
    <tr about='https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&package_version_id=3690#ChangeLog' typeof='spdx:File'>
      <td property='spdx:Name'>
        ChangeLog<span resource='https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&package_version_id=3690' rev='spdx:MemberFile' />
      </td>
      
      <td property='spdx:Type'>
        other
      </td>
      
      <td>
        <a href='http://spdx.org/licenses/Zlib' rel='spdx:License'>Zlib</a>
      </td>
      
      <td datatype='spdx:sha256' property='spdx:mac'>
        rxKcRCSHu8.4tHMdiJRINMatY4efBCMz.PmHpo.gj9s
      </td>
      
      <td property='spdx:Copyright'>
        unknown
      </td>
    </tr>
    
    <tr about='https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&package_version_id=3690#contrib/ada/readme.txt' typeof='spdx:File'>
      <td property='spdx:Name'>
        contrib/ada/readme.txt<span resource='https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&package_version_id=3690' rev='spdx:MemberFile' />
      </td>
      
      <td property='spdx:Type'>
        other
      </td>
      
      <td>
        <a href='http://spdx.org/licenses/GPL-2.0' rel='spdx:License'>GPL-2.0</a>
      </td>
      
      <td datatype='spdx:sha256' property='spdx:mac'>
        j.nlMD8ujot0bHglDnS3xK63zmIS_c51H8Ogzlakf.I
      </td>
      
      <td property='spdx:Copyright'>
        unknown
      </td>
    </tr>
  </table>
</div>

The following is a similar amount of information expressed in RDF/XML

    <spdx:File rdf:about="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip=3690#CMakeLists.txt">
      <spdx:Copyright>unknown</spdx:Copyright>
      <spdx:License rdf:resource="http://spdx.org/licenses/Zlib"/>
      <spdx:Name>CMakeLists.txt</spdx:Name>
      <spdx:Type>source</spdx:Type>
      <spdx:sha1>L_NaoTEuHjO8uI1VLGIai2rDk7wPkoyeSe5vVD1gxOc</spdx:sha1>
    </spdx:File>
    <spdx:File rdf:about="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip=3690#ChangeLog">
      <spdx:Copyright>unknown</spdx:Copyright>
      <spdx:License rdf:resource="http://spdx.org/licenses/Zlib"/>
      <spdx:Name>ChangeLog</spdx:Name>
      <spdx:Type>other</spdx:Type>
      <spdx:sha1>rxKcRCSHu8.4tHMdiJRINMatY4efBCMz.PmHpo.gj9s</spdx:sha1>
    </spdx:File>
    <spdx:File rdf:about="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip=3690#FAQ">
      <spdx:Copyright>unknown</spdx:Copyright>
      <spdx:License rdf:resource="http://spdx.org/licenses/Zlib"/>
      <spdx:Name>FAQ</spdx:Name>
      <spdx:Type>other</spdx:Type>
      <spdx:sha1>qNaKL48VknhfAA7NgpOLjMoyHlxv45x.hMu8UfAy0Fc</spdx:sha1>
    </spdx:File>
    <spdx:File rdf:about="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip=3690#INDEX">
      <spdx:Copyright>unknown</spdx:Copyright>
      <spdx:License rdf:resource="http://spdx.org/licenses/Zlib"/>
      <spdx:Name>INDEX</spdx:Name>
      <spdx:Type>other</spdx:Type>
      <spdx:sha1>HMjPi3ZRY2Anq1vc4Lfl5x77JWj3hhaWWT.I34QPK9g</spdx:sha1>
    </spdx:File>

The extreme accessibility of HTML+RDFa for both humans and machines makes it an obviously superior choice for data interchange formats. HTML+RDFa is a relatively new entry into the arena. Hopefully we will see more data formats use this superb technology.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='rdfa-as-interchange-formatfn:1'>
      <p>
        The <a href='http://spdx.org/'>Software Package Data Exchange</a> project is designing a way to exchange licensing information for software packages. The current phase of development is primarily focused on simple manifest and copyright licensing related information.
      </p>
      
      <p>
        <a href='#rdfa-as-interchange-formatfnref:1' rev='footnote'>&#8617;</a></li> </ol> </div>
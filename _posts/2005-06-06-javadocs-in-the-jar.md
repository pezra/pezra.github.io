---
id: 118
title: Javadocs in the Jar
date: 2005-06-06T15:04:24-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2005/06/java-api-documentation-in-the-jar/
permalink: /blog/2005/06/javadocs-in-the-jar/
categories:
  - Software Development
tags:
  - Software
---
I recently upgraded to Eclipse 3.1RC1. While importing my project I noticed that you can associate a set of javadocs stored in an archive to a jar your project requires. After noticing this I checked IntelliJ IDEA and found that it can also use javadocs in an archive. I have never used NetBeans but suspect it has the same behavior.

This means that the javadocs for a particular API can be stored in the jar with the API itself. I wonder why this is not common? If it were common IDEs would probably automatically find and use the javadocs in a jar, if there were any, rather than forcing the user to manually associate them. (It is possible that IDEs do this already.) Even better would be standard manifest attribute that specifies the URI of the javadocs, which could be relative to the root of the jar or an external URI. This would allow javadocs to be included in the jar when appropriate and kept external otherwise (though I have a hard time imagining a situation in which external javadocs would really be a better choice) but either way IDEs would still be able to find the API documentation without manual intervention.

From now on any library jars I create will include the javadocs. Even if IDEs cannot find it automatically having the API documentation in the jar is still a better choice than the documentation being completely disconnected from the implementation.
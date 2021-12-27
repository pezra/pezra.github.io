---
id: 108
title: 'What Is A &#8220;Unit&#8221; Test'
date: 2005-05-06T09:39:34-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2005/05/what-is-a-unit-test/
permalink: /blog/2005/05/what-is-a-unit-test/
categories:
  - Software Development
tags:
  - Agile
  - Software
---
I think that one reason unit testing is such a big win is that it integrates design verification into the design (read: coding) process. (See Jack Reeves&#8217; [Code As Design](http://www.developerdotstar.com/mag/articles/reeves_design.html).) This connection has made me start to pay a little more attention to the unit tests I see around me and I have found some odd ideas about what is a unit test vs a functional test. There is quite a lot of room for interpretation here because the name is, probably intentionally, vague. These ideas, that I think are odd, have caused me to refine my idea of what a unit test is.

One of the odd ideas is that a unit test should only test a single class. In some ways this seems to be reasonable. This is basically the unit test orthodoxy that I originally learned, though perhaps taken to an extreme. However, I think this view of unit tests is _not_ internally consistent. Even simple unit tests test how different classes fit together. When is the last time you mocked the String, List or other basic classes that come with the language you are using? No one does that because it is not worth the effort. But by not doing that you are inherently not testing a single class, you are testing that a set of classes work together correctly.

<pre class='markdown-html-error' style='border: solid 3px red; background-color: pink'>REXML could not parse this XML/HTML: 
&lt;p&gt;The other odd idea is that a test that interacts the outside world &#8212; the file system, network, etc. &#8212 should be called "functional" tests rather than unit tests.  I am a firm believer that names are, almost always, important.  The impact of calling the tests functional, rather than unit, is that they do not get run nearly as often.  This happens because the "functional" tests often require significant setup.  So everyone basically avoids running the functional tests that are not directly related to the code on which they are working, even if some portion of them are easy to run.&lt;/p&gt;</pre>

I dislike both of the above ideas. In a perfect world, I think unit tests would test every possible use of every bit of code in the system and they would be run before every check-in. This is obvious unrealistic but I think you end up at a better place if you start at what you would really like, and then remove only the parts that are not feasible. There are two main obstacles to my perfect world unit tests. First, they would take a long time for any non-trivial application. Worse yet, some tests are by definition long running and you do not want to run those tests every time you check-in. Second, some of the tests you would like require an environment that is difficult to recreate programmatically, for example if you need a DB in known state, middle-ware that only your test is using, etc.

[TestNG](http://www.beust.com/testng/) has a nice solution for long testing times. (There is a lot about TestNG that concerns me but this feature is cool.) You can annotate your tests in such a way that you can exclude tests which take a long time from any particular test run. Those long running tests can still be unit tests, just like all the other unit tests, but they can be excluded when it is important for the testing to complete quickly. In other frameworks you can achieve the same behavior by sequestering your long tests in particular fixtures which are not included in the &#8220;normal&#8221; test suite, but are run by your automated build system.

The second issue is the one concerns me the most. This is because running these tests is not just a matter of waiting for them to finish but rather knowing how to setup the environment such that they will work at all. Normally these tests are off loaded to a QA or test team. This means that you have moved the verification of the design to a team outside of your control. This strikes me as sub-optimal because is lengthens the time between defect introduction and detection. Unfortunately, I do not have a good solution for this problem. In most environments it is more acceptable to have a QA team run these sorts of tests than to have the developers spend a lot of time setting up and maintaining multiple independent environments for tests purposes. [Joel points out](http://www.joelonsoftware.com/items/2005/05/02.html) that in some situations VMware may be a good solutions to this problem.

For me, the difference between a unit test and a functional test has nothing to do with what is tested, but rather when the test is run. If it is primarily run as part of coding it is a unit test. If it is primarily run outside of the coding process it is a functional test. The sooner you can find a flaw in the design the better so it is better to do as much testing as possible as part of coding, so I think all levels of software should be tested in unit tests. There should be fixtures that test individual classes, the interactions of classes in individual packages and the interactions of classes across packages. These fixture should do what ever is necessary to support the tests, including writing to the file system or network. All of those tests go into the main test suite unless one of the following is demonstrably true.

  * It is not feasible to setup the appropriate environment programmatically
  * The test would take an unreasonable amount of time to execute

Those two criteria are a bit vague, and intentionally so. I think that every project has to decide for itself what feasible and reasonable. I can say that for the projects on which I have worked, the execution time of the tests has been completely immaterial, with the exception of load tests.
---
id: 175
title: 'Test Anti-Pattern: Proving The Code is Written Like the Code Is Written'
date: 2005-10-21T13:30:12-06:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=175
permalink: /blog/2005/10/test-anti-pattern-proving-the-code-is-written-like-the-code-is-written/
categories:
  - Software Development
tags:
  - Agile
  - Software
---
Testing is suppose to verify that code does _what_ we want. However, I have seen many &#8220;unit&#8221; tests that spend a lot of time verifying that code works how they wrote it, rather than verifying that it achieves the desired result. The worst cases of this I have seen involve the use of mock objects. This is an real example (I changed the names to protect the guilty) of that anti-pattern (using Java and EasyMock):

<pre class='code'><code>
public void testDoSomthingUseful()
{
  MockControl mockObjToTestControl = MockClassControl.createControl(ClassToTest.class);
  ClassToTest mockObjToTest = (ClassToTest)mockObjToTestControl.getMock();
  
  mockObjToTest.doSubtask1();
  mockObjToTestControl.setReturnValue("abc");
  
  mockObjToTest.doSubtask2();
  mockObjToTestControl.setReturnValue(1);

  ClassToTest objToTest = new ClassToTest()
  {
    String doSubtask1()
    {
      return mockObjToTest.doSubtask1();
    }

    String doSubtask2()
    {
      return mockObjToTest.doSubtask2();
    }
  }

   mockObjToTest.replay();

   objToTest.doSomethingUseful();

   mockObjToTest.verify();
}
</code>
</pre>

As you can see this code proves that the `doSomethingUseful()` calls do `doSubtask1()` and `doSubtask2()`. The problem is that this is a total _useless_ test. The only thing it proves is that the code is written the way the code is written. There are not even any assertions that the desired result as achieved. Even if there were some assertions made about the the results of doing something useful you still have a test which is completely and absolutely tied to the _current_ implementation of the `doSomethingUseful()` method.

Tests should check for the desired behavior<sup><a href='#links-to-bdd-material'>1</a></sup> and avoid checking implementation details. Our tests should not care if next week the `doSomethingUseful()` method is refactored such that it does not call `doSubtask1()` as long as it still achieves the same result. The above example is an extreme case and very few people write tests as pathologically broken as that. However, a disturbing number of people write tests with little thought to whether their are verifying desired behavior or implementation details.

A key way to encourage this behavior oriented approach is to only use an objects, globally, public interface in the tests. One objection I have heard to this approach is that you will miss bugs because you correctness checks will, inevitably, be made at a higher level. This argument is just wrong. Any bug that is undetectable from the public interface _does not matter_. Of course, detecting some bugs via the public interface might be a bit more difficult than immediately reaching into the guts of your objects but it will be better because you will be testing the intended behavior of those objects, not their implementations. As a side benefit you will be implicitly documenting the public interface with is a Good Thing.

As with any rule there are a few situation in which I do write tests against non-public parts of an objects interface. For example, if you have a non-public method that you suspect is likely to break (e.g. it embodies a non-trivial algorithm or involves non-trivial interaction with other components) and it is difficult to diagnose it&#8217;s failures from the public interface. However, I consider this confluence of situations to be extremely suspect. It will most often result from sub-optimal design, better solved by refactoring than than by writing tests against the non-public interface. 

<a name='links-to-bdd-material'>1.</a> [Dave Astels](http://blog.daveastels.com/) is working on something he calls [<acronym title='Behavior Driven Development'>BDD</acronym>](http://blog.daveastels.com/?p=5) (these [slides](http://daveastels.com/files/sdbp2005/BDD%20Intro.pdf) are good too). I think he has got it just about right and I am looking forward to trying out [RSpec](http://rspec.rubyforge.org/) real soon now.
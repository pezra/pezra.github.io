---
id: 80
title: An Anti-pattern I Have
date: 2005-03-09T12:00:59-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/wordpress/?p=80
permalink: /blog/2005/03/anti-pattern-i-have/
categories:
  - Software Development
---
I have just passed the six month mark at my new job. And I am learning a lot of new things, which is nice. However, I recently noticed an anti-pattern in my performance in my new job.

Anti-pattern: Developing generic technical services as a form of procrastination.

My basic tendency is to develop software &#8220;bottom-up&#8221;, by which I mean generic technical services first and then implementing the application logic on top of those. I had been at my previous job for six years and after a couple of years you start to really understand the domain really well. This bottom up development style served me well at my previous job because I, for the most part, understood the problems I was solving (and the environment in which I was solving it) almost with out thinking. It is very easy to write generic technical services first if you have a reasonable understanding of the problem and solution domains.

In my new job I do not have a good understanding of the problem domains or the environment in which my solutions will run. I have spent the last several months working on a small-ish project and I have spent most of that time work on generic technical services to support the actually application logic. At this point I have far more technical service code than I to application code. The technical service code does exactly what I intended it to do but unfortunately it is for more complex than is really necessary for my current project.

It is very easy to procrastinate actually learning the domain, requirements and environment by jumping into coding the technical services you think you will need. Unfortunately, the &#8220;bottom-up&#8221; development style really only works if you understand the requirements, domain and environment well. If you do not understand the problem and environment well you will just write tons of code that is either solves the wrong problem or solves the right problem inappropriately.

PS: This article was prompted by a couple of people I respect, [David Hansson](http://www.loudthinking.com/arc/000418.html) and [Jamis Buck](http://jamis.jamisbuck.org/blog.cgi/programming/The%20perils%20of%20programming%20while%20sick_20050308202933.tx), blogging about mistakes they have made. I agree with David that the net benefits of transparency far out weigh the risks.
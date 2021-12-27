---
id: 609
title: Developing software as if quality matters
date: 2011-11-17T05:00:21-07:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=609
permalink: /blog/2011/11/as-if-quality-matters/
categories:
  - Software Development
---
When we started developing [CloudSwing](http://cloudswing.openlogic.com/) we decided to develop CloudSwing as if quality actually matters. By quality i don&#8217;t just mean that the code functions as designed. High quality products also meet the needs of the business and customers.

In the past there has been a sense of disconnection between the business, QA and development parts of our team. (An all too common situation in my experience.) If quality actually matters there must be good connections between all the stakeholders and quality must be considered at every step of the process. We don&#8217;t have a product manager, or a large QA team<sup id='as-if-quality-mattersfnref:2'><a href='#as-if-quality-mattersfn:2' rel='footnote'>2</a></sup>. That means that if quality is going to be considered at every step it is going to be because the developers are doing it, there is no one else at many of the steps.

Our solution is to acknowledge developer responsibility for all aspects of feature development. That unification reduces the chances of things being overlooked. It does change the workload of developers, though. Now developers must:

  * elicit requirements from all stakeholders
  * write _testable_ acceptance criteria
  * develop automated acceptance tests
  * implement the feature
  * verify all acceptance (included pre-existing ones) pass
  * verify that the feature, as implemented, matches the desire of the champion
  * deploy changes to production

This has lightened the load on QA to the point that our one QA resource can keep up with four developers. QA still has a lot of responsibility:

  * verify the acceptance criteria make sense
  * verify the acceptance criteria is testable
  * verify the acceptance criteria cover all important variations of the feature
  * verify the acceptance tests cover all important variations
  * refine acceptance tests
  * verify all tests pass in a production like environment
  * perform visual inspections in various browsers
  * accept features into the production branch<figure><figcaption> 

#### New feature development process<sup id='as-if-quality-mattersfnref:1'><a href='#as-if-quality-mattersfn:1' rel='footnote'>1</a></sup></figcaption>

![](/blog/uploads/as-if-quality-matters/flow.jpg)  
</figure> 

We have been using this version<sup id='as-if-quality-mattersfnref:3'><a href='#as-if-quality-mattersfn:3' rel='footnote'>3</a></sup> of the process for a while now. I think is has worked really well. In fact, during our last retrospective our QA guy stated that he thought our quality management processes belonged in the &#8220;Good things&#8221; category. I think that is a first i have ever heard a QA person be so positive about processes.

Only time will tell if this approach produces significantly better results but I am very optimistic. So far it seems to have created a world of difference.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='as-if-quality-mattersfn:1'>
      <p>
        In practice the process is more fluid that it looks in the diagram. Developers are empowered to do what it takes to get the job done. Including not following the process. However, not following the process requires an <a href='http://en.wikipedia.org/wiki/Affirmative_defense'>affirmative defense</a> when QA asks &#8220;WTF?&#8221;.
      </p>
      
      <p>
        <a href='#as-if-quality-mattersfnref:1' rev='footnote'>&#8617;</a></li> 
        
        <li id='as-if-quality-mattersfn:2'>
          <p>
            The QA team we do have is super top notch, though.
          </p>
          
          <p>
            <a href='#as-if-quality-mattersfnref:2' rev='footnote'>&#8617;</a></li> 
            
            <li id='as-if-quality-mattersfn:3'>
              <p>
                It has taken a lot iterative refinements to get to this version of the process.
              </p>
              
              <p>
                <a href='#as-if-quality-mattersfnref:3' rev='footnote'>&#8617;</a></li> </ol> </div>
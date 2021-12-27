---
id: 304
title: ActiveRecord race conditions
date: 2007-11-10T23:58:32-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/11/activerecord-race-conditions/
permalink: /blog/2007/11/activerecord-race-conditions/
categories:
  - Software Development
tags:
  - Rails
  - Ruby
  - Software
---
[Ara Howard has discovered that the ActiveRecord validation mechanism does not ensure data integrity](http://drawohara.tumblr.com/post/18926188).<sup id='77e2efb3a7a952efcbb236faa90f2f1640b5b077fnref:1'><a href='#77e2efb3a7a952efcbb236faa90f2f1640b5b077fn:1' rel='footnote'>1</a></sup> Validations feel a bit like database constraints but it turns out they are really only useful for producing human friendly error messages.

This is because the assertions they define are tested by reading from the database before the changes are written to the database. As you will no doubt recall, phantom reads are not prevented by any isolation mode other than [serializable](http://en.wikipedia.org/wiki/Isolation_(computer_science)#SERIALIZABLE). So unless you are running your database in serializable isolation mode (and you aren&#8217;t because nobody does) that means that the use of ActiveRecord validations setup a classic [race condition](http://en.wikipedia.org/wiki/Race_condition).

On my work project we found this out the hard way. The appearance of multiple records should have been blocked by the validations was a bit surprising. In our case, the impacted models happened to be immutable so we only had to solve this from for the `#find_or_create` case. We ended up reimplementing `#find_or_create` so that it does the following:

  1. do a find 2.
    
      1. if we found a matching record return the model object
      2. if it does not exist create a savepoint

  2. insert the record 4.
    
      1. if the insert succeeds return the new model object
      2. if the insert failed roll back to the savepoint

  3. re-run the find and return the result

This approach does requires the use of database constraints but, having your data integrity constraints separated from the data model definition has always felt a bit awkward. So I think this more of a feature than a bug.

It would be really nice if this behavior were included by default in ActiveRecord. A similar approach could be used to remove the race conditions in regular creates and saves by simply detecting the insert or update failures and re-executing the validations. This would not even require that the validations/constraints be duplicated. The validations could, in most cases, be generated mechanically from the database constraints. For example, [DrySQL](http://drysql.rubyforge.org/) already does this.

Such an approach would provide the pretty error messages Rails users expect, neatly combined with the data integrity guarantees that users of modern databases expect.

<div class='footnotes'>
  <hr />
  
  <ol>
    <li id='77e2efb3a7a952efcbb236faa90f2f1640b5b077fn:1'>
      <p>
        You simply must love any sample code that has a method <code>Fubar.hork_the_db</code>.
      </p>
      
      <p>
        <a href='#77e2efb3a7a952efcbb236faa90f2f1640b5b077fnref:1' rev='footnote'>&#8617;</a></li> </ol> </div>
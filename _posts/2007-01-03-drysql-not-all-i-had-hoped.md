---
id: 262
title: 'DrySQL: Not All I Had Hoped'
date: 2007-01-03T17:31:50-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/2007/01/drysql-not-all-i-had-hoped/
permalink: /blog/2007/01/drysql-not-all-i-had-hoped/
tags:
  - Rails
---
I happened upon [DrySQL](http://drysql.rubyforge.org/index.html) the other day and I was immediately interested. DrySQL is an add-on to the standard [ActiveRecord](http://wiki.rubyonrails.org/rails/pages/ActiveRecord) support in Rails that uses a lot more of the meta-data in the database to generated the model classes. The standard ActiveRecord classes basically just use the column names to create accessor and modifier methods for the models. DrySQL takes the idea of using the database schema definition as a source of information much further. It figures out the primary key looking for the primary key column in the database, rather than a column named &#8216;id&#8217;. It setups up validations based on the data type of the columns in the database. It defines joins based on the referential constraints. So basically DrySQL is trying to make ActiveRecord what it should have been from the beginning.

I was really happy when I found that someone is working on improving ActiveRecord. I have always been annoyed by in the inconsistencies of Rails ActiveRecord. For example, Rails ActiveRecord requires some of the meta-data to reside in the DB and some to reside in the code has bugged me. Even worse your are actually required to duplicate some of the meta-data by the standard ActiveRecord library, for example validations.

I use the past tense for my happiness because today I installed DrySQL and I am no longer all that happy. It may well do exactly what it says, which I will just assume, but unfortunately I don&#8217;t have time to really test it. The reason I do not have time to test it is that it adds a least twenty four (24) seconds to the response time for every single request. One of the simplest requests the application handles goes from

> Completed in 0.18736

to

> Completed in 24.24459

simply by adding `require_gem &#39;drysql&#39;` to environment.rb.

That twenty four seconds seems to be to build the User model. Requests that instantiate other models incur even greater overhead. This kind of performance hit is completely untenable. While this overhead would be less of an issue in production because this generation will only be done once per model class it is still a complete show stopper for me, and I suspect for most other people. Really, who can give up thirty seconds of their life every single request, even in their development environment?

There is a possibility, and I really hope this is the case, that this is performance issue really just artifact of my environment. I am running a fairly recent version of Edge Rails (a couple of weeks old), [postgres](http://ruby.scripting.ca/postgres/) 0.7.1 (installed as gem) and postgresql-8.1 all on [Edgy](https://wiki.ubuntu.com/EdgyEft). I would love to hear about it if you see a problem with my setup (or if you are using DrySQL without these issues).

Even though this version of DrySQL does not seem to work for today I remain hopeful. DrySQL is definitely the way ActiveRecord should work and there is no reason generating the model class should take an excessive amount of time. Perhaps the next version of DrySQL will be more performant.
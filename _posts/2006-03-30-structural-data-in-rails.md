---
id: 220
title: Structural Data in Rails
date: 2006-03-30T22:18:44-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=220
permalink: /blog/2006/03/structural-data-in-rails/
categories:
  - Software Development
tags:
  - Rails
  - Software
---
On a [recent project](http://pezra.barelyenough.org/blog/2006/03/my-first-live-rails-app/) I ran into a situation where I needed some structural data. I was writing a conference registration application. Each track at the conference costs a different amount and attendees can sign up for more than one track. We already have an accounting infrastructure that has a concept of a &#8220;product&#8221;, which is just something for which we charge money. So to support registering attendees for tracks, taking their money and letting the bean counters know that we have taken their money I needed to add some new products representing the registration for each track. However these products are far from mere cruft need by the accounting infrastructure, the conference registration code is completely dependent on the existence of these products. There are a set of check boxes on the registration page that the controller maps into the appropriate products from the products table. That means that the conference registration code will simply not work with out those products.

These products are the sort of data I am referring too when I say structural data. In my mind any data whose absence would cause a failure or that is managed as part of the development process, rather than in the application runtime, is structural data. This sort of data (dare I say &#8220;pattern&#8221;?) occurs fairly frequently, in my experience, and can be used to great effect

Given the fact that the code is tightly coupled to structural data it makes sense to manage structural data in the same way you manage the database schema. If you are using Rails odds are you are managing your schema with [migration](http://wiki.rubyonrails.org/rails/pages/ActiveRecordMigration)( and if you aren&#8217;t you should be). Migration is a great way to manage a rapidly changing database schema, and it easily supports creation and modification of structural data. Using migrations in this way has several benefits. It keeps versions of the structural data associated with the versions of the code with which they were intended to work (just check out the project from Subversion and you ready to go). It ensures that those products get inserted when the conference registration code gets deployed (migrating the database is part of the deployment process). And finally it places those vital database records under revision control.

### The Problem {#the_problem}

This technique works very well with the exception of testing. Unfortunately, the way Rails handles the test data means that you are forced to repeat any structural data in both the migrations and in the fixtures. When a test is started in Rails it purges the test database, then it recreates it by copying the schema of the development database<footnote>The details of how exactly this schema  
copying happens vary depending on the schema format you are using and  
whether you are using migrations but the end result is the same. No  
matter what you end up with and exact duplicate of your development  
databases schema.</footnote>. This approach is not completely unreasonable, your test database always has an identical structure as your development database, however I see several problems with it.

Cloning the development database assumes that the development database is up-to-date. Most of the time development databases are up to date but if you checked out and forgot to do a `rake migrate` your development database could quite easily be out of date. If this happens you are going to see test failures and the reason is not going to be immediately obvious (I can hear it now, &#8220;but it works fine in dev&#8230;&#8221;).

Cloning assumes that the development database is the authoritative version of the schema. In my world it is not. The migrations are the authoritative version of the schema. When I go to production I am going to do it by executing the migrations not by dumping my development database schema.

The behavior to clone the database is duplicative. We already have a perfectly good way to create the needed database schema. Namely, the migrations that are going to run in production to product the schema against which this code will run. Why have _more code_ to achieve that same result of building a schema?

Cloning the development database assumes that the structure is all that is important. This completely ignores the structural data which is just as important as the physical structure of the database. To work around this you have to duplicate this structural data in both the migrations and the test fixtures. And I despise repeating myself when I am programming&#8230;

### The solution: schema_format = :migration {#the_solution_schema_format__migration}

I finally got around to creating a solution (you can [download the plugin here](http://pezra.barelyenough.org/blog/wp-content/migration_schema_format_plugin.tar.gz)) that avoids all these problems. This plugin introduces a new schema format of `:migration`. This schema format treats the migrations you have put so much time and effort into creating as the final authority for what belongs in a database for your application. With this plugin installed and enabled tests will start by purging the existing test database and then running the migrations, in order, from 001 to the most recent migration. This guarantees that the tests will be run against the most recent schema that could possibly make it&#8217;s way to production.

This solves the first two issues I raised above. We will ignore the third issue, duplicative code, because the existing code must remain for compatibility reasons and it does not directly impact us, anyway. The fourth issue, structural data, is handled by the plugin also. At first blush it might appear that the behavior I described above would be sufficient to solve this issue also but it is not. This issue remains because the `Fixtures` code in `ActiveRecord` actively deletes all rows from a table before loading the fixture data into that table.

Purging a table before loading the fixture data helps isolate tests from one another by ensuring that a test will never get data that has been modified by a previous test. With [transactional fixtures](http://www.clarkware.com/cgi/blosxom/2005/10/24) this is less of an issue but even with transactional fixture there are situation where modified data will not be removed at the end of a test<footnote>For example, if a test fails the data it  
modified/created will not be removed. This allows for more easy  
debugging of failed tests because state of the database is exactly as  
they left them. On the other hand this is only really useful for the  
very last test that fails.</footnote>. Unfortunately, we usually want fixture data even for tables that contain structural data. This is because the structural data we currently have may not fully exercise the functionality of the associated model class. Having fixture data means that at some point those tables will get wiped.

To avoid this problem the migration schema format plugin includes functionality to protect records in the database that are not fixture data. This is achieved by changing the table purging behavior of fixture loading. Rather than purging the entire table the fixture loading code only deletes the record that has the same primary key as the fixture it is currently loading. This means that your fixture data and structural data can live in peace and harmony. The only constraint is that fixture data must never use a primary key that is also used by a piece of real structural data. That constraint is easy to deal with simply by using large values for the primary key in fixture data that needs to play nice with structural data.

Enabling the migration schema format is easy:

1. download the tar and unpack it into your `vendor/plugins` directory 2. edit your `config/environments.rb` to include the line &#8221;`config.active_record.schema_format = :migration` &#8221; within the `Rails::Initializer.run do |config|` block 3. add the line &#8221;`require &#39;shield_nonfixture_data&#39;`&#8221; to `test/test_helper.rb` immediately after `require &#39;test_help&#39;`

And voila, you can test using migrations and structural data.
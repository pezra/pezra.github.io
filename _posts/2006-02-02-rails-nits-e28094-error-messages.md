---
id: 207
title: Rails Nits â€” Error Messages
date: 2006-02-02T22:45:16-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/blog/?p=207
permalink: '/blog/2006/02/rails-nits-%e2%80%94-error-messages/'
categories:
  - Software Development
tags:
  - Rails
  - Software
---
<pre class='markdown-html-error' style='border: solid 3px red; background-color: pink'>REXML could not parse this XML/HTML: 
&lt;i&gt;Update: Due to a misconfiguration some of my blog entries from the
month of Feburary recently lost. This is merely a repost of the
original content.&lt;i&gt;

At the [Ruby User Groups meeting][] the other day someone asked me
what things I did not like about Ruby and Rails. At the time I did not
have any really good answers, which bothered me because that is an
important question. No technology is perfect and honest critiques are
a vitally important way to improve the state of the art. In that
spirit this is the first in a series to relate some issues I have with
Rails (and fixes when possible).

When everything works Rails is absolutely brilliant. However, it when
things do not go well it often yields ambiguous, vague or misleading
error messages. I have noticed this several time but today it was
driven home once again. Error messages might seem like a little thing
but a single bad error message can send a developer off in the wrong
direction wasting hours (or at least tens of minutes ).

For example, earlier today I was investigating moving our database
schema management onto ActiveRecord::Migration.[^migrations-good] I
found Jamis'; [Getting Started With ActiveRecord Migrations][] article,
which is excellent. I followed all the steps but when I ran the
migrate rake task I got the following instead of the correct schema.

    pwilliams@xps:~/projects/ramps$ rake migrate 
    (in /home/pwilliams/projects/ramps) 
    rake aborted!  
    MysqlError: Table ';ramps_development.schema_info'; doesn';t exist: SELECT version FROM schema_info

My first thought was that I needed create the schema_info table that
the SQL above references. However, while looking for the shape that
table I find that the [ActiveRecord:Migration API documentation][]
says it does not need to be create manually.

&gt; To run migrations against the currently configured database, use
&gt; rake migrate. This will update the database by running all of the
&gt; pending migrations, creating the schema_info table if missing.


At that point I was totally confused, the documentation says this
table will be created for you if necessary but the code is *not
actually creating it*.  After digging around in the
code[^reading-rails-is-complicated] a little I finally
figured that the problem was the table create code was eating the real
error message. The code that creates the schema_info table looks like
this&lt;/p&gt;</pre>

    # Should not be called normally, but this operation is non-destructive.  
    # The migrations module handles this automatically.  
    def initialize_schema_information 
      begin 
        execute "CREATE TABLE #{ActiveRecord::Migrator.schema_info_table_name} (version #{type_to_sql(:integer)})" 
        execute "INSERT INTO #{ActiveRecord::Migrator.schema_info_table_name} (version) VALUES(0)"
      rescue ActiveRecord::StatementInvalid 
        # Schema has been intialized 
      end      
    end

That code just tries to create the needed table. It catches any failures while executing the table creation and eats the error under the assumption that a failure to create the table means the table already exists. And therein lies the problem. I had not granted the user specified in my database.yml rights to added tables. The rails user did not need this privilege before because I was loading sql files as myself not as the rails user. It is appropriate that ActiveRecord::Migration failed, it cannot add tables if the RDBMS does not allow it to, but that error message is totally unacceptable. If the actual problem had be reported it would have taken be about 30 seconds to fix it, rather than 30 minutes.

In the spirit of improving this problem here is a patch that make ActiveRecord::ConnectionAdapters::SchemaStatements#initialize\_schema\_information return a better error message in this case, and possibly others. As it turns out it was actually quite easy to improve this error message. Rather than rescuing the attempt to create the table from all failures. Try to select from the tables first, if that fails then attempt to create the table but let any failure be raise up so that the user sees them. [Here](http://pezra.barelyenough.org/schema_info_table_create_error_messages.rake) is a file that when added to RAILS-PROJECT-DIR/lib/tasks does the trick. Below are the entire contents of that file.

    module ActiveRecord
      module ConnectionAdapters
        module SchemaStatements
    
          # Creates the schema_info table in preparation for using 
          # ActiveRecord::Migration. Obviously, this should not be 
          # called normally, but this operation is non-destructive.
          # The migrations module handles this automatically.
          def initialize_schema_information
            begin
              execute "SELECT COUNT(*) from #{ActiveRecord::Migrator.schema_info_table_name}"
            rescue ActiveRecord::StatementInvalid
              execute "CREATE TABLE #{ActiveRecord::Migrator.schema_info_table_name} (version #{type_to_sql(:integer)})"
              execute "INSERT INTO #{ActiveRecord::Migrator.schema_info_table_name} (version) VALUES(0)"
            end
          end
        end
      end
    end

I tried implementing that as a plugin but apparently plugins do not get loaded when executing the migrate rake task. That makes sense since plugins are really a Rails thing and not a Active record thing. Implementing it as a rake file meant that it gets loaded automagically before the migrate task is actually executed so the code changes are in place a the appropriate time.

This is yet another example of the power of open classes. The ability to fix bugs in the framework and libraries without having to physically patch the shipped source code of the framework or library is priceless.
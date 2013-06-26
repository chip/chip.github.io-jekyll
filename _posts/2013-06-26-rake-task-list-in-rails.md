---
layout: post
title: "Rake task list in Rails"
description: ""
category: 
tags: [Rake Rails Ruby on Rails]
---
{% include JB/setup %}

Sometimes in **Rails** it's helpful to see a list of **Rake** tasks that are
available to you.

Maybe you know there's a task that does something specific, but you can't quite
remember what it was called.  

The solution?  Just list the tasks, like so:

    rake -T

On a few of my projects, this returns quite a massive list of rake tasks, which
can take a while to sift through.  To speed this up, you can always provide a
prefix to help **Rake** filter the results.  For example, let's say I want to
list all of the **db** related tasks, I can just do this:

    rake -T db

On my system running **Rails 3.2.13**, this is the result:

    * rake -T db
    WARNING: Nokogiri was built against LibXML version 2.8.0, but has dynamically loaded 2.7.8
    rake db:create          # Create the database from DATABASE_URL or config/database.yml for the current Rails.env (use db:create:all to create all dbs in the config)
    rake db:drop            # Drops the database using DATABASE_URL or the current Rails.env (use db:drop:all to drop all databases)
    rake db:fixtures:load   # Load fixtures into the current environment's database.
    rake db:migrate         # Migrate the database (options: VERSION=x, VERBOSE=false).
    rake db:migrate:status  # Display status of migrations
    rake db:rollback        # Rolls the schema back to the previous version (specify steps w/ STEP=n).
    rake db:schema:dump     # Create a db/schema.rb file that can be portably used against any DB supported by AR
    rake db:schema:load     # Load a schema.rb file into the database
    rake db:seed            # Load the seed data from db/seeds.rb
    rake db:setup           # Create the database, load the schema, and initialize with the seed data (use db:reset to also drop the db first)
    rake db:structure:dump  # Dump the database structure to db/structure.sql. Specify another file with DB_STRUCTURE=db/my_structure.sql
    rake db:version         # Retrieves the current schema version number

This has been a nice little time-saver for me, so I hope you get similar
mileage.

Enjoy,
Chip

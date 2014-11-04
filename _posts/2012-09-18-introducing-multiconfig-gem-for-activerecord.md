---
layout: post
title: "Introducing MultiConfig gem for ActiveRecord"
description: ""
category: rails
tags: [rails, gem]
permalink: blog/2012/09/18/introducing-multiconfig-gem-for-activerecord
---
{% include JB/setup %}
If you need your model to use a different database from the default databases(production, development etc.) then you have to call method `establish_connection` in the model. The parameters are either complete database config or a key name as specified in database.yml like *other_test* or *other_production* etc. If u need it to dynamically change according to the environment then you write:

    MyModel < ActiveRecord::Base
        establish_connection "other_#{Rails.env}"
    end

This messes the *database.yml* a bit as you end up with a *database.yml* with configs it should not have, since the other configs are not really environments. This gave me an idea to create a gem which allows you to specify separate config files for models which access other dbs. It's this simple:

    # In your Gemfile
    require 'multi_config'

    # In your model
    MyModel > ActiveRecord::Base
        self.config_file = 'other'
    end

Your config file *config/other.yml* just needs to have similar environments to *database.yml*. You can see it is much simpler to maintain and cleaner.

This is how it works:

* ActiveRecord stores the configurations read from database.yml in ActiveRecord::Base.configurations hash (copied from Rails.application.config.database_configuration).

* When `establish_connection` connection is called with a string argument, it loads the config from the configurations hash having key same as the argument.

* Multi config extends this hash. It prefixes filename_ as a namespace in the keys to avoid collisions. Then it calls `establish_connection` with the namespaced key for the current environment.

That's the core really; rest of the code deals with railties initialization, error conditions and the majority is tests. I have been absolutely obsessive about tests and documentation. I have added comments to every detail like `Bundler.require` etc so that it can serve as goto guide for writing gems for myself and maybe for you too. Similarly **CHANGELOG** and **README** are created as per best practices. For continous integration I am using [travis](http://travis-ci.org/) which is really easy to integrate with github.

One interesting thing I did:

    # In your environments config file (e.g. development.rb)
    ActiveRecord::Base.config_file = 'other'
Now, all your models will use the new config file. Though this can also be done by setting **DATABASE_URL** environment variable

There are some caveats of course, like migrations. I may add support for those as well in future. Meanwhile you can do this : [Migration om different DB](http://stackoverflow.com/questions/1404620/using-rails-migration-on-different-database-than-standard-production-or-devel)

 I will be glad if you download/install the gem, fork the repo or share this article. Thanks for reading.
[view source here](https://github.com/shadabahmed/multi_config)

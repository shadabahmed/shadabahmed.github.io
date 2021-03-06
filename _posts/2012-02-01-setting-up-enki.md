---
layout: post
title: "Setting Up Enki"
description: ""
category: rails
tags: [heroku, rails, enki]
permalink: blog/2012/02/01/setting-up-enki
---

{% include JB/setup %}

I have been thinking of setting up my blog for a long time. However, I did not want to host a generic wordpress/blogspot blog. Wanted something which I have more control over and I can add features over time.

Thats how I ended up with ****[enki](http://www.enkiblog.com/**) . It's a very simple rails blogging engine which you can customize very easily. Here's what I changed from the original source:

\* It's a complete portfolio site and not just a blog anymore. You can add description, cv and projects list. You blog will be under /blog scope.
\* Styles added from Twitter ****[Bootstrap](http://twitter.github.com/bootstrap/**) project
\* Performance improvements and analytics to it using **[Dalli](https://github.com/mperham/dalli*(memcache)) and**[New Relic](https://github.com/newrelic/rpm*).
\* Social media bar for each post. Facebook and Google plus right now
\* Disqus comments
\* Changed db to postgres from sqlite for deployment at heroku (Heroku only supports postgres for free accounts).
You can see my github fork at : \*[https://github.com/shadabahmed/shadabahmed.com](https://github.com/shadabahmed/shadabahmed.com*).

Follow the steps below to setup your enki clone on Ubuntu 11.10

### Install git and checkout code:

    sudo apt-get install git

    git clone https://github.com/shadabahmed/shadabahmed.com.git enki

### Install Postgres database:

    sudo apt-get install postgresql
    sudo -u postgres psql postgres

Change password to "***postgres***" - `postgres-# \password postgres`

Exit psql shell - `postgres-# \q`

Create databases:

    sudo -u postgres createdb enki_test
    sudo -u postgres createdb enki_dev
    sudo -u postgres createdb enki_prod

### Install modified Enki App:

Goto app folder and run:

    cd enki
    bundle install
    bundle exec rake db:migrate

#### To customize the blog:

Edit **config/enki.yml**

-   Change title
-   Change the **open\_id** setting under **author**. Open ID is be an url which you can get from your Open ID provider. You might already be having it. See ****[this](http://openid.net/get-an-openid/**)

Now test the app locally - `rails s`

Open <http://localhost:3000/admin> and enter your Open ID url. This will ask for authentication and you are through. You can also skip this in development mode.

If you wish to host this at heroku, see my post on setting up RoR app at heroku - ****[here](/blog/2012/02/04/heroku-setup**)

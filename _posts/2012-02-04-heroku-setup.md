---
layout: post
title: "Setting up your RoR app on Heroku"
description: ""
category: rails
tags: [heroku, rails]
permalink: blog/2012/02/04/heroku-setup
---

{% include JB/setup %}

If you are looking to host your rails site for free(or even paid) without worrying about server configuration etc , think no further than ****[Heroku](http://heroku.com**). **Heroku** is one of the most awesome cloud platforms to host your app with minimal fuss.

You can find many guides and quick starts for heroku but you may still spend hours looking at multiple guides. So if you are set out to do exactly what I wanted to do i.e. ***Deploy my app on heroku at my domain***, this blog will be helpful. Tested on Ubuntu 11.10

### Setting up git

First we need to setup your application's git repository. Ignore if you have already set this up.

    git init
    touch .gitignore
    git add .
    git commit
    # This will create a local repository

### Install and Setup Heroku

    sudo apt-get install heroku-toolbelt
    heroku login # login with your heroku username and password
    heroku create --stack cedar # create heroku app for the local app. This is for rails 3.1+.  Do this in your app directory

### Verifying Heroku Setup

Confirm your app has been created by - `git config -l`. Output should be like:

    user.name=Shadab Ahmed
    user.email=*******@gmail.com
    core.repositoryformatversion=0
    core.filemode=true
    core.bare=false
    core.logallrefupdates=true
    remote.heroku.url=git@heroku.com:empty-samurai-3618.git
    remote.heroku.fetch=+refs/heads/*:refs/remotes/heroku/*

Heroku will become just like another remote repository for you app where you can push commits.

### Deploying on Heroku

Deploying is as simple as a git push.

    git commit -a
    git push heroku master

If successful you will get your app url you can visit to see the app. You can also rename your app with `heroku rename newappname`. Now your app will be available on **http://newappname.herokuapp.com**

### Setting up your domain for your app

If you want to access your app through **http://yourdomain.com** instead of **http://appname.herokuapp.com**. Setting up your domain is very simple:

    heroku addons:add custom_domains
    heroku domains:add yourdomain.com # Setup your domain for your current heroku app

Setup A-Records for your domain for the following IPs:

    75.101.163.44
    75.101.145.87
    174.129.212.2

Also setup C-Name record for your domain www.yourdomain.com
`appname.herokuapp.com`

That's it. You app has been setup and you can access it using http://yourdomain.com .

You might want to optimize your site after this. See my post for setting up backups, analytics and performance tuning of your app on Heroku - ****[Tuning Heroku](/blog/2012/02/15/heroku-tuning**)

Good Luck!

**P.S.** Heroku free account only supports postgres database as of now. I would recommend using postgres for your test, development and production environments for the sake of consistency

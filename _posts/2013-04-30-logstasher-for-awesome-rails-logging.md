---
layout: post
title: "Logstasher - For awesome Rails logging"
description: ""
category: 
tags: [rails]
permalink: blog/2013/04/30/logstasher-for-awesome-rails-logging
---
{% include JB/setup %}
## Rails Logs

Rails logging is a mess, atleast the ***default*** one. I've even seen some gems, spewing garbage into the logfile with no predefined format. Good thing is that you can easily fix/customize it. Infact, you can customize it so much, that you can have this:

<img class="magnifier" src="http://i.imgur.com/zZXWQNp.png" title="Awesome Rails Logs"/>

## What's there in these supposedly *awesome* logs ?

  * Superfast search backend
  * UI is much much cooler than, what the single screenshot shows
  * You can add custom fields such as username and custom rails instrumentations
  * Consolidation of logs across multiple servers
  * Email notifications on events such as exceptions

## How it's done ?

The slick UI with superfast search is made possible with Rails Logs using:

  * [Logstasher Gem](https://github.com/shadabahmed/logstasher) - A rails gem I wrote. It makes rails, generate [logstash](http://logstash.net/) compatible logs with minimal overhead
  * [Logstash](http://logstash.net/) - An open source log collector
  * [Kibana](http://kibana.org/) - A ruby/rack application for visualization and searching the logstash index

## Logstasher Gem

I have already written much about it on the [README at Github](https://github.com/shadabahmed/logstasher). Please check it out to know the usage/installation

## Logstash

Now this is the core of the logging system. It collects and stores the logs. It has a very clean seperation of concerns, basically:

  * **Inputs** - The source for logs. There are tons of options for input sources. We'll be using [file input](http://logstash.net/docs/1.1.10/inputs/file) for local logs and [lumberjack](http://logstash.net/docs/1.1.10/inputs/lumberjack) for remote logs.
  * **Filters** - These are tranformations you would like to apply on the input log. Used for sanitizing and appending any extra information. Since logstasher already sends sanitized logs, we'll not need any filters, thus saving on some processing.
  * **Ouputs** - Again we have many options for outputs. We will use the email and elastic-search output.

See the full list of input, filter and output options [here](http://logstash.net/docs/1.1.10/)

#### Setting up logstash

 * You can compile the jar from [source code](https://github.com/logstash/logstash) or [download it](https://logstash.objects.dreamhost.com/release/logstash-1.1.10-flatjar.jar). My preference is to compile, since it is newer (more fixes). Copy the jar file to `/opt/logstash/logstash.jar`
 * Set it up as a service - [Ubuntu  and SUSE SLES init.d scripts](https://gist.github.com/shadabahmed/5486949)
 * Setup your config file - See [sample config file](https://gist.github.com/shadabahmed/5486949#file-logstash-conf). Copy it to `/etc/logstash/logstash.conf`. The email output functionality is not perfect yet. I am waiting still for this [pull request](https://github.com/logstash/logstash/pull/409) to be merged.

Once your logstash is setup, you now need to configure input source. If the rails app is on the same machine, you can skip the lumberjack section.

For output we're using the embedded elastic-search, so no need for any extra setups.

## Lumberjack

If your app resides on a remote server, or there are multiple app servers, you need to transport the logs to logstash. Lumberjack is a very fast and small utility to do the same with minimal memory usage. It also encrypts the logs, so super secure as well. It will just monitor your logfile for changes and on  any updates, sends the new logs to logstash.

#### Setting up Lumberjack

 * I found compiling lumberjack to be a bit of pain on SUSE(Yes, unfortunately I have to use SUSE). You can have a go yourself by grabbing it from [source](https://github.com/jordansissel/lumberjack). You will need to install [Go Language](http://golang.org/) to compile it. Luckily I found rpm and deb packages [here](https://github.com/hectcastro/chef-lumberjack/tree/master/files/default). Install the deb by `dpkg -i debfilename` or rpm by `rpm -i rpmfilename`
 * Set it up as a service - [init.d script](https://gist.github.com/shadabahmed/5487541)
 * Set up the config file - [sample config](https://gist.github.com/shadabahmed/5487541#file-lumberjack). Create the config file at `/etc/default/lumberjack`

It is a good idea to restart lumberjack when you deploy or rotate logs.

There is a new protocol for lumberjack v2. I will play with it and update the documentation on that.

#### Key and SSL Certificate for lumberjack

You'll notice that key and certificate files are required by logstash and lumberjack. These are required just to encrypt the log shipping. All the commands I used to generate them are consolidated [here](https://gist.github.com/shadabahmed/5487541#file-logstash-cert-sh)

## Kibana

This is the easiest part. Just see [here](http://kibana.org/intro.html)

Once you have everything setup, you should be able to see logs in the Kibana UI. Play around to see the powerful search feature. You can also write your custom queries for .e.g. to get all requests from an IP which raised an exception just put this query in the search field - ` @fields.ip:"157.191.96.84" AND @tags:"exception"`

## Inspirations

 * [https://github.com/roidrage/lograge](https://github.com/roidrage/lograge)
 * [http://www.vmdoh.com/blog/centralizing-logs-lumberjack-logstash-and-elasticsearch]
(http://www.vmdoh.com/blog/centralizing-logs-lumberjack-logstash-and-elasticsearch)
 * [Loggly](Loggly.com) and [Logentries](logentries.com)

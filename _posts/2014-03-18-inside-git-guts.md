---
layout: post
title: "Inside Git Guts"
description: ""
category: Git
tags: [git, ruby]
permalink: blog/2014/03/18/inside-git-guts
---
{% include JB/setup %}
**TLDR;** Git is greatly extensible. I made many custom commands to create an interesting talk on "The Insides of Git". Skip the text and just watch the video for details.

Back in June last year in the cool city of [Pune](http://en.wikipedia.org/wiki/Pune) at the RubyConf India 2013, I gave a talk on Git. It was aptly titled **Inside Git Guts with Ruby**.

The idea was to explore what happens inside the `.git` folder when you do git operations like *commit, push, merge* etc. The talk was far more about ***git*** than ***ruby***. Infact, the part **with Ruby** in the title was added just to ensure a better chance of my proposal getting selected for RubyConf. Although, all of the tools I showed in the talk were written in Ruby, the actual ruby code shown was just about 4 lines. 

Now, here's the actual talk: 

<iframe width="560" height="315" src="//www.youtube.com/embed/lPlwkxrG2NM" frameborder="0" allowfullscreen></iframe>


The talk was greatly inspired by the movie [Inception](http://www.imdb.com/title/tt1375666/combined). Infact I even had a command `git inception` which gave the following output(truncated):

    $ git inception                                           
                            .git
                         .git
                      .git
                   .git
                .git
             .git
          .git
       .git <--- 2nd Level Gitception
    .git

The **Inception** bit was actually the most interesting part of the talk. There were other super cool commands like - `git music`, `git autocommit`, `git server`, `git fireworks`, `git quote`, `git about` and more

Lots of folks have asked me to share the code and the instructions, so here's the [code](https://github.com/shadabahmed/git_guts). To get all the tools on your system:

    gem install git_guts

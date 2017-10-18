---
layout: post
title: "Fix for Passenger `unexpected end of file detected`"
description: ""
category: rails
tags: [rails, passenger]
permalink: blog/2012/09/02/passenger-error-unexpected-end-of-file
---

{% include JB/setup %}
I recently saw this error on one of our servers - Passenger/Nginx error “unexpected end of file detected.”. This was giving no clue at all in the stack trace on what was the exact problem. Everyone has wracked their brains over it with no solutions. We have an apache/passenger stack with REE 1.8.7. No matter what version of passenger we installed, it was giving the same error.

On further digging in the apache logs I saw:

    *** glibc detected *** Passenger ApplicationSpawner: /opt/apps/current: free(): invalid pointer: 0x0000000002eb1780 ***

    and somewhere down in stack trace

    7f66ac1f8000-7f66ac215000 r-xp 00000000 fd:09 5360931                    /opt/ruby-enterprise-1.8.7-2010.02/lib/libtcmalloc_minimal.so.0.0.0
    7f66ac215000-7f66ac415000 ---p 0001d000 fd:09 5360931                    /opt/ruby-enterprise-1.8.7-2010.02/lib/libtcmalloc_minimal.so.0.0.0
    7f66ac415000-7f66ac416000 r--p 0001d000 fd:09 5360931                    /opt/ruby-enterprise-1.8.7-2010.02/lib/libtcmalloc_minimal.so.0.0.0
    7f66ac416000-7f66ac417000 rw-p 0001e000 fd:09 5360931                    /opt/ruby-enterprise-1.8.7-2010.02/lib/libtcmalloc_minimal.so.0.0.0

### Reinstall REE

I did a quick google on tcmalloc and found out other people also having the same issues. So I installed ree without tcmalloc. REE doesn't require it but generally performs better. TCmalloc is part of google-perftools which REE includes. It is an improved memory allocator. You may still want try to look into why it's not working with tcmalloc. To install without tcmalloc just extract the REE archive and run this:

`./install --no-tcmalloc`

Voila! this solved all the issues. Of course I had to whip up a quick bash script to reinstall bundle for all the apps on the server.

I will update this post if I make tcmalloc working :)

Update: Seems like a version issue. I was using ruby-enterprise-1.8.7-2010.02 earlier. I compiled ruby-enterprise-1.8.7-2012.02 with tcmalloc and i did not face the tcmalloc issue.

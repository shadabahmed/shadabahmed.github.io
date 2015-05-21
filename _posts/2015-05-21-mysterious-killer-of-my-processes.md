---
layout: post
title: "Mysterious Killer of my Processes"
description: ""
category: 
tags: [linux]
---
{% include JB/setup %}

Recently I had deployed my node app on of the overloaded linux servers, I was using. It uses `cluster` node library, which also sends me notification if any child process it killed and respawn the child. 

So ever since deployment, everyday around lunch, I would get a hella-lot of emails about the child processes being killed. The weird thing was that everyone of the child process were receiving `SIGKILL`, thus someone was killing them.
During the carnage which would last for around 2-3 hours, even processes such as `npm install` would just be killed as mercilessly.

Shutting down a few processes did help. It slowed down the killing, but did not stop it completely. Only passing of time would stop it, so by late evening till next day lunch, the carnage was again silent.

So next day, I figured out what other thing was happening at the same time and turns out another team was uploading a huge data which also involved lots of processing and thus system was becoming `OOM` (Out of Memory) and the `OOM Killer` was killing my processes. Looking at the mem stats:


    free -m
                 total       used       free     shared    buffers     cached
    Mem:          2008       1995         13          0          0         23
    -/+ buffers/cache:       1971         36
    Swap:         1535       1535          0



This server was super-contrained for memory. Obviously not ideal amount of **RAM** and even worse **SWAP** space but this was a `sandbox` server, so a bit of unsettleness dint matter so much. Looking at more logs for `oom killer`:

    grep -i 'kill' /var/log/messages | awk -F 'kernel:' '{print $2}'
     [1776235.313736] PassengerHel/ei invoked oom-killer: gfp_mask=0x201da, order=0, oom_adj=0
     [1776235.313791]  [<ffffffff810b8dec>] oom_kill_process+0xcc/0x2f0
     [1776235.322903] Out of memory: kill process 12050 (ruby) score 113591 or a child
     [1776235.322906] Killed process 12050 (ruby)
     [1776766.287970] python invoked oom-killer: gfp_mask=0x201da, order=0, oom_adj=0
     [1776766.288033]  [<ffffffff810b8dec>] oom_kill_process+0xcc/0x2f0
     [1776766.298348] Out of memory: kill process 12121 (ruby) score 112262 or a child
     [1776766.298350] Killed process 12121 (ruby)
     [1776766.304348] python invoked oom-killer: gfp_mask=0x201da, order=0, oom_adj=0
     [1776766.304389]  [<ffffffff810b8dec>] oom_kill_process+0xcc/0x2f0
     [1776766.313545] Out of memory: kill process 8259 (python) score 85423 or a child
     [1776766.313548] Killed process 8259 (python)
     ...truncated
     
As sou can see, `OOM Killer` killed a number of processes. The best way to resolve this `OOM Killing` is to add more **RAM** and **SWAP** space. You can see more helpful reference for the `OOM Killer` here:

[oom-killer](http://www.oracle.com/technetwork/articles/servers-storage-dev/oom-killer-1911807.html)

[will-linux-start-killing-my-processes-without-asking-me-if-memory-gets-short](http://unix.stackexchange.com/questions/136291/will-linux-start-killing-my-processes-without-asking-me-if-memory-gets-short)

[where-can-i-see-a-list-of-kernel-killed-processes](http://unix.stackexchange.com/questions/10077/where-can-i-see-a-list-of-kernel-killed-processes)

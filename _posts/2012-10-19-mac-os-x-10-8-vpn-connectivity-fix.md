---
layout: post
title: "MAC OS X 10.8 VPN Connectivity Fix"
description: ""
category: mac
tags: [mac]
---
{% include JB/setup %}
I recently updated my MAC OS X version to 10.8.2. After this update, my VPN started having random issues. Sometimes while connecting it would show me **_authentication failure_** or **_no server response_**  while at other times it would connect perfectly. On some networks it refused to let me connect at all. I have found a solution to this and now my VPN connects fine every time. Here's how:

### Step 1

Open terminal and type:

    # Create backup
    cp -r /Library/Preferences/SystemConfiguration ~ 

    # Delete the SystemConfiguration
    sudo rm -rf /Library/Preferences/SystemConfiguration

This will delete all your custom networks. Make sure you have the settings written down somewhere.

### Step 2

Reboot and recreate the VPN network connection

Test your VPN now. Voila! It works !

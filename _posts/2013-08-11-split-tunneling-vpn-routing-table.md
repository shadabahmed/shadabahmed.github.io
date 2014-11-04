---
layout: post
title: "Split Tunneling on VPN via Routing Table"
description: ""
category: Networking
tags: [linux, networking]
permalink: blog/2013/08/11/split-tunneling-vpn-routing-table
---
{% include JB/setup %}
### Split Tunneling

Whenever I connect to VPN on my mac, my default route is modified to this:


    $ netstat -nr
    Routing tables

    Internet:
    Destination        Gateway            Flags        Refs      Use   Netif Expire
    default            utun1              UCS            34        0   utun1 
    default            192.168.2.1        UGScI           0        0     en0

What this does is, route all my traffic, even the one which is outside of VPN network routed through the VPN interface ```utun1```. The second default route via ```192.168.1.1``` is not even used. This is unnecessary and sometimes counterproductive as the VPN network takes extra load on bandwidth/resources for the IPs outside of its network and even bans sites which do not require banning.

You can easily change your  routing table to circumvent this using the script below. This way you access public IPs directly and private IPs over VPN.  This concept is called [Split Tunneling](http://en.wikipedia.org/wiki/Split_tunneling)

### Modifying Routing Table

Now to modify routing table for split tunneling, all you need to do is to find out the subnet of the IPs in your VPN you need access to. Let't take for example, if the subnets of my VPN private space are ```10.109.0.0``` and ```10.110.0.0``` (Any IPs starting with 10.109 and 10.110): 

    #! /usr/bin/env bash
    if (( EUID != 0 )); then
      echo "Please, run this command with sudo" 1>&2
      exit 1
    fi
    WIRELESS_INTERFACE=en0
    TUNNEL_INTERFACE=utun0
    GATEWAY=$(netstat -nrf inet | grep default | grep $WIRELESS_INTERFACE | awk '{print $2}')

    echo "Resetting routes with gateway => $GATEWAY"
    echo
    route -n delete default -ifscope $WIRELESS_INTERFACE
    route -n delete -net default -interface $TUNNEL_INTERFACE
    route -n add -net default $GATEWAY
    for subnet in  10.109 10.110
    do
      route -n add -net $subnet -interface $TUNNEL_INTERFACE
    done

Just save the script as ```/usr/bin/vpn``` and whenever you connect to vpn, just run ```sudo vpn```. This  is what the script does:

* Finds wireless router ```IP (en0)```
* Remove the default routes set by VPN
* Set the IP found for en0 as the default gateway
* Add the specific routes for your VPN subnet

If you are connecting over wired network, just change the ```en0``` in script to ```en1```. Don't change your VPN DNS to make sure you can resolve private domains on the VPN.

Looking at the routing table again after running the script:

    $ netstat -nr
    Routing tables

    Internet:
    Destination        Gateway            Flags        Refs      Use   Netif Expire
    default            192.168.2.1        UGSc            1        0     en0
    ...
    10.109                 utun1              USc            0        0   utun1
    10.110                 utun1              USc            0        0   utun1
 
As you can see now, our default gate is back to wifi router IP - ```192.168.1.1 on en0``` and there are some routes for the subnets inside the VPN. 

You can test the route as well with a `route` command. Route for any IP outside of your VPN:

    $ route get google.com                                            
    route to: lga15s29-in-f5.1e100.net
    destination: default
       mask: default
       gateway: 192.168.1.1
       interface: en0
       flags: <UP,GATEWAY,DONE,STATIC,PRCLONING>
       recvpipe  sendpipe  ssthresh  rtt,msec    rttvar  hopcount      mtu     expire
                0         0         0         0         0         0      1500         0

Route for IP inside your VPN:

    $ route get 10.109.10.135
    route to: 10.109.10.135
    destination: 156.107.0.0
       mask: 255.255.0.0
       interface: utun0
       flags: <UP,GATEWAY,DONE,STATIC,PRCLONING>
       recvpipe  sendpipe  ssthresh  rtt,msec    rttvar  hopcount      mtu     expire
                0         0         0         0         0         0      1280         0

To reset the routing table back, just disconnect from the VPN. Same script can be used on Linux with some modifications.

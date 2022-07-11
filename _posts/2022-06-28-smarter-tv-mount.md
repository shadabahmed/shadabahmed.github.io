---
layout: post
title: "A Smarter TV Mount with Arduino"
description: "How I made a motorized TV Mount better with Arduino"
category: 
tags: [arduino, electronics]
---
{% include JB/setup %}

* TOC
{:toc}

### Intro

Since I'm on a break in-between jobs, I thought I'd revisit my old blog and post about something interesting I've done during the break. I recently installed a motorized above-fireplace TV mount since I've been wanting to place my TV above the fireplace in my home, and have a way to lower it down while watching. This [Monoprice TV Mount](https://www.amazon.com/Monoprice-Motorized-Fireplace-Full-Motion-Articulating/dp/B08J9VHTZ6){:target="_blank"} seemed perfect for it:

![TV Mount Image](/assets/images/concept.png)

It does handle the weight pretty well. However, a few major issues:
* The remote is horrible. It picks up IR signals from the tv remote and moves on things like volume up/down/mute etc. Super annoying!
* It has a powerful motor, but no sensor to stop if something is obstructing. It literally can crush anything which comes in its way.
* If you press the up button for too long, the motor keeps hammering as it's stuck at the top-most position and doesn't know it should stop.
* At the top-most position, you can still swivel the TV right/left, possibly hitting the wall. Guaranteed to damage the TV or the wall behind it.

I decided to make it better with features like:
* RF remote instead of IR, so it isn't affected by the TV remote or any other IR source
* Detect if the TV is too close to the wall and prevent any further moving up to prevent damage to the mount or the TV
* Detect motor stalls or obstructions by measuring motor current and stop the motors in such fault conditions
* Move the mount to the rest position(top-most) automatically, when the TV is turned OFF
* Similarly, move the mount to the viewing position(bottom most) automatically, when the TV is turned ON

### Components

Here's a block diagram of the final solution:

{:refdef: style="text-align: center;"}
![Block Diagram](/assets/images/block_diagram.png){:.centered}
{: refdef}

The two time-of-flight(TOF) distance sensors are [VL53L0x](https://www.st.com/en/imaging-and-photonics-solutions/vl53l0x.html){:target="_blank"} sensors I brought from [Amazon](https://www.amazon.com/HiLetgo-VL53L0X-Distance-Measurement-Breakout/dp/B071DW8M8V/){:target="_blank"}. These are pretty accurate from 20mm to 2000mm. These are placed behind the TV on the opposite ends to detect the distance from the wall as well as the swivel angle.

![TV Mount Image](/assets/images/motion.png)
![TV Mount Image](/assets/images/top_image.png)

### The Circuit

The circuit is just soldered on two separate prototype PCBs, layered one over another. The top part contains the Arduino and connectors for the sensors. Also has a USB connector for the cable coming from the TV for TV On/Off detection. The bottom part contains the two H-bridges for the two motors in the mount(up/down and swivel). There's a [DC/DC converter](https://www.amazon.com/gp/product/B01MQGMOKI/){:target="_blank"} as well for running the Arduino from the 24V power supply.

![TV Mount Image](/assets/images/pcb.png)

Fits nearly perfectly in this [enclosure](https://www.amazon.com/gp/product/B08PZCC562/){:target="_blank"} built for Raspberry PI:

![TV Mount Image](/assets/images/assembled.png)

### The Code

The code is written in C++ using the Arduino framework. I initially started coding in Arduino IDE but as the code got bigger, switched over to the Jetbrains CLion using the Platform IO framework. I must say that CLion is so much better to write code in!

The main part of the code is a state machine that manages the various states and transitions based on the inputs from the remote/sensors. You can find the code with libs here: 

[Github Repo](https://github.com/shadabahmed/smarter_tv_mount){:target="_blank"}

As a side challenge, I also ported the code to an almost pin-compatible [Arduino Nano RP2040](https://docs.arduino.cc/hardware/nano-rp2040-connect){:target="_blank"}, which is based on the [Raspberry Pi Pico](https://www.raspberrypi.com/products/raspberry-pi-pico/){:target="_blank"} microcontroller. 

### Final Setup

Right now, I crammed the circuit, power supply etc in a cable box. I do plan to make it neater. Videos of how the whole thing works:

<div class="vid-container row ">
  <div class="vid-border col-xs-6 nopadding">
    <img src="/assets/images/tv_on.png">
      <details>
      <summary role="button" aria-label="static image" class="gif-player">
        <img class="button play" src="/assets/images/play_circle_filled-24px.svg">
        <img class="button pause" src="/assets/images/pause_circle_filled-24px.svg">
        <img src="/assets/images/tv_on.jpg" class="thumb" loading="lazy">
        <img src="/assets/images/tv_on.gif" class="gif">
      </summary>        
    </details>  
  </div>
  <div class="vid-border col-xs-6 nopadding">
    <img src="/assets/images/tv_off.png">
    <details>
      <summary role="button" aria-label="static image" class="gif-player">
        <img class="button play" src="/assets/images/play_circle_filled-24px.svg">
        <img class="button pause" src="/assets/images/pause_circle_filled-24px.svg">
        <img src="/assets/images/tv_off.jpg" class="thumb" loading="lazy">
        <img src="/assets/images/tv_off.gif" class="gif">
      </summary>        
    </details>
  </div>
</div>

### Conclusion

This has been a fun little project to do, and pretty useful as well. I last coded on a microcontroller (AVR Atmega series) in 2008, and it was all on custom-manufactured circuit boards. Comparing that to now, here's what I observed:

* There are many microcontrollers available today with a host of connectivity options like LoRa, Wifi, Bluetooth etc. 
* You can buy fully assembled breadboard-ready microcontroller boards for just a few dollars
* The diversity in the microcontroller choice is also supplemented by a good set of frameworks like Arduino, Platform IO etc. Not too hard to write portable code without a lot of changes like I did with Arduino Nano RP2040
* You can virtually find any sensor on Amazon, Sparkfun, AdaFruit or many other sites.

Overall so much easier to do your hobby projects today. I too hope to pick up a few other cool projects as well. Will keep you posted. Cheers !!!!
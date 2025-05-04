---
layout: post
title: "Fan Mod for Meanwell Power Supply"
description: "How I silenced my loud Meanwell PSU with quiet fans"
category: Electronics
tags: [electronics, modding, robotics]
---

{% include JB/setup %}

* TOC
{:toc}

Working in robotics often means dealing with reliable yet noisy Meanwell power supplies. Unfortunately, the tiny 40mm fans inside these units are incredibly loud, significantly affecting comfort and causing increased tinnitus over extended periods. In an effort to reclaim some sanity, I embarked on a modding adventure to silence my Meanwell PSU. Here’s a straightforward guide on how you can do the same. The specidic model I worked with is 
<a href="https://www.meanwell.com/Upload/PDF/RSP-2000/RSP-2000-SPEC.PDF" target="_blank" rel="noopener noreferrer">RSP-2000</a>


![Meanwell](/assets/images/fan_mod/RSP-2000-483.png){:.media-left, style="width:300px"}

> **⚠️ Caution:**  
> Meanwell power supplies can retain charge even after power-off. Ensure the unit is unplugged and powered off for at least a few hours before opening it up.

---

## Parts Required

1. **Fan Cable Adapters** ([2-pack, 4-pin](https://www.amazon.com/gp/product/B07Q5BTTDX)):
   - Allows connection of larger fans to Meanwell’s small fan headers.
   - **Note:** You’ll need to re-pin these cables (details below).

2. **2 x 120mm CPU Cooling Fans**:
   - Any standard brand works, but for silent operation, I highly recommend:
     - [Noctua NF-A12x15 PWM chromax](https://www.amazon.com/Noctua-NF-A12x15-PWM-chromax-Black-swap-120x15mm/dp/B0813X9G8T) (ultra-quiet & reliable).
   - RGB fans are also an option if you want aesthetics, provided you have a motherboard or RGB controller to power the lights.

3. **Mesh Filters** ([ATX case dust filters](https://www.amazon.com/gp/product/B0BN89YW8R)):
   - Protects internal components from dust while maintaining airflow.

---

## Modding Steps

### 1. Open the Meanwell PSU Case
- Unscrew and remove the top cover.
- Remove the two noisy 40mm fans, carefully scraping off any hot glue.

![Removing Meanwell Fans](/assets/images/fan_mod/meanwell_fans.png){: width="500px" }

You’ll see two exposed 4-pin connectors:

![4-pin connectors exposed](/assets/images/fan_mod/connector_board.png){: width="500px" }

---

### 2. Prepare the Fan Adapter Cables
- The default adapter pinout **will damage your fans** if connected directly. You need to pry out the individual wires and re-pin the adapters following this schematic:

![Modified Adapter Schematic](/assets/images/fan_mod/connector_cable.png){: style="width:600px" }

- **Larger End (for the new fans):**

![Adapter Large End](/assets/images/fan_mod/connector_big.png){: style="width:300px" }

- **Smaller End (connects to Meanwell PSU):**

![Adapter Small End](/assets/images/fan_mod/connector_small.png){: style="width:300px" }

---

### 3. Connect and Prepare the Fans
- Zip-tie the two 120mm fans together for easy mounting:

![Fans Tied Together](/assets/images/fan_mod/fans.png){: style="width:500px" }

---

### 4. Connect Fans to Meanwell PSU
- Plug fan connectors into the larger ends of adapter cables.
- Connect smaller adapter ends to Meanwell’s PCB connectors.
- Route cables neatly through bottom openings for tidy management:

![Fans Connected to PCB](/assets/images/fan_mod/connected.png){: style="width:500px" }

I also used aluminum metal tape (used on dryer vents) to secure loose cables

---

### 5. Cover PSU with Mesh Filter
- Cut ATX mesh filter to match Meanwell PSU dimensions.
- Cover PSU top opening to allow airflow yet protect from dust:

![Mesh Filter on PSU](/assets/images/fan_mod/mesh.png){: style="width:600px" }

---

### 6. Mount Fans onto the PSU
- Position fans on top of the PSU, aligning carefully.
- Secure with screws through fan corners. You might need to drill small holes in fan frames for alignment:

![Fans Mounted on PSU](/assets/images/fan_mod/modded.png){: style="width:600px" }

- Optionally, cover side openings and cable gaps using aluminum tape for better airflow management.

---

## Final Result

Enjoy your newly quiet, cool-running Meanwell power supply. My PSU is now effectively unnoticeable and temperatures are good to touch even at load. Here's my completed mod in it's full RGB glory:

![Meanwell Modded PSU RGB](/assets/images/fan_mod/case.gif){: style="width:400px" }


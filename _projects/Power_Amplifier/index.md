---
layout: post
title: High-Efficiency Power Amplifier – OPA564 Design Project
description: A high-output current amplifier system designed to maximize efficiency and output power for an 8Ω speaker. Developed and validated in LTSpice with full thermal and performance analysis. 
skills:
  - High-current analog design
  - LTSpice simulation
  - Power & thermal modeling
  - Efficiency optimization
  - Inverting amplifier topology
  - THD suppression
  - Op-amp (OPA564) integration
  - PCB design for audio amplifier (Altium)
main-image: /power_pcb.webp
---

## 1. Objective & Design Overview

The goal of this project was to design a power amplifier capable of delivering up to **25W** to an **8Ω speaker**, while keeping **THD < 1%** and ensuring **power efficiency**. The selected op-amp, **OPA564**, provided high output current (up to 1.5A), high slew rate (40V/µs), and wide voltage swing—all critical for efficient power delivery.

## 2. Circuit Design & Simulation

### **a. Schematic Design**

The amplifier circuit was designed in LTSpice using a custom OPA564 symbol to simulate all relevant behaviors, including **current limit** and **enable/disable** features.

{% include image-gallery.html images="power_schem.webp, schem.png" height="400" %}

- Supply: ±12V
- Load: 8Ω speaker
- Output swing: ~12Vpp
- Current limit set to ~1.7A

### **b. Power & Efficiency Analysis**

{% include image-gallery.html images="diss_pow.png, avg_pow.png" height="400" %}

- **Delivered power** to load: 8.25W avg  
- **Supply power draw**: 11.86W  
- **Efficiency**: ~69.6%

The amplifier achieved **71–72%** efficiency at peak operation and maintained decent performance across lower gains by adjusting supply voltage.


### **c. THD Performance**

{% include image-gallery.html images="thd_log.png" height="400" %}

- THD < 1% at full swing  
- Simulation confirmed stable, clean output under max power condition

## 3. Optimization Strategies

- **Supply voltage** tuned down to **±4.2V** for low-power mode
- Adjusted **feedback resistor (R2)** for variable gain
- Thermal performance validated with **heat sink selection** via datasheet calculations
- Simulations showed efficient behavior across multiple voltage/gain settings

## 4. PCB design

{% include image-gallery.html images="power_routing.png" height="400" %}


Carefully routed two-layer board with:

Wide power planes for current handling

Thermal pad under IC with copper pour and vias for heat dissipation

Compact through-hole terminal blocks for input/output/power

Silkscreen, layer stack, and trace widths were reviewed for manufacturability and performance.

## 5. Thermal & Hardware Considerations

Calculated thermal resistance to be around 13 C/W
Used **power pad** package with low thermal resistance
Included thermal vias and isolated anodized heatsink in layout
Verified against maximum PCB area constraints (16cm²)

## 6. Reflection

This project emphasized real-world constraints in power electronics design: **thermal management**, **efficiency tuning**, and **signal quality** under load. It balanced simulation with practical hardware considerations and validated all performance targets before PCB implementation.

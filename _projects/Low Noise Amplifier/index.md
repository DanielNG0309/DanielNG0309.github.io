---
layout: post
title: project title
description:  short description of the project
skills: 
- skill 1
- skill 2
main-image: /project.webp 
---

## 1. Objective & Design Context

{% include image-gallery.html images="power.png" height="400" %}

{% include image-gallery.html images="projector.png" height="400" %} 

This project aimed to build a bandpass amplifier with:

Bandwidth: 20 Hz – 20 kHz

Gain: 100× to 500×

THD < 0.5%

Power < 15 mW

We used LTspice for schematic design, simulation, and optimization. Once the design met performance targets, it was prototyped on a breadboard and tested with an oscilloscope and signal generator to verify frequency response, gain linearity, and signal distortion in real hardware.

## 2. Circuit Design Overview
The amplifier consists of three stages:

2nd-order Sallen-Key High-Pass Filter

3rd-order Butterworth Low-Pass Filter

Inverting Amplifier with variable gain

This configuration ensures a smooth passband, low noise, and adjustable gain across a wide frequency spectrum.

## 3. Key Components & Design
### **a. High-Pass Filter Stage**
Cutoff ~10 Hz to suppress flicker noise

Gain: ~21.6 dB

Sallen-Key topology

### **b. Low-Pass Filter Stage**
Cutoff ~20–30 kHz

No added gain to minimize THD

Combines active and passive filters

### **c. Inverting Amplifier**
Adjustable gain via potentiometer

Final gain range from 1x–5x

Optional offset tuning via resistor biasing

## 4. LTSPICE Simulation & Optimization
### **a. Frequency Response**

{% include image-gallery.html images="1st stage.png" height="400" %} 

**HPF Output (Stage 1)** – Designed with Chebyshev characteristics to suppress low-frequency flicker noise.

{% include image-gallery.html images="2nd stage.png" height="400" %}

**LPF Output (Stage 2)** – Butterworth filter ensures a flat response before gain stage.

### **b. Full System Gain Response**

{% include image-gallery.html images="100 gain.png" height="400" %} 

**Gain = 100**, 10 mVpp input → 1 Vpp output

{% include image-gallery.html images="500 gain.png" height="400" %}

**Gain = 500**, 2 mVpp input → 1 Vpp output

### **c. THD and Noise**

{% include image-gallery.html images="100 output.png, 100 rms noise.png" height="400" %}

**THD** and **Noise RMS** at **100** Gain

{% include image-gallery.html images="500 output.png, 500 rms noise.png" height="400" %}

**THD** and **Noise RMS** at **500** Gain

### **d. Power Consumption**

{% include image-gallery.html images="power.png" height="400" %}

Average power per amplifier is about 2.5 mW, so it makes 2.5*3 = **7.5 mW** of dissipated power for the whole design.

## 5. Breadboard Prototyping & Testing
### **a. Physical Build**
Assembled full 3-stage design on breadboard

Used ADA4084 op-amp for high linearity

Verified all capacitor and resistor values against simulation

### **b. Validation Tools**
Signal generator (sine sweep from 10 Hz – 50 kHz)

Digital oscilloscope for gain and clipping checks

Real-time measurements of output amplitude and phase response

### **c. Observations**
Passband closely matched simulation

Amplifier output remained linear under all tested gain settings

No oscillation or unexpected peaking observed

Minor component value tweaks were applied post-testing for optimal flatness

## 6. Reflection
This project reinforced the full cycle of design → simulate → prototype → validate, requiring both theoretical design via LTspice and practical skills in breadboarding, measurement, and debugging. It was a great demonstration of analog system integration and performance tuning.

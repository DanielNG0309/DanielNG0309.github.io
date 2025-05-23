---
layout: post
title: ARBox - Augmented Reality Sandbox
description:  An interactive sandbox developed for educational purposes, ARBox transforms real-world sand elevations into dynamic topographic maps using a time-of-flight depth camera and real-time projection. Designed as part of an academic engineering course, the project required practical integration of hardware and software. I contributed to system interfacing, sensor selection, and real-time image processing. I served as the technical lead, overseeing nearly all technical aspects of the system design.

skills: 
  - Embedded Linux (Raspberry Pi 4)
  - ToF sensor integration (ArduCam)
  - Real-time image processing (OpenCV, Python, C++)
  - Contour interpolation & visualization
  - 3D modelling (Fusion360) and 3D printing
  - Technical leadership and rapid prototyping

main-image: /ARBOx.jpg
---

<div style="width: 100%; max-width: 360px; margin: auto; text-align: center;">
  <div style="position: relative; padding-bottom: 177.78%; height: 0; overflow: hidden;">
    <iframe 
      src="https://www.youtube.com/embed/47oBgbT1ehI" 
      style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;" 
      frameborder="0" 
      allowfullscreen>
    </iframe>
  </div>
  <p style="margin-top: 0.5em; font-size: 0.95em; color: #555;">ARBox prototype (not final MVP)</p>
</div>

---

## 1. System Architecture

{% include image-gallery.html images="ToF.png, projector.png" height="400" %} 

Depth Sensing: Selected and integrated the Arducam Time-of-Flight (ToF) depth camera, considering resolution, range, and Raspberry Pi compatibility.


Processing Unit: Used a Raspberry Pi 4 for real-time image processing and hardware control.

Projection: Configured the DLP LightCrafter Display 2000 EVM projector with RGB565 DPI output, calibrated to match the sandbox dimensions.

---

## 2. Software & Image Processing

Wrote C++ code with OpenCV to process raw depth data into usable topographic visuals.

Implemented a full image pipeline: region-of-interest cropping, cubic interpolation, color mapping, median filtering, and positional recalibration.

Developed an auto-launch boot script for the Raspberry Pi to begin projection on power-up.

---

## 3. Hardware Interface & Integration

{% include image-gallery.html images="pcb.png" height="400" %} 

Designed the PCB schematic to streamline connections between the ToF camera, Raspberry Pi, and projector (routing handled by another teammate).

Prototyped and tested all subsystems individually before full system integration.

Calibrated the physical alignment of the camera and projector relative to the sandbox for accurate overlay.

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Power Budget**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The projector module draws 609 mA max with an average of about 470
mA.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The camera module has a 700 mA maximum current drawn listed on its
specification but on average draws about 500 mA in use

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The Raspberry Pi on average draws about 900 mA in use

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The total power budget is about 1.87 A * 5V = 9.35 W which is about ⅔ of what the
power supply can provide, leaving some room for further expansions or changes in the future
like adding an IR thermal camera


---

## 4. Prototyping & Enclosure

{% include image-gallery.html images="3d.png, frame.png" height="400" %} 

Built a wooden sandbox frame to match the projection’s 4:3 aspect ratio and height constraints of the depth sensor and projector.

Designed and 3D printed an adjustable enclosure for the PCB and components, enabling fine-tuned alignment.

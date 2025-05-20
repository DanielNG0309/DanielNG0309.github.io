---
layout: post
title: Agrisync - IoT Inventory Management System
description:  A start-up style capstone project developing a BLE-enabled IoT system for real-time monitoring of agricultural inventory. I led the hardware team, designing the custom PCB, analog front-end, and embedded firmware using Zephyr RTOS on an nRF52833 SoC. The project also involved business model development, customer discovery, and market validation, simulating a full product development cycle from concept to launch.
skills: 
  - PCB design(Altium)
  - Analog front-end design
  - BLE Configuration
  - Zephyr RTOS (Embedded C)
  - Rapid prototyping & testing
  - Market research & business planning
  - Technical Leadership

main-image: /agrisyncOverview.png
---


## 1. Problem & Concept
Traditional farm inventory methods (e.g., livestock feed, crop bins) are manual and error-prone, often relying on visual estimation. AgriSync aims to provide real-time, scalable, and wireless tracking of physical goods using load-sensing and BLE connectivity.

## 2. Hardware Design

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;### a. PCB design

{% include image-gallery.html images="pcb schem.png" height="400" %} 


The custom PCB for the AgriSync Smart Scale was designed in Altium and fabricated through JLCPCB, with PCB Assembly (PCBA) services utilized for the majority of surface-mounted components to ensure reliable assembly quality and to accelerate the prototyping process.


This was a two-layer PCB, measuring approximately 140 mm by 70 mm. Given that the final installation does not impose size constraints, a larger footprint was chosen to simplify routing and allow clear silkscreen labeling for debugging and assembly.


To support current demands on power lines and reduce resistive losses, main power traces were routed using 0.7 mm to 1.0 mm widths, with geometric power planes implemented where feasible. Signal and low-current digital traces were routed using 0.254 mm width, which provided a safe clearance margin while accommodating the dense pad spacing of the MCU, whose pads measure just 0.4 mm x 0.4 mm with 0.2 mm spacing between them. To ensure signal integrity and facilitate grounding across the board, ground polygon pours were included on both layers, ensuring a unified and low-impedance ground reference throughout the circuit.


The PCB primarily features surface-mount devices (SMDs), including the Nordic BLE module, ADCs, resistors, capacitors, voltage regulator, and instrumentation amplifiers. Through-hole components were limited to the 12V DC barrel jack input, an LED 7-segment display, and four unpopulated potentiometer footprints intended for optional tuning of amplifier gains. A custom library was made for the project, which includes all components and footprints, following the industry standard of having a dedicated library for a PCB project.

{% include image-gallery.html images="PCB routing.jpg" height="400" %} 


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;### b. Load cell


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;### c. In-Amp


### Header 3 
Use this to have subsection if needed


## Embedding images 
### External images
{% include image-gallery.html images="https://live.staticflickr.com/65535/52821641477_d397e56bc4_k.jpg, https://live.staticflickr.com/65535/52822650673_f074b20d90_k.jpg" height="400"%}
<span style="font-size: 10px">"Starship Test Flight Mission" from https://www.flickr.com/photos/spacex/52821641477/</span>  
You can put in multiple entries. All images will be at a fixed height in the same row. With smaller window, they will switch to columns.  

### Embeed images
{% include image-gallery.html images="project2.jpg" height="400" %} 
place the images in project folder/images then update the file path.   


## Embedding youtube video
The second video has the autoplay on. copy and paste the 11-digit id found in the url link. <br>
*Example* : https://www.youtube.com/watch?v={**MhVw-MHGv4s**}&ab_channel=engineerguy
{% include youtube-video.html id="MhVw-MHGv4s" autoplay= "false"%}
{% include youtube-video.html id="XGC31lmdS6s" autoplay = "true" %}

you can also set up custom size by specifying the width (the aspect ratio has been set to 16/9). The default size is 560 pixels x 315 pixels.  

The width of the video below. Regardless of initial width, all the videos is responsive and will fit within the smaller screen.
{% include youtube-video.html id="tGCdLEQzde0" autoplay = "false" width= "900px" %}  

<br>

## Adding a hozontal line
---

## Starting a new line
leave two spaces "  " at the end or enter <br>

## Adding bold text
this is how you input **bold text**

## Adding italic text
Italicized text is the *cat's meow*.

## Adding ordered list
1. First item
2. Second item
3. Third item
4. Fourth item

## Adding unordered list
- First item
- Second item
- Third item
- Fourth item

## Adding code block
```ruby
def hello_world
  puts "Hello, World!"
end
```

```python
def start()
  print("time to start!")
```

```javascript
let x = 1;
if (x === 1) {
  let x = 2;
  console.log(x);
}
console.log(x);

```

## Adding external links
[Wikipedia](https://en.wikipedia.org)


## Adding block quote
> A blockquote would look great if you need to highlight something


## Adding table 

| Header 1 | Header 2 |
|----------|----------|
| Row 1, Col 1 | Row 1, Col 2 |
| Row 2, Col 1 | Row 2, Col 2 |

make sure to leave aline betwen the table and the header



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


### ***a. PCB design***


{% include image-gallery.html images="pcb schem.png" height="400" %} 


The **custom PCB** for the AgriSync Smart Scale was designed in **Altium** and fabricated through **JLCPCB**, with PCB Assembly (PCBA) services utilized for the majority of surface-mounted components to ensure reliable assembly quality and to accelerate the prototyping process.


This was a **two-layer PCB**, measuring approximately **140 mm** by **70 mm**. Given that the final installation does not impose size constraints, a larger footprint was chosen to simplify routing and allow clear silkscreen labeling for debugging and assembly.


To **support current demands** on power lines and **reduce resistive losses**, main power traces were routed using **0.7 mm** to **1.0 mm** widths, with **geometric power planes** implemented where feasible. Signal and low-current digital traces were routed using **0.254 mm** width, which provided a **safe clearance margin** while accommodating the dense pad spacing of the MCU, whose pads measure just **0.4 mm** x **0.4 mm** with **0.2 mm** spacing between them. To ensure **signal integrity** and facilitate grounding across the board, **ground polygon pours** were included on both layers, ensuring a unified and **low-impedance ground reference** throughout the circuit.


The PCB primarily features **surface-mount devices** (SMDs), including the **Nordic BLE module**, **ADCs**, **resistors**, **capacitors**, **voltage regulator**, and **instrumentation amplifiers**. Through-hole components were limited to the **12V DC barrel jack** input, an **LED 7-segment display**, and four unpopulated **potentiometer** footprints intended for **optional tuning** of amplifier gains. A **custom library** was made for the project, which includes all components and footprints, following the **industry standard** of having a dedicated library for a PCB project.

{% include image-gallery.html images="PCB routing.jpg" height="400" %} 


### ***b. Load cell***


The AgriSync system uses four **1000 kg** load cells to measure weight, chosen for their durability and precision in harsh agricultural environments. Key specifications:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Rated Output: **2.0 ± 0.004 mV/V**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Excitation Voltage: **10V** nominal (maximum 15V AC/DC)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Operating Temperature: **-30°C** to **+65°C** — suitable for outdoor farm environments

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Environmental Protection: **IP67** — dust-tight and water-resistant for rugged field use

With a **12V excitation**, the theoretical maximum output from each load cell is approximately **24 mV** (2 mV/V × 12 V). The actual sensitivity tested is about **1.2 mV/W** which would convert to a maximum of **14.4 mV**, requiring **amplification** before analog-to-digital conversion.


### ***c. In-Amp***


An **INA826** instrumentation amplifier is used for each load cell to amplify the weak differential signal. With a **gain resistor** set to **250 Ω**, each amplifier achieves a gain of approximately **250x**, bringing the signal into a readable range for the ADC. Key reasons for this selection include:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; High **Common-Mode Rejection Ratio** (CMRR) – essential for eliminating the 6V common mode voltage in the load cell outputs.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Wide **supply voltage** range (>15V), compatible with the 12V rail used in the system.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Compact** and **cost-effective** solution with **high linearity** and **low offset**.

Each load cell has a dedicated **INA826** amplifier to ensure **isolated** and **accurate signal conditioning**. 


### ***d. Analog to Digital Converter*** 


The amplified analog signals are fed into two NAU7802 24-bit ADCs. Each ADC supports two channels, enabling it to process signals from two load cells. The reasons for choosing the NAU7802 include:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; High-resolution **24-bit** conversion, critical for detecting small weight variations.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Better **accuracy** and **signal integrity** compared to the internal 12-bit ADCs in typical microcontrollers.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **I²C** communication, simplifying integration with the BLE controller module.


### ***e. BLE Controller Module***


The system uses the **BL653u** module featuring the **Nordic nRF52833 SoC**, a **Bluetooth 5.1**-enabled microcontroller. Nordic was selected for its strong track record in **BLE** performance and its **robust development ecosystem**. Key benefits include:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Integrated **BLE stack** with support for **custom GATT services**, enabling structured communication with external devices.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Reliable **BLE connectivity** with long-range support and stable performance, well-suited for unpredictable farm environments.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Extensive **development tools** and **community support**, simplifying firmware development and debugging.


The controller handles all **BLE communication** with a **mobile application**, enabling **real-time** transmission of load cell data to the user interface. 


### ***f. Power Supply***


A **dual-rail** power supply configuration powers the AgriSync system:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Primary Supply**: **12V DC** input derived from standard AC sources **(100–240V, 50/60 Hz)**, making it compatible with global power 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; standards, including **Canada’s 120V/60Hz** and **Brazil’s 127V** or **220V/60Hz**. This rail powers the **load cells** and **INA826** 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; instrumentation amplifiers.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **3.3V Regulated Rail**: A **L78L33ABUTR** linear voltage regulator steps down **12V** to **3.3V** to power the controller and other low-voltage

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; digital components. This rail powers the **BL653u** module and **NAU7802** ADCs.

### ***g. 7-segment display***

The system includes a **single-digit 7-segment display** used for scale identification and on-site debugging.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Scale Identification**: Displays the unique **scale number** to distinguish between multiple deployed systems.


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Debugging Aid**: Provided real-time feedback during development and testing, such as status codes and sensor activity indicators.

## 3. Firmware & Embedded Systems

To implement the design the firmware was created with these key components:

The **nrf52833** chip utilizes **Zephyr RTOS** as its main development tool. So, a **custom board file** was created for this implementation with both **I2C channels**, **custom pin mapping**, and **configuration settings** necessary for **BLE** enabled. **Drivers** for the **NAU7802** were created based on its datasheet for initial setup, data collection, and commands.

```c++
 void i2c_setup(const struct i2c_dt_spec *dev_i2c){
	 i2c_reg_write_byte_dt(dev_i2c,NAU7802_PU_CTRL,0x01);
	 i2c_reg_write_byte_dt(dev_i2c,NAU7802_PU_CTRL,0x02);
	 k_msleep(1);
	 i2c_reg_write_byte_dt(dev_i2c,NAU7802_I2C,0x01);
	 i2c_reg_write_byte_dt(dev_i2c,NAU7802_ADC,0x30);
	 i2c_reg_write_byte_dt(dev_i2c,NAU7802_PGA,0x11);
	 i2c_reg_write_byte_dt(dev_i2c,NAU7802_PU_CTRL,0x16);
 }
```

A **GATT service** was set up to handle **BLE communication** between the **scale** and the **mobile app** with **2 characteristics**, one dedicated to weight change information and one for other communications needs (data integrity check, commands, etc,...). Connection and advertising parameters were set to a **minimum interval** of **50 ms** and a **maximum interval** of **100 ms**. **MTU** (Maximum Transmission Unit) was also set to the max of **247 bytes** instead of the default **23 bytes**. These changes ensure fast and seamless connection and data transfer between the scale and mobile app.

```c
BT_GATT_SERVICE_DEFINE(
	 my_agrisync_svc, 
	 BT_GATT_PRIMARY_SERVICE(BT_UUID_AGRISYNC_SERV),
	 // Weight update characteristic (notify only)
	 BT_GATT_CHARACTERISTIC(BT_UUID_WEIGHT_UPDATE_CHRC, BT_GATT_CHRC_NOTIFY, BT_GATT_PERM_NONE, NULL, NULL, NULL),
	 BT_GATT_CCC(weight_update_cfg_changed, BT_GATT_PERM_READ | BT_GATT_PERM_WRITE),
	 // Control characteristic for everything else (write and notify)
	 BT_GATT_CHARACTERISTIC(BT_UUID_INFO_EXCHANGE_CHRC, BT_GATT_CHRC_WRITE | BT_GATT_CHRC_NOTIFY, BT_GATT_PERM_READ | BT_GATT_PERM_WRITE, NULL, combined_write, NULL),
	 BT_GATT_CCC(info_exchange_cfg_changed, BT_GATT_PERM_READ | BT_GATT_PERM_WRITE)
 );
```

```c
CONFIG_BT_L2CAP_TX_MTU=247
CONFIG_BT_BUF_ACL_RX_SIZE=251
```

```c
static const struct bt_le_adv_param *adv_param = BT_LE_ADV_PARAM((BT_LE_ADV_OPT_CONNECTABLE | BT_LE_ADV_OPT_USE_IDENTITY), 50,100,NULL);
```

```c
static const struct bt_le_conn_param *conn_param = BT_LE_CONN_PARAM(50, 100, 0, 1000);
```

A **state machine** configuration was used to gather the data from the 2 **ADCs** and 4 **load cells**. The **first state** is to **wait for the data to be ready** and read from load cells **1** and **3** (both connected to the internal MUX line 1 of each of the 2 ADCs). When both load cells **1** and **3** have been **read**, **MUX line 2** will be **selected**, and a predetermined **wait time** (400 ms) will be implemented for the **MUXs to settle**. Data will be then read from load cells **2** and **4** (both connected to MUX line 2) when it is **ready**. Once all **4 load cells are read**, the **sum** of them will be sent back to the main loop, **MUX line 1** will be selected, and the system will wait for another **400 ms**.

```c
uint64_t adc_data_process(state_t *state, const struct i2c_dt_spec *dev_i2c1, const struct i2c_dt_spec *dev_i2c2) {
	static bool ready_1 = false, ready_2 = false, ready_3 = false, ready_4 = false;
	uint64_t adc_total = 0;
	uint8_t ADC_val[3];  
 
	switch (*state) {
		 case READ_LOAD_CELL_1_3:
			if (adc1_ready && !ready_1) {
				i2c_burst_read_dt(dev_i2c1, NAU7802_ADCO_B2, ADC_val, sizeof(ADC_val));
				load_cell_1 = ADC_val[0] * 256 * 256 + ADC_val[1] * 256 + ADC_val[2];
				ready_1 = true;
				
			}
			if (adc2_ready && !ready_3) {
				i2c_burst_read_dt(dev_i2c2, NAU7802_ADCO_B2, ADC_val, sizeof(ADC_val));
				load_cell_3 = ADC_val[0] * 256 * 256 + ADC_val[1] * 256 + ADC_val[2];
				ready_3 = true;
			}
			if (ready_1 && ready_3) {
				*state = SWITCH_TO_MUX_2;
			}
			break;
 
		case SWITCH_TO_MUX_2:
			i2c_reg_write_byte_dt(dev_i2c1, NAU7802_CTRL2, 0x80);
			i2c_reg_write_byte_dt(dev_i2c2, NAU7802_CTRL2, 0x80);
			k_msleep(400);
			*state = READ_LOAD_CELL_2_4;
			break;
 
		case READ_LOAD_CELL_2_4:
			if (adc1_ready && !ready_2) {
				i2c_burst_read_dt(dev_i2c1, NAU7802_ADCO_B2, ADC_val, sizeof(ADC_val));
				load_cell_2 = ADC_val[0] * 256 * 256 + ADC_val[1] * 256 + ADC_val[2];
				ready_2 = true;
			}
			if (adc2_ready && !ready_4) {
				i2c_burst_read_dt(dev_i2c2, NAU7802_ADCO_B2, ADC_val, sizeof(ADC_val));
				load_cell_4 = ADC_val[0] * 256 * 256 + ADC_val[1] * 256 + ADC_val[2];
				ready_4 = true;
			}
			if (ready_2 && ready_4) {
				*state = RETURN_DATA;
			}
			break;
 
		case SWITCH_TO_MUX_1:
			i2c_reg_write_byte_dt(dev_i2c1, NAU7802_CTRL2, 0x00);
			i2c_reg_write_byte_dt(dev_i2c2, NAU7802_CTRL2, 0x00);
			k_msleep(400);
			*state = READ_LOAD_CELL_1_3;
			break;
 
		case RETURN_DATA:
			if (ready_1 && ready_2 && ready_3 && ready_4) {
				adc_total = load_cell_1 + load_cell_2 + load_cell_3 + load_cell_4;
				ready_1 = ready_2 = ready_3 = ready_4 = false;  // Reset flags
				*state = SWITCH_TO_MUX_1;  // Restart state machine
				return adc_total;
			}
			break;
	}
 
	return 0;  // Default return if no data is ready
}
```

To **smooth out the data**, multiple options were considered including **leaky integrator, moving average, and Kalman filter**. The **moving average** method was chosen at the end because of its **ease of implementation**, **decent performance**, and relatively **less resource-intensive** requirement. Raw data coming from the state machine was stored in a **cyclical buffer** continuously with **6 entries**. The **sum of the data** was then **averaged out**, giving a smooth and stable output. **Thresholds** in both **values** and **time** were also implemented to **ignore small and momentary changes** 

```c
weight_buff_sum +=data_kg;
			weight_buff_sum -=raw_data_buff[(raw_data_index)%MOV_AVG_SIZE];
			raw_data_buff[raw_data_index%MOV_AVG_SIZE] = data_kg;
			raw_data_index++;
			moving_avg = weight_buff_sum/MOV_AVG_SIZE;
			data_kg_current = moving_avg;
			diff = fabsf(data_kg_current - data_kg_prev);
			
			
			if(diff>big_threshold){
				stable_cycle = 0;
				data_kg_prev = data_kg_current;
				big_change = true;
				
			}
			if(diff<small_threshold&&big_change&&(fabsf(data_kg_current-last_weight_saved)>=small_threshold)){
				stable_cycle++;
			}
			else{
				stable_cycle = 0;
			}	
			// Only store the moving average if the change is significant and sustained
			if (stable_cycle >= hold_cycle) {
				weight_buff[data_index] = moving_avg;  // Store the filtered moving average
				last_weight_saved = weight_buff[data_index];
				time_buff[data_index] = k_uptime_seconds();
				stable_cycle = 0;
				big_change = false;
				data_index++;
			}
			if(data_index>=MAX_ENTRIES){
				data_index = 0;
			}
```



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



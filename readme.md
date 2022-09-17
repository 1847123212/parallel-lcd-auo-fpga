# Parallel-Interface LCD Driver for AUO A030DW02 V0 using FPGA

Caution: this project is in a working process and not been done. Part of the document has not finished.
<br>
<br>
<br>
<br>
<br>
<br>
This project is managing to drive LCD(AUO A030DW02 V0, Part-No. 59.03A33.001) using Altera Cyclone IV EP4CE10F17C8N FPGA.

Author: jlywxy (jlywxy@outlook.com)<br>
Document Version: 1.0
- --

## Overview of the LCM Interface 

### 1. Display Interface

This LCM supports different interfaces: UPS051/052(8-bit RGB), CCIR656, YUV720/640.
```
AUO - A030DW02 V0 (Part-No. 59.03A33.001)

Electrical-Level       | Speed         | Wires
--------------------------------------------------
3.3v-TTL with 3.3v VDD | 24.535/27 MHz | SPI+[HSYNC+VSYNC+CLK+DATA8]

```
* This display will not use DE signal.

### 2. Backlight Interface

This LCM requires 9.6v single power with 20mA current with selectable internal backlight driver.

### 3. Connector

The part number of Mating connector is not shown in specification sheet. This project is using AYF333935 of Panasonic connector, which has 39pin with crossed 0.3mm interval.
```
1 3 5 7 9       insert direction: v
❚|❚|❚|❚|❚ ...   each row has 0.6mm interval
|❚|❚|❚|❚ ...
 2 4 6 8 
```
- --

## Availability Test
1. Since this LCM need external capacitor connected, a PCB is made to convert connector to FPGA, include power management.

< TO BE CONTINUED >

2. The test method of availability is to show color flow gradient animation.

< TO BE CONTINUED >

- --

## Display Workflow(Steps to light up display)

0. Backlight power on.
1. LCM VDD on, Reset.
2. Initialize LCM with commands.
3. Start data transmission.

- --
## Misc

### LCM Optical Characteristics

```
AUO - A030DW02 V0 (Part-No. 59.03A33.001)

Pixel-Arrangement  | Panel-Type | Color-Depth
-------------------------------------------------
RGB delta          | TN         | 8-bit(16.2M)


Contrast | Color-Chromaticity | Backlight
-------------------------------------------------
300:1    | unknown            | 500 nits
```

### Knowledge Bases of Concepts
1. AUO UPS051/052 YUV720/640 CCIR656

< TO BE CONTINUED >

2. MIPI DPI

* The MIPI Alliance defines modern interface of mobile devices like phones, including display, cameras, etc. 
* MIPI-DPI is one of the MIPI display interface series, which is well known as RGB/Parallel/LTDC interface. This interface splits control lines(HSYNC/VSYNC/DE) with data lines(RGB parallel lines). Since it uses single-ended signals(compared to MIPI-DSI), the max speed(clock speed) could be limited, but it can transfer full pixel data in one clock period(compared to serial interfaces). The color depth is configurable as RGB565/RGB666/RGB888 and more, which could also be 'hacked' to leave out some pins or branch some lines(when downsampling color depth, throw away certain LSB; when upsampling, branch certain MSB to LSB or connect certain LSB to GND).
```
RGB888 (typical format of 16.7M color display)
-------------------------------------------------
       RRRRRRRR GGGGGGGG BBBBBBBB (3 bytes)

RGB666 (typical format of 262k color display, and 16.2M color TN panels with FRC)
-------------------------------------------------
             RR RRRRGGGG GGBBBBBB (18 bits)

RGB565
-------------------------------------------------
                RRRRRGGG GGGBBBBB (2 bytes)

RGB101010 (not available in most of the displays, typical format of 1.07B color screen)
-------------------------------------------------
RRRRRR RRRRGGGG GGGGGGBB BBBBBBBB (30 bits)
```

4. LCM

* A abbreviation of Liquid Crystal Module, which includes LCD glass panel and backlight LEDs.


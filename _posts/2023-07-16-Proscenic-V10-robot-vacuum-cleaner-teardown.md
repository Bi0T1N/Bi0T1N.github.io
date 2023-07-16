---
layout: default
title:  "Teardown of robot vacuum cleaner Proscenic V10"
categories: [proscenic, proscenic v10, floobot v10, teardown, robot vacuum cleaner, technical details]
---

# {{ page.title }}

## Introduction

The robot vacuum cleaner V10 is the "little brother" of the X1 from Proscenic. It comes without the cleaning station but also supports ultrasonic mopping and that for a small budget.

## Teardown

Since it's interesting ( and because I can ;-) ) I opened up my new Proscenic V10 to see how a robot vacuum cleaner is built. I also wanted to get more information on what hardware ([SoC](https://en.wikipedia.org/wiki/System_on_a_chip), [LiDAR](https://en.wikipedia.org/wiki/Lidar), etc.) is being used. My findings are presented and described below.

<!-- taken from https://tamarisk.it/automatically-numbering-figures-in-markdown/ -->
<style>
    /* initialise the counter */
    body { counter-reset: figureCounter; }
    /* increment the counter for every instance of a figure even if it doesn't have a caption */
    figure { counter-increment: figureCounter; }
    /* prepend the counter to the figcaption content */
    figure figcaption:before {
        content: "Fig " counter(figureCounter) ": "
    }
</style>

#### Robot

<figure id="bottom_view">
	<img src="/images/proscenic/bottom_view.jpg" alt="Bottom of the robot with description">
	<figcaption>Bottom side of the robot with description of parts</figcaption>
</figure>
<figure id="top_view">
	<img src="/images/proscenic/top_view.jpg" alt="Top of the robot with description but without cover">
	<figcaption>Top side of the robot with description but without cover</figcaption>
</figure>
<figure id="inner_top_view">
	<img src="/images/proscenic/inner_top_view.jpg" alt="Top view of the inner life">
	<figcaption>Top view of the inner life</figcaption>
</figure>
<figure id="type_plate">
	<img src="/images/proscenic/type_plate.jpg" alt="Type plate of the robot">
	<figcaption>Type plate of the robot</figcaption>
</figure>
<figure id="side_brush">
	<img src="/images/proscenic/side_brush.jpg" alt="Side brush with additional brush to clean the housing">
	<figcaption>Side brush with additional brush to clean the housing</figcaption>
</figure>

#### Battery

Contrary the manufacturer's specification the battery only has a capacity of 2600mAh instead of 3200mAh.
<figure id="battery">
	<img src="/images/proscenic/battery.png" alt="Lithium-ion Battery with nominal 14.4V and 2600mAh">
	<figcaption>Lithium-ion Battery with nominal voltage of 14.4V and nominal capacity of 2600mAh</figcaption>
</figure>
<figure id="battery_position">
	<img src="/images/proscenic/battery_position.jpg" alt="Position of the battery">
	<figcaption>Battery position with enough space for a larger battery</figcaption>
</figure>

#### Mainboard

<figure id="mainboard_top_view">
	<img src="/images/proscenic/mainboard_top_view.jpg" alt="Mainboard top view">
	<figcaption>Top view of the mainboard</figcaption>
</figure>
<figure id="sigmastar_processor_uascent_wifi_bluetooth">
	<img src="/images/proscenic/processor.jpg" alt="processor and network module">
	<figcaption>SigmaStar processor and Uascent WiFi/Bluetooth module</figcaption>
</figure>
<figure id="on_off_mainboard_detailed">
	<img src="/images/proscenic/onoff_home_button1.jpg" alt="on-off and home button mainboard detailed">
	<figcaption>Top view of the on-off and home button mainboard</figcaption>
</figure>
<figure id="on_off_mainboard">
	<img src="/images/proscenic/onoff_home_button_and_debug.jpg" alt="on-off and home button mainboard top view">
	<figcaption>Top view of the on-off/home button mainboard and easily accessible debug port</figcaption>
</figure>

#### SoC

<figure id="cpu_color">
	<img src="/images/proscenic/arm_cpu.jpg" alt="Picture of the ARM CPU with company logo and model number">
	<figcaption>ARM CPU from SigmaStar</figcaption>
</figure>
<figure id="cpu_information1">
	<img src="/images/proscenic/processor1.jpg" alt="Picture with inverted color to show the model number of the CPU">
	<figcaption>SigmaStar SSD222D</figcaption>
</figure>
<figure id="cpu_calendar_week">
	<img src="/images/proscenic/processor2.jpg" alt="Calendar week as date of manufacture">
	<figcaption>SigmaStar SSD222D, which was manufactured in calendar week 36 in 2021</figcaption>
</figure>

#### Network

<figure id="network_module">
	<img src="/images/proscenic/network_module.jpg" alt="Combined WiFi Bluetooth module">
	<figcaption>Combined WiFi and Bluetooth module UAW6158B from Shenzhen Uascent Technology</figcaption>
</figure>

#### Debug port

<figure id="debug_port">
	<img src="/images/proscenic/debug_port.jpg" alt="debug port">
	<figcaption>9-pin debug port</figcaption>
</figure>

#### LiDAR

<figure id="lidar">
	<img src="/images/proscenic/lidar.jpg" alt="LiDAR positioned in front of the robot">
	<figcaption>LiDAR positioned in front of the robot</figcaption>
</figure>
<figure id="lidar_detailed">
	<img src="/images/proscenic/lidar_detailed.jpg" alt="Detailed picture of LiDAR with visible logo">
	<figcaption>Close-up of LiDAR and circuit board</figcaption>
</figure>

#### Warranty seal
Fun fact: the warranty seal is still intact after my teardown because you don't have to loosen this screw to open it. This only makes the removal of the housing a little more time-consuming.

#### Detailed mainboard pictures
![Detailed mainboard image](/images/proscenic/mainboard_detailed_1.jpg)
![Detailed mainboard image](/images/proscenic/mainboard_detailed_2.jpg)
![Detailed mainboard image](/images/proscenic/mainboard_detailed_3.jpg)

## Technical details

### System on a chip (SoC)

The pictures reveal that the robot uses a [SigmaStar SSD222D](#soc) SoC with an ARM CPU. A [Preliminary Product Brief](https://web.archive.org/web/20230715221211/http://linux-chenxing.org/misc/SSD222.pdf) for the SSD222 series is available and in there it lists the SoC as "Smart Display CAM Controller" and mentions the CPU is an ARM Cortex-A7 dual-core and that up to 512MB DDR3 RAM is supported. The [linux-chenxing](https://web.archive.org/web/20230715223416/http://linux-chenxing.org/) organization tries to provide Linux for weird Cortex A7 based SoCs from MStar/SigmaStar.  
The [SigmaStar](https://web.archive.org/web/20230715220859/http://www.sigmastarsemi.com/) company [lists](https://web.archive.org/web/20230715221635/https://www.comake.online/index.php?p=products_show&lanmu=2&c_id=6&id=44) that the SSD222D is a ["Smart Display CAM Controller with Embedded 16-bit 128MB DDR3"](https://web.archive.org/web/20220626103145/https://comake.online/uploadfile/file/20220419/20220419022157_42191.pdf) that uses an ARM Cortex-A7 dua-core CPU with an frequency up to 1GHz. It provides an memory management unit (MMU) for Linux support, has a dedicated image/video processor, JPEG encoder, ethernet, some cryptography algorithms, secure boot and multiple peripherals like General Purpose Input/Output (GPIO), Inter-Integrated Circuit (I²C) and Pulse-width modulation (PWM). Another [technical product brief](https://web.archive.org/web/20230716195244/https://comake.online/uploadfile/file/20220406/20220406023422_88086.pdf) provides information about the pin diagram, signal descriptions, electrical specifications and marking information. Additional information missing in the technical product brief are available in a [blog post](https://web.archive.org/web/20230716195444/https://blog.csdn.net/weixin_44281973/article/details/118692027).

The SSD222D might be similar to the [SSD202D](https://web.archive.org/web/20230716193129/https://linux-chenxing.org/infinity2/SSD202D_pb_S_v01.pdf) which uses the same ARM processor, just with a smaller cache. And instead of the image/video processor it has dedicated H.264/AVC and H.265/HEVC decoder. In addition to [linux-chenxing's Linux fork](https://web.archive.org/web/20230716193723/https://github.com/linux-chenxing/linux) with [support for SSD202D](https://web.archive.org/web/20230716195612/https://www.cnx-software.com/2021/01/29/sigmastar-ssd201-ssd202-powered-4g-lte-industrial-gateway-made-to-run-mainline-linux/), an [SigmaStar original Buildroot SDK Boot Kernel](https://web.archive.org/web/20230716193338/https://github.com/DongshanPI/Buildroot_SigmastarOriginalSDK) is available.  
SigmaStar provides [SigmaStarDocs](https://web.archive.org/web/20230716200409/https://wx.comake.online/doc/doc/SigmaStarDocs-SSD220-SIGMASTAR-202305231834/) as a static HTML site for developer documentation.

### Network

The networking capabilities are provided via a [Wi-FI and Bluetooth low energy chip](https://web.archive.org/web/20230716201959/https://fccid.io/2A68EJX-UAW6158B0/User-Manual/user-manual-5956644.pdf) with the model number UAW6158B, FCC (Federal Communications Commission) ID is [2A68EJX-UAW6158B0](https://web.archive.org/web/20230716201234/https://fccid.io/2A68EJX-UAW6158B0), from Shenzhen Uascent Technology Co.,Ltd.

## Conclusion

The teardown of the Proscenic Floobot V10 is not particularly complicated and the robot uses almost exclusively Chinese hardware. However, my teardown has brought to light some interesting technical details that were not available before. The battery capacity is smaller than stated but could also be much larger as the space is there. The warranty seal is just a joke, but since it's a Chinese product, I doubt you'll get a warranty at all. The debug port of the robot is easily accessible.  
The final question is whether it is possible to run your own Linux on it? Time will tell.

## Sources

[Floobot V10 - Proscenic](https://web.archive.org/web/20230610210700/https://www.proscenic.com/de/product/staubsauger/floobot-v10/)  
[Proscenic V10 3000Pa Suction Robot Vacuum Cleaner](https://web.archive.org/web/20230716202630/https://www.geekmaxi.com/en/robot-vacuum-cleaner-proscenic-v10-3000pa-suction-robot-vacuum-cleaner-floor-mopping-240ml-dust-bin-3165.html)  
[Floobot X1 - Proscenic](https://web.archive.org/web/20230716202320/https://www.proscenic.com/uk/product/vacuums/pay-1-to-pre-order-floobot-x1-save-50-2/)  

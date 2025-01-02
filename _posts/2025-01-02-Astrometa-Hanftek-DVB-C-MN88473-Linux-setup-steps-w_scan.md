---
layout: default
title:  "Setting up the Panasonic MN88473 of an Astrometa USB stick for DVB-C under Linux"
categories: [Astrometa, Hanftek, dvb-c, MN88473, setup, Linux, w_scan]
---

# Setting up the Panasonic MN88473 of an Astrometa USB stick for DVB-C under Linux

## Introduction
My [Astrometa USB stick](https://www.linuxtv.org/wiki/index.php/Astrometa_DVB-T2) is the 2014 revision with "DVB-T/T2/C+FM+DAB" text on the stick and it supports DVB-C, DVB-T and DVB-T2. It came with some additional hardware (e.g. antenna and remote control) as shown in the following picture.

<img src="/images/astrometa_2014_version.jpg" width="50%" alt="Astrometa USB stick - Revision 2014"/>

The components of the Astrometa device are shown in the following table:

| Component Type    | Component Name |
| -------- | ------- |
| USB Bridge | Realtek RTL2832P |
| Demodulator | Panasonic MN88473 |
| Tuner | Rafael Micro R828D |

Once the USB stick is plugged into the computer, the device identifies as a "HanfTek Astrometa DVB-T/T2/C FM & DAB receiver [RTL2832P]". This can be seen in the output from the `lsusb` command below.
```text
ubuntu@homeserver:~$ lsusb
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 0000:0538   USB OPTICAL MOUSE
Bus 001 Device 003: ID 15f4:0131 HanfTek Astrometa DVB-T/T2/C FM & DAB receiver [RTL2832P]
Bus 001 Device 004: ID 8564:1000 Transcend Information, Inc. JetFlash
Bus 001 Device 005: ID 046a:c098 CHERRY CHERRY Corded Device
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
```

Detailed `lsusb` information are shown below:
```text
ubuntu@homeserver:~$ lsusb -vvv
Bus 001 Device 006: ID 15f4:0131 HanfTek Astrometa DVB-T/T2/C FM & DAB receiver [RTL2832P]
Couldn't open device, some information will be missing
Device Descriptor:
  bLength                18
  bDescriptorType         1
  bcdUSB               2.00
  bDeviceClass            0 [unknown]
  bDeviceSubClass         0 [unknown]
  bDeviceProtocol         0 
  bMaxPacketSize0        64
  idVendor           0x15f4 HanfTek
  idProduct          0x0131 Astrometa DVB-T/T2/C FM & DAB receiver [RTL2832P]
  bcdDevice            1.00
  iManufacturer           1 astrometadvbt2
  iProduct                2 dvbt2
  iSerial                 0 
  bNumConfigurations      1
  Configuration Descriptor:
    bLength                 9
    bDescriptorType         2
    wTotalLength       0x0022
    bNumInterfaces          2
    bConfigurationValue     1
    iConfiguration          4 
    bmAttributes         0x80
      (Bus Powered)
    MaxPower              500mA
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        0
      bAlternateSetting       0
      bNumEndpoints           1
      bInterfaceClass       255 Vendor Specific Class
      bInterfaceSubClass    255 Vendor Specific Subclass
      bInterfaceProtocol    255 Vendor Specific Protocol
      iInterface              5 
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x81  EP 1 IN
        bmAttributes            2
          Transfer Type            Bulk
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0200  1x 512 bytes
        bInterval               0
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        1
      bAlternateSetting       0
      bNumEndpoints           0
      bInterfaceClass       255 Vendor Specific Class
      bInterfaceSubClass    255 Vendor Specific Subclass
      bInterfaceProtocol    255 Vendor Specific Protocol
      iInterface              5
```

And the `dmesg` log output is the following:
```text
[  248.896665] usb 1-5: new high-speed USB device number 6 using xhci_hcd
[  249.038101] usb 1-5: New USB device found, idVendor=15f4, idProduct=0131, bcdDevice= 1.00
[  249.038122] usb 1-5: New USB device strings: Mfr=1, Product=2, SerialNumber=0
[  249.038131] usb 1-5: Product: dvbt2
[  249.038138] usb 1-5: Manufacturer: astrometadvbt2
[  249.046567] usb 1-5: dvb_usb_v2: found a 'Astrometa DVB-T2' in warm state
[  249.204312] usb 1-5: dvb_usb_v2: will pass the complete MPEG2 transport stream to the software demuxer
[  249.204358] dvbdev: DVB: registering new adapter (Astrometa DVB-T2)
[  249.204368] usb 1-5: media controller created
[  249.205162] dvbdev: dvb_create_media_entity: media entity 'dvb-demux' registered.
[  249.212300] i2c i2c-1: Added multiplexed i2c bus 2
[  249.212303] rtl2832 1-0010: Realtek RTL2832 successfully attached
[  249.218108] mn88473 1-0018: Panasonic MN88473 successfully identified
[  249.218122] usb 1-5: DVB: registering adapter 0 frontend 0 (Realtek RTL2832 (DVB-T))...
[  249.218127] dvbdev: dvb_create_media_entity: media entity 'Realtek RTL2832 (DVB-T)' registered.
[  249.218188] usb 1-5: DVB: registering adapter 0 frontend 1 (Panasonic MN88473)...
[  249.218191] dvbdev: dvb_create_media_entity: media entity 'Panasonic MN88473' registered.
[  249.218227] r820t 2-003a: creating new instance
[  249.225164] r820t 2-003a: Rafael Micro r820t successfully identified, chip type: R828D
[  249.225174] r820t 2-003a: attaching existing instance
[  249.230053] r820t 2-003a: Rafael Micro r820t successfully identified, chip type: R828D
[  249.232003] rtl2832_sdr rtl2832_sdr.1.auto: Registered as swradio0
[  249.232006] rtl2832_sdr rtl2832_sdr.1.auto: Realtek RTL2832 SDR attached
[  249.232008] rtl2832_sdr rtl2832_sdr.1.auto: SDR API is still slightly experimental and functionality changes may follow
[  249.242314] Registered IR keymap rc-astrometa-t2hybrid
[  249.242359] rc rc0: Astrometa DVB-T2 as /devices/pci0000:00/0000:00:14.0/usb1/1-5/rc/rc0
[  249.242476] rc rc0: lirc_dev: driver dvb_usb_rtl28xxu registered at minor = 0, raw IR receiver, no transmitter
[  249.242530] input: Astrometa DVB-T2 as /devices/pci0000:00/0000:00:14.0/usb1/1-5/rc/rc0/input20
[  249.242785] usb 1-5: dvb_usb_v2: schedule remote query interval to 200 msecs
[  249.251271] usb 1-5: dvb_usb_v2: 'Astrometa DVB-T2' successfully initialized and connected
```

## Scanning frequencies with `w_scan` program
The command line utility `w_scan` is used to perform frequency scans for DVB transmissions.  
The output of the [`w_scan` command](https://manpages.ubuntu.com/manpages/noble/man1/w_scan.1.html) only detects two DVB-T devices on the Astrometa USB stick, both frontends are detected as "TERRESTRIAL".

```text
ubuntu@homeserver:~$ w_scan 
w_scan version 20170107 (compiled for DVB API 5.11)
WARNING: could not guess your country. Falling back to 'DE'
guessing country 'DE', use -c <country> to override
using settings for GERMANY
DVB aerial
DVB-T Europe
scan type TERRESTRIAL, channellist 4
output format vdr-2.0
WARNING: could not guess your codepage. Falling back to 'UTF-8'
output charset 'UTF-8', use -C <charset> to override
Info: using DVB adapter auto detection.
        /dev/dvb/adapter0/frontend0 -> TERRESTRIAL "Realtek RTL2832 (DVB-T)": good :-)
        /dev/dvb/adapter0/frontend1 -> TERRESTRIAL "Panasonic MN88473": very good :-))

Using TERRESTRIAL frontend (adapter /dev/dvb/adapter0/frontend1)
-_-_-_-_ Getting frontend capabilities-_-_-_-_ 
Using DVB API 5.12
frontend 'Panasonic MN88473' supports
DVB-T2
INVERSION_AUTO
QAM_AUTO
TRANSMISSION_MODE_AUTO
GUARD_INTERVAL_AUTO
HIERARCHY_AUTO
FEC_AUTO
BANDWIDTH_AUTO not supported, trying 6/7/8 MHz.
FREQ (42.00MHz ... 1002.00MHz)
...
```

However, the latter one also supports DVB-C, so let's force the use of DVB-C with the command `w_scan -fc -cDE`.

```text
ubuntu@homeserver:~$ w_scan -fc -cDE
w_scan version 20170107 (compiled for DVB API 5.11)
using settings for GERMANY
DVB cable
DVB-C
scan type CABLE, channellist 7
output format vdr-2.0
WARNING: could not guess your codepage. Falling back to 'UTF-8'
output charset 'UTF-8', use -C <charset> to override
Info: using DVB adapter auto detection.
        /dev/dvb/adapter0/frontend0 -> "Realtek RTL2832 (DVB-T)" doesnt support CABLE -> SEARCH NEXT ONE.
        /dev/dvb/adapter0/frontend1 -> CABLE "Panasonic MN88473": very good :-))

Using CABLE frontend (adapter /dev/dvb/adapter0/frontend1)
-_-_-_-_ Getting frontend capabilities-_-_-_-_ 
Using DVB API 5.12
frontend 'Panasonic MN88473' supports
INVERSION_AUTO
QAM_AUTO
FEC_AUTO
FREQ (42.00MHz ... 1002.00MHz)
SRATE (1.000MSym/s ... 7.200MSym/s)
...
ERROR: Sorry - i couldn't get any working frequency/transponder
 Nothing to scan!!
```

Now it uses "CABLE" for the adapter frontend but it still doesn't find any frequencies/transponders. Checking the dmesg output provides the answer for this:

```text
[  447.588086] mn88473 1-0018: Direct firmware load for dvb-demod-mn88473-01.fw failed with error -2
[  447.588108] mn88473 1-0018: firmware file 'dvb-demod-mn88473-01.fw' not found
[  447.839732] mn88473 1-0018: Direct firmware load for dvb-demod-mn88473-01.fw failed with error -2
[  447.839752] mn88473 1-0018: firmware file 'dvb-demod-mn88473-01.fw' not found
```

It doesn't find the firmware file that is required to use the DVB-C frontend of the Astrometa USB stick. The [proprietary firmware for the Panasonic MN88473 is missing](https://forum.libreelec.tv/thread/11553-astrometa-t2hybrid/). The firmware doesn't ship with the default kernel from Ubuntu 24.04.1 LTS (Noble Numbat).

```bash
ubuntu@homeserver:~$ uname -a
Linux lubuntu 6.8.0-41-generic #41-Ubuntu SMP PREEMPT_DYNAMIC Fri Aug  2 20:41:06 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
```

## Installing the missing firmware
The firmware file needs to be placed into the folder `/lib/firmware` of the Linux system partition. Run the following commands to change the directory and to download[^1] the required firmware file.

```bash
cd /lib/firmware
sudo wget https://github.com/OpenELEC/dvb-firmware/raw/refs/heads/master/firmware/dvb-demod-mn88473-01.fw
```

Now re-insert the USB device and/or reboot the system.  

Re-running `w_scan` now finds all DVB-C programs as expected:

```text
ubuntu@homeserver:~$ w_scan -fc -cDE 
w_scan version 20170107 (compiled for DVB API 5.11)
using settings for GERMANY
DVB cable
DVB-C
scan type CABLE, channellist 7
output format vdr-2.0
WARNING: could not guess your codepage. Falling back to 'UTF-8'
output charset 'UTF-8', use -C <charset> to override
Info: using DVB adapter auto detection.
	/dev/dvb/adapter0/frontend0 -> "Realtek RTL2832 (DVB-T)" doesnt support CABLE -> SEARCH NEXT ONE.
	/dev/dvb/adapter0/frontend1 -> CABLE "Panasonic MN88473": very good :-))

Using CABLE frontend (adapter /dev/dvb/adapter0/frontend1)
-_-_-_-_ Getting frontend capabilities-_-_-_-_ 
Using DVB API 5.12
frontend 'Panasonic MN88473' supports
INVERSION_AUTO
QAM_AUTO
FEC_AUTO
FREQ (42.00MHz ... 1002.00MHz)
SRATE (1.000MSym/s ... 7.200MSym/s)
-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_ 
73000: sr6900 (time: 00:01.412) sr6875 (time: 00:02.945) 
81000: sr6900 (time: 00:04.475) sr6875 (time: 00:06.003) 
113000: sr6900 (time: 00:07.539) sr6875 (time: 00:09.069) 
121000: sr6900 (time: 00:10.609) sr6875 (time: 00:12.134) 
129000: sr6900 (time: 00:13.661) sr6875 (time: 00:15.202) 
137000: sr6900 (time: 00:16.732) sr6875 (time: 00:18.254) 
145000: sr6900 (time: 00:19.781) sr6875 (time: 00:21.312) 
153000: sr6900 (time: 00:22.849) sr6875 (time: 00:24.384) 
161000: sr6900 (time: 00:25.912) sr6875 (time: 00:27.447) 
169000: sr6900 (time: 00:28.972) sr6875 (time: 00:30.500) 
314000: sr6900 (time: 00:32.027) sr6875 (time: 00:33.552) 
322000: sr6900 (time: 00:35.078) sr6875 (time: 00:36.610) 
330000: sr6900 (time: 00:38.146)         signal ok:	QAM_AUTO f = 330000 kHz S6900C999  (0:0:0)
        QAM_AUTO f = 330000 kHz S6900C999  (0:0:0) : updating network_id -> (0:61449:0)
        new transponder: (QAM_256  f = 338000 kHz S6900C0  (1:61449:1101)) 0x4044
        already known: (QAM_AUTO f = 330000 kHz S6900C999  (0:61449:0)), but not found by pids
        QAM_AUTO f = 330000 kHz S6900C999  (0:61449:0) : updating tp ids -> (1:61449:1051)
        updating transponder:
           (QAM_AUTO f = 330000 kHz S6900C999  (1:61449:1051)) 0x0000
        to (QAM_256  f = 330000 kHz S6900C0  (1:61449:1051)) 0x4044
        new transponder: (QAM_256  f = 458000 kHz S6900C0  (1:61449:1073)) 0x4044
        new transponder: (QAM_256  f = 450000 kHz S6900C0  (1:61449:1079)) 0x4044
        new transponder: (QAM_256  f = 442000 kHz S6900C0  (61441:61449:10008)) 0x4044
        new transponder: (QAM_256  f = 466000 kHz S6900C0  (61441:61449:10000)) 0x4044
        new transponder: (QAM_256  f = 362000 kHz S6900C0  (133:61449:3)) 0x4044
        new transponder: (QAM_256  f = 378000 kHz S6900C0  (133:61449:4)) 0x4044
        new transponder: (QAM_256  f = 386000 kHz S6900C0  (133:61449:1)) 0x4044
        new transponder: (QAM_256  f = 522000 kHz S6900C0  (133:61449:6)) 0x4044
        new transponder: (QAM_256  f = 554000 kHz S6900C0  (133:61449:12)) 0x4044
        new transponder: (QAM_256  f = 346000 kHz S6900C0  (61441:61449:10019)) 0x4044
        new transponder: (QAM_256  f = 354000 kHz S6900C0  (61441:61449:10016)) 0x4044
        new transponder: (QAM_256  f = 370000 kHz S6900C0  (61441:61449:10002)) 0x4044
        new transponder: (QAM_256  f = 394000 kHz S6900C0  (61441:61449:10018)) 0x4044
        new transponder: (QAM_256  f = 402000 kHz S6900C0  (61441:61449:10023)) 0x4044
        new transponder: (QAM_256  f = 410000 kHz S6900C0  (61441:61449:10020)) 0x4044
        new transponder: (QAM_256  f = 418000 kHz S6900C0  (61441:61449:10014)) 0x4044
        new transponder: (QAM_256  f = 426000 kHz S6900C0  (61441:61449:10013)) 0x4044
        new transponder: (QAM_256  f = 434000 kHz S6900C0  (61441:61449:10012)) 0x4044
        new transponder: (QAM_256  f = 474000 kHz S6900C0  (61441:61449:10009)) 0x4044
        new transponder: (QAM_256  f = 482000 kHz S6900C0  (61441:61449:10010)) 0x4044
        new transponder: (QAM_256  f = 490000 kHz S6900C0  (61441:61449:10011)) 0x4044
        new transponder: (QAM_256  f = 498000 kHz S6900C0  (61441:61449:10006)) 0x4044
        new transponder: (QAM_64   f = 610000 kHz S6900C0  (61441:61449:10021)) 0x4044
        new transponder: (QAM_256  f = 562000 kHz S6900C0  (61441:61449:10017)) 0x4044
        new transponder: (QAM_256  f = 538000 kHz S6900C0  (61441:61449:10003)) 0x4044
        new transponder: (QAM_256  f = 546000 kHz S6900C0  (61441:61449:10005)) 0x4044
338000: skipped (already known transponder)
346000: skipped (already known transponder)
354000: skipped (already known transponder)
362000: skipped (already known transponder)
370000: skipped (already known transponder)
378000: skipped (already known transponder)
386000: skipped (already known transponder)
394000: skipped (already known transponder)
402000: skipped (already known transponder)
410000: skipped (already known transponder)
418000: skipped (already known transponder)
426000: skipped (already known transponder)
434000: skipped (already known transponder)
442000: skipped (already known transponder)
450000: skipped (already known transponder)
458000: skipped (already known transponder)
466000: skipped (already known transponder)
474000: skipped (already known transponder)
482000: skipped (already known transponder)
490000: skipped (already known transponder)
498000: skipped (already known transponder)
506000: sr6900 (time: 00:40.952)         signal ok:	QAM_AUTO f = 506000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 506000 kHz S6900C999  (0:0:0))
514000: sr6900 (time: 00:44.558)         signal ok:	QAM_AUTO f = 514000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 514000 kHz S6900C999  (0:0:0))
522000: skipped (already known transponder)
530000: sr6900 (time: 00:47.557)         signal ok:	QAM_AUTO f = 530000 kHz S6900C999  (0:0:0)
WARNING: received garbage data: crc = 0x8ac1fe22; expected crc = 0x271959e9
increasing filter timeout to 31 secs (pid:0 table_id:0 table_id_ext:-1).
        Info: no data from NIT(actual )after 11 seconds
538000: skipped (already known transponder)
546000: skipped (already known transponder)
554000: skipped (already known transponder)
562000: skipped (already known transponder)
570000: sr6900 (time: 01:02.562)         signal ok:	QAM_AUTO f = 570000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 570000 kHz S6900C999  (0:0:0))
578000: sr6900 (time: 01:05.573)         signal ok:	QAM_AUTO f = 578000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 578000 kHz S6900C999  (0:0:0))
586000: sr6900 (time: 01:08.555)         signal ok:	QAM_AUTO f = 586000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 586000 kHz S6900C999  (0:0:0))
594000: sr6900 (time: 01:11.565)         signal ok:	QAM_AUTO f = 594000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 594000 kHz S6900C999  (0:0:0))
602000: sr6900 (time: 01:14.574)         signal ok:	QAM_AUTO f = 602000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 602000 kHz S6900C999  (0:0:0))
610000: skipped (already known transponder)
618000: sr6900 (time: 01:17.557)         signal ok:	QAM_AUTO f = 618000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 618000 kHz S6900C999  (0:0:0))
626000: sr6900 (time: 01:20.554)         signal ok:	QAM_AUTO f = 626000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 626000 kHz S6900C999  (0:0:0))
634000: sr6900 (time: 01:23.573)         signal ok:	QAM_AUTO f = 634000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 634000 kHz S6900C999  (0:0:0))
642000: sr6900 (time: 01:26.571)         signal ok:	QAM_AUTO f = 642000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 642000 kHz S6900C999  (0:0:0))
650000: sr6900 (time: 01:29.564)         signal ok:	QAM_AUTO f = 650000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 650000 kHz S6900C999  (0:0:0))
658000: sr6900 (time: 01:32.550)         signal ok:	QAM_AUTO f = 658000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 658000 kHz S6900C999  (0:0:0))
666000: sr6900 (time: 01:35.552)         signal ok:	QAM_AUTO f = 666000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 666000 kHz S6900C999  (0:0:0))
674000: sr6900 (time: 01:38.554)         signal ok:	QAM_AUTO f = 674000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 674000 kHz S6900C999  (0:0:0))
682000: sr6900 (time: 01:41.557)         signal ok:	QAM_AUTO f = 682000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 682000 kHz S6900C999  (0:0:0))
690000: sr6900 (time: 01:44.562)         signal ok:	QAM_AUTO f = 690000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 690000 kHz S6900C999  (0:0:0))
698000: sr6900 (time: 01:47.572)         signal ok:	QAM_AUTO f = 698000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 698000 kHz S6900C999  (0:0:0))
706000: sr6900 (time: 01:50.554)         signal ok:	QAM_AUTO f = 706000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 706000 kHz S6900C999  (0:0:0))
714000: sr6900 (time: 01:53.566)         signal ok:	QAM_AUTO f = 714000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 714000 kHz S6900C999  (0:0:0))
722000: sr6900 (time: 01:56.556)         signal ok:	QAM_AUTO f = 722000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 722000 kHz S6900C999  (0:0:0))
730000: sr6900 (time: 01:59.567)         signal ok:	QAM_AUTO f = 730000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 730000 kHz S6900C999  (0:0:0))
738000: sr6900 (time: 02:02.556)         signal ok:	QAM_AUTO f = 738000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 738000 kHz S6900C999  (0:0:0))
746000: sr6900 (time: 02:05.565)         signal ok:	QAM_AUTO f = 746000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 746000 kHz S6900C999  (0:0:0))
754000: sr6900 (time: 02:08.571)         signal ok:	QAM_AUTO f = 754000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 754000 kHz S6900C999  (0:0:0))
762000: sr6900 (time: 02:11.558)         signal ok:	QAM_AUTO f = 762000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 762000 kHz S6900C999  (0:0:0))
770000: sr6900 (time: 02:14.550)         signal ok:	QAM_AUTO f = 770000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 770000 kHz S6900C999  (0:0:0))
778000: sr6900 (time: 02:17.567)         signal ok:	QAM_AUTO f = 778000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 778000 kHz S6900C999  (0:0:0))
786000: sr6900 (time: 02:20.557)         signal ok:	QAM_AUTO f = 786000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 786000 kHz S6900C999  (0:0:0))
794000: sr6900 (time: 02:23.573)         signal ok:	QAM_AUTO f = 794000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 794000 kHz S6900C999  (0:0:0))
802000: sr6900 (time: 02:26.561)         signal ok:	QAM_AUTO f = 802000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 802000 kHz S6900C999  (0:0:0))
810000: sr6900 (time: 02:29.551)         signal ok:	QAM_AUTO f = 810000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 810000 kHz S6900C999  (0:0:0))
818000: sr6900 (time: 02:32.574)         signal ok:	QAM_AUTO f = 818000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 818000 kHz S6900C999  (0:0:0))
826000: sr6900 (time: 02:35.551)         signal ok:	QAM_AUTO f = 826000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 826000 kHz S6900C999  (0:0:0))
834000: sr6900 (time: 02:38.573)         signal ok:	QAM_AUTO f = 834000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 834000 kHz S6900C999  (0:0:0))
842000: sr6900 (time: 02:41.552)         signal ok:	QAM_AUTO f = 842000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 842000 kHz S6900C999  (0:0:0))
850000: sr6900 (time: 02:44.550)         signal ok:	QAM_AUTO f = 850000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 850000 kHz S6900C999  (0:0:0))
858000: sr6900 (time: 02:47.552)         signal ok:	QAM_AUTO f = 858000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 858000 kHz S6900C999  (0:0:0))
73000: sr6900 (time: 02:50.553)         signal ok:	QAM_AUTO f = 73000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 73000 kHz S6900C999  (0:0:0))
81000: sr6900 (time: 02:53.552)         signal ok:	QAM_AUTO f = 81000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 81000 kHz S6900C999  (0:0:0))
113000: sr6900 (time: 02:56.552)         signal ok:	QAM_AUTO f = 113000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 113000 kHz S6900C999  (0:0:0))
121000: sr6900 (time: 02:59.551)         signal ok:	QAM_AUTO f = 121000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 121000 kHz S6900C999  (0:0:0))
129000: sr6900 (time: 03:02.551)         signal ok:	QAM_AUTO f = 129000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 129000 kHz S6900C999  (0:0:0))
137000: sr6900 (time: 03:05.551)         signal ok:	QAM_AUTO f = 137000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 137000 kHz S6900C999  (0:0:0))
145000: sr6900 (time: 03:08.560)         signal ok:	QAM_AUTO f = 145000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 145000 kHz S6900C999  (0:0:0))
153000: sr6900 (time: 03:11.561)         signal ok:	QAM_AUTO f = 153000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 153000 kHz S6900C999  (0:0:0))
161000: sr6900 (time: 03:14.558)         signal ok:	QAM_AUTO f = 161000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 161000 kHz S6900C999  (0:0:0))
169000: sr6900 (time: 03:17.560)         signal ok:	QAM_AUTO f = 169000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 169000 kHz S6900C999  (0:0:0))
314000: sr6900 (time: 03:20.559)         signal ok:	QAM_AUTO f = 314000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 314000 kHz S6900C999  (0:0:0))
322000: sr6900 (time: 03:23.563)         signal ok:	QAM_AUTO f = 322000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 322000 kHz S6900C999  (0:0:0))
330000: skipped (already known transponder)
338000: skipped (already known transponder)
346000: skipped (already known transponder)
354000: skipped (already known transponder)
362000: skipped (already known transponder)
370000: skipped (already known transponder)
378000: skipped (already known transponder)
386000: skipped (already known transponder)
394000: skipped (already known transponder)
402000: skipped (already known transponder)
410000: skipped (already known transponder)
418000: skipped (already known transponder)
426000: skipped (already known transponder)
434000: skipped (already known transponder)
442000: skipped (already known transponder)
450000: skipped (already known transponder)
458000: skipped (already known transponder)
466000: skipped (already known transponder)
474000: skipped (already known transponder)
482000: skipped (already known transponder)
490000: skipped (already known transponder)
498000: skipped (already known transponder)
506000: sr6900 (time: 03:26.559)         signal ok:	QAM_AUTO f = 506000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 506000 kHz S6900C999  (0:0:0))
514000: sr6900 (time: 03:29.562)         signal ok:	QAM_AUTO f = 514000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 514000 kHz S6900C999  (0:0:0))
522000: skipped (already known transponder)
530000: sr6900 (time: 03:32.561)         signal ok:	QAM_AUTO f = 530000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 530000 kHz S6900C999  (0:0:0))
538000: skipped (already known transponder)
546000: skipped (already known transponder)
554000: skipped (already known transponder)
562000: skipped (already known transponder)
570000: sr6900 (time: 03:35.573)         signal ok:	QAM_AUTO f = 570000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 570000 kHz S6900C999  (0:0:0))
578000: sr6900 (time: 03:38.565)         signal ok:	QAM_AUTO f = 578000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 578000 kHz S6900C999  (0:0:0))
586000: sr6900 (time: 03:41.555)         signal ok:	QAM_AUTO f = 586000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 586000 kHz S6900C999  (0:0:0))
594000: sr6900 (time: 03:44.565)         signal ok:	QAM_AUTO f = 594000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 594000 kHz S6900C999  (0:0:0))
602000: sr6900 (time: 03:47.554)         signal ok:	QAM_AUTO f = 602000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 602000 kHz S6900C999  (0:0:0))
610000: skipped (already known transponder)
618000: sr6900 (time: 03:50.553)         signal ok:	QAM_AUTO f = 618000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 618000 kHz S6900C999  (0:0:0))
626000: sr6900 (time: 03:53.556)         signal ok:	QAM_AUTO f = 626000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 626000 kHz S6900C999  (0:0:0))
634000: sr6900 (time: 03:56.551)         signal ok:	QAM_AUTO f = 634000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 634000 kHz S6900C999  (0:0:0))
642000: sr6900 (time: 03:59.572)         signal ok:	QAM_AUTO f = 642000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 642000 kHz S6900C999  (0:0:0))
650000: sr6900 (time: 04:02.552)         signal ok:	QAM_AUTO f = 650000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 650000 kHz S6900C999  (0:0:0))
658000: sr6900 (time: 04:05.550)         signal ok:	QAM_AUTO f = 658000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 658000 kHz S6900C999  (0:0:0))
666000: sr6900 (time: 04:08.560)         signal ok:	QAM_AUTO f = 666000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 666000 kHz S6900C999  (0:0:0))
674000: sr6900 (time: 04:11.558)         signal ok:	QAM_AUTO f = 674000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 674000 kHz S6900C999  (0:0:0))
682000: sr6900 (time: 04:14.570)         signal ok:	QAM_AUTO f = 682000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 682000 kHz S6900C999  (0:0:0))
690000: sr6900 (time: 04:17.551)         signal ok:	QAM_AUTO f = 690000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 690000 kHz S6900C999  (0:0:0))
698000: sr6900 (time: 04:20.572)         signal ok:	QAM_AUTO f = 698000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 698000 kHz S6900C999  (0:0:0))
706000: sr6900 (time: 04:23.573)         signal ok:	QAM_AUTO f = 706000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 706000 kHz S6900C999  (0:0:0))
714000: sr6900 (time: 04:26.565)         signal ok:	QAM_AUTO f = 714000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 714000 kHz S6900C999  (0:0:0))
722000: sr6900 (time: 04:29.551)         signal ok:	QAM_AUTO f = 722000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 722000 kHz S6900C999  (0:0:0))
730000: sr6900 (time: 04:32.574)         signal ok:	QAM_AUTO f = 730000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 730000 kHz S6900C999  (0:0:0))
738000: sr6900 (time: 04:35.563)         signal ok:	QAM_AUTO f = 738000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 738000 kHz S6900C999  (0:0:0))
746000: sr6900 (time: 04:38.557)         signal ok:	QAM_AUTO f = 746000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 746000 kHz S6900C999  (0:0:0))
754000: sr6900 (time: 04:41.567)         signal ok:	QAM_AUTO f = 754000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 754000 kHz S6900C999  (0:0:0))
762000: sr6900 (time: 04:44.562)         signal ok:	QAM_AUTO f = 762000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 762000 kHz S6900C999  (0:0:0))
770000: sr6900 (time: 04:47.570)         signal ok:	QAM_AUTO f = 770000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 770000 kHz S6900C999  (0:0:0))
778000: sr6900 (time: 04:50.561)         signal ok:	QAM_AUTO f = 778000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 778000 kHz S6900C999  (0:0:0))
786000: sr6900 (time: 04:53.558)         signal ok:	QAM_AUTO f = 786000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 786000 kHz S6900C999  (0:0:0))
794000: sr6900 (time: 04:56.558)         signal ok:	QAM_AUTO f = 794000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 794000 kHz S6900C999  (0:0:0))
802000: sr6900 (time: 04:59.561)         signal ok:	QAM_AUTO f = 802000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 802000 kHz S6900C999  (0:0:0))
810000: sr6900 (time: 05:02.561)         signal ok:	QAM_AUTO f = 810000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 810000 kHz S6900C999  (0:0:0))
818000: sr6900 (time: 05:05.556)         signal ok:	QAM_AUTO f = 818000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 818000 kHz S6900C999  (0:0:0))
826000: sr6900 (time: 05:08.559)         signal ok:	QAM_AUTO f = 826000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 826000 kHz S6900C999  (0:0:0))
834000: sr6900 (time: 05:11.565)         signal ok:	QAM_AUTO f = 834000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 834000 kHz S6900C999  (0:0:0))
842000: sr6900 (time: 05:14.566)         signal ok:	QAM_AUTO f = 842000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 842000 kHz S6900C999  (0:0:0))
850000: sr6900 (time: 05:17.550)         signal ok:	QAM_AUTO f = 850000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 850000 kHz S6900C999  (0:0:0))
858000: sr6900 (time: 05:20.555)         signal ok:	QAM_AUTO f = 858000 kHz S6900C999  (0:0:0)
        Info: no data from PAT after 2 seconds
        deleting (QAM_AUTO f = 858000 kHz S6900C999  (0:0:0))
tune to: QAM_256  f = 330000 kHz S6900C0  (1:61449:1051) (time: 05:23.570) 
	service = Das Erste HD (ARD)
	service = SWR RP HD (ARD)
	service = WDR HD Köln (ARD)
	service = alt_SR Fernsehen (ARD)
tune to: QAM_256  f = 338000 kHz S6900C0  (1:61449:1101) (time: 05:35.565) 
	service = BR Süd HD (ARD)
	service = NDR FS NDS HD (ARD)
	service = MDR Sachsen HD (ARD)
	service = SWR1 BW (ARD)
	service = SWR1 RP (ARD)
	service = SWR Kultur (ARD)
	service = SWR3 (ARD)
	service = SWR4 BW (ARD)
	service = SWR4 RP (ARD)
	service = DASDING (ARD)
	service = SWR Aktuell (ARD)
	service = hr1 (ARD)
	service = hr2 (ARD)
	service = hr3 (ARD)
	service = hr4 (ARD)
	service = YOU FM (ARD)
	service = hr INFO (ARD)
	service = MDR SACHSEN DD (ARD)
	service = MDR S-ANHALT MD (ARD)
	service = MDR THÜR Mitte-W (ARD)
	service = MDR KULTUR (ARD)
	service = MDR JUMP (ARD)
	service = MDR SPUTNIK (ARD)
	service = MDR AKTUELL (ARD)
	service = MDR KLASSIK (ARD)
	service = rbb24 Inforadio (ARD)
	service = radio3 (ARD)
	service = Antenne Brandenburg (ARD)
	service = rbb 88.8 (ARD)
	service = radioeins (ARD)
	service = Fritz (ARD)
	service = alt_Das Erste (ARD)
	service = alt_BR Süd (ARD)
	service = alt_hr-fernsehen (ARD)
	service = alt_WDR Köln (ARD)
	service = alt_SWR Fernsehen RP (ARD)
tune to: QAM_256  f = 458000 kHz S6900C0  (1:61449:1073) (time: 05:47.567) 
	service = alt_MDR S-Anhalt (ARD)
	service = Data Carousel 1 ((null))
	service = NDR FS HH HD (ARD)
	service = NDR FS SH HD (ARD)
	service = Bayern 1 (ARD)
	service = Bayern 2 (ARD)
	service = BAYERN 3 (ARD)
	service = BR-KLASSIK (ARD)
	service = BR24 (ARD)
	service = BR24live (ARD)
	service = BR Schlager (ARD)
	service = PULS (ARD)
	service = BR Heimat (ARD)
	service = NDR 2 NDS (ARD)
	service = NDR Kultur (ARD)
	service = NDR Info NDS (ARD)
	service = N-JOY (ARD)
	service = NDR 90,3 (ARD)
	service = NDR 1 Welle Nord KI (ARD)
	service = NDR 1 Radio MV SN (ARD)
	service = NDR 1 Nieders. HAN (ARD)
	service = NDR Info Spezial (ARD)
	service = NDR Blue (ARD)
	service = NDR Schlager (ARD)
	service = Bremen Eins (ARD)
	service = Bremen Zwei (ARD)
	service = Bremen Vier (ARD)
	service = Bremen NEXT (ARD)
	service = SR 1 Europawelle (ARD)
	service = SR kultur (ARD)
	service = SR 3 Saarlandwelle (ARD)
	service = 1LIVE (ARD)
	service = WDR 2 (ARD)
	service = WDR 3 (ARD)
	service = WDR 4 (ARD)
	service = WDR 5 (ARD)
	service = COSMO (ARD)
	service = 1LIVE diGGi (ARD)
	service = Die Maus (ARD)
	service = WDR Event (ARD)
	service = alt_rbb Berlin (ARD)
	service = alt_NDR FS SH (ARD)
tune to: QAM_256  f = 450000 kHz S6900C0  (1:61449:1079) (time: 05:59.574) 
	service = rbb Berlin HD (ARD)
	service = ZDF HD (ZDFvision)
	service = ZDFinfo HD (ZDFvision)
	service = ZDF (ZDFvision)
	service = 3sat (ZDFvision)
	service = KiKA (ZDFvision)
	service = ZDFinfo (ZDFvision)
	service = Dlf Kultur (ZDFvision)
	service = Dlf (ZDFvision)
	service = zdf_neo (ZDFvision)
	service = Dlf Nova (ZDFvision)
	service = Data Carousel 2 (KD Home)
tune to: QAM_256  f = 442000 kHz S6900C0  (61441:61449:10008) (time: 06:12.553) 
	service = Radio-Test (Digital Free)
	service = sixx (Digital Free)
	service = TELE 5 (Digital Free)
	service = EPG Digital (KD Home)
	service = Sagem RCI88-320 (1) (KD Home)
	service = Eurosport 1 (Digital Free)
	service = ProSieben (Digital Free)
	service = kabel eins (Digital Free)
	service = WELT (Digital Free)
tune to: QAM_256  f = 466000 kHz S6900C0  (61441:61449:10000) (time: 06:25.553) 
	service = TV2 Danmark (dan) (Digital Free)
	service = Sylt1 (easyCAST )
	service = DR1 (dan) (Digital Free)
	service = ANIXE+ (Digital Free)
	service = DMF HD (KD Home)
	service = Radio BOB (Vodafone)
	service = Schlager Radio (Vodafone)
	service = ROCK ANTENNE Hamburg (Vodafone)
	service = Delta Radio (Vodafone)
	service = radio ffn - Hamburg/Lüneburg (Vodafone)
	service = Antenne Niedersachsen HH/LG (Vodafone)
	service = Radio Teddy (Vodafone)
	service = Klassik Radio (Vodafone)
	service = Radio Hamburg (Vodafone)
	service = MEGA Radio (Digital Free)
	service = Radio Schleswig-Holstein - Kiel (Vodafone)
	service = DRP1/DRP2 (dan) (Vodafone)
	service = DRP3 (dan) (Vodafone)
	service = DRP4 (dan) (Vodafone)
	service = NDR 2 (Vodafone)
	service = NDR Info SH (Vodafone)
	service = RTL (Digital Free)
	service = SAT.1 (Digital Free)
tune to: QAM_256  f = 362000 kHz S6900C0  (133:61449:3) (time: 06:37.564) 
	service = Romance TV (SKY)
	service = BEM Service ((null))
	service = GFD Service ((null))
	service = Humax PR-HD3000C ((null))
	service = Pace TDC 866 NSDX ((null))
	service = STB channel config. ((null))
	service = Sky Sport Bundesliga 6 HD (SKY)
	service = Sky Sport Bundesliga 7 HD (SKY)
	service = Sky Sport 6 HD (SKY)
	service = Sky Sport 7 HD (SKY)
	service = Sky Crime HD (SKY)
	service = Nick.Jr. (SKY)
	service = Sky Sport Golf HD (SKY)
	service = Sky Sport Tennis HD (SKY)
	service = Sky Cinema Highlights HD (SKY)
	service = Sky Cinema Classics HD (SKY)
tune to: QAM_256  f = 378000 kHz S6900C0  (133:61449:4) (time: 06:50.571) 
	service = Sky Cinema Family HD (SKY)
	service = Cartoonito (SKY)
	service = Sky Sport Bundesliga 5 HD (SKY)
	service = Sky Sport 5 HD (SKY)
	service = NDS Applikation (SKY)
	service = SDMS Service 1 ((null))
	service = SDMS Service 2 ((null))
	service = Sky Sport Bundesliga 8 HD (SKY)
	service = Sky Sport 8 HD (SKY)
	service = Sky Sport F1 HD (SKY)
	service = Sky Sport Premier League HD (SKY)
	service = HISTORY Channel (SKY)
	service = Sky Atlantic HD (SKY)
	service = Sky One HD (SKY)
	service = Prime Sportsbar (SKY)
	service = RTLSport 1 (SKY)
tune to: QAM_256  f = 386000 kHz S6900C0  (133:61449:1) (time: 07:02.551) 
	service = 13th Street HD (SKY)
	service = SYFY HD (SKY)
	service = Universal TV HD ((null))
	service = Sky Krimi HD (SKY)
	service = Sky Sport Bundesliga 4 HD (SKY)
	service = Sky Sport 4 HD (SKY)
	service = Sky Sport Bundesliga 9 HD (SKY)
	service = Sky Sport 9 HD (SKY)
	service = Sky Sport Mix HD (SKY)
	service = Sky Replay HD (SKY)
	service = Sky Cinema Action HD (SKY)
	service = RTLSport 2 (SKY)
tune to: QAM_256  f = 522000 kHz S6900C0  (133:61449:6) (time: 07:14.567) 
WARNING: received garbage data: crc = 0xa261ae1e; expected crc = 0x7effffff
increasing filter timeout to 31 secs (pid:0 table_id:0 table_id_ext:-1).
WARNING: received garbage data: crc = 0xe6e5fd2c; expected crc = 0x4ceba331
WARNING: received garbage data: crc = 0x9e78254a; expected crc = 0xab33ffff
WARNING: received garbage data: crc = 0xe04b5702; expected crc = 0x4cebab30
WARNING: received garbage data: crc = 0xcd7b9dc9; expected crc = 0x0074536b
WARNING: received garbage data: crc = 0x6e17e8ac; expected crc = 0xe731ffff
        Info: no data from PAT after 31 seconds
        Info: no data from PAT after 2 seconds
        Info: no data from SDT(actual) after 3 seconds
        Info: no data from NIT(actual )after 11 seconds
tune to: QAM_256  f = 554000 kHz S6900C0  (133:61449:12) (time: 07:59.592) 
	service = Sky Sport Bundesliga HD (SKY)
	service = Sky Sport Bundesliga 3 HD (SKY)
	service = Sky Sport 3 HD (SKY)
	service = Sky Sport News HD (SKY)
	service = Sky Sport Bundesliga UHD (SKY)
	service = Sky Sport UHD (SKY)
	service = . (SKY)
	service = Sky Documentaries HD (SKY)
	service = Sky Sport Bundesliga 2 HD (SKY)
	service = Sky Sport 2 HD (SKY)
	service = Beate Uhse HD (SKY)
tune to: QAM_256  f = 346000 kHz S6900C0  (61441:61449:10019) (time: 08:11.562) 
	service = RTL HD (Digital Free)
	service = SAT.1 HD (Digital Free)
	service = Kiel TV (Kiel TV)
tune to: QAM_256  f = 354000 kHz S6900C0  (61441:61449:10016) (time: 08:23.563) 
	service = AXN White HD (KD Home)
	service = Disney Ch. HD (Digital Free)
	service = NatGeo Wild HD (VF) (KD Home)
	service = The HISTORY Channel HD (KD Home)
	service = WELT HD (Digital Free)
	service = UNIVERSAL TV HD (VF) (KD Home)
	service = RTL Crime HD (KD Home)
tune to: QAM_256  f = 370000 kHz S6900C0  (61441:61449:10002) (time: 08:35.568) 
----------no signal----------
tune to: QAM_AUTO f = 370000 kHz S6900C999  (61441:61449:10002) (time: 08:40.164)  (no signal)
----------no signal----------
tune to: QAM_256  f = 394000 kHz S6900C0  (61441:61449:10018) (time: 08:44.725) 
	service = tagesschau24 HD (Digital Free)
	service = ARD alpha HD (Digital Free)
	service = SR Fernsehen HD (ARD)
	service = SPORT1+ HD (KD Home)
	service = AXN Black HD (KD Home)
	service = HOME & GARDEN TV HD (KD Home)
        updating transponder:
           (QAM_AUTO f = 370000 kHz S6900C999  (61441:61449:10002)) 0x0000
        to (QAM_256  f = 370000 kHz S6900C0  (61441:61449:10002)) 0x4044
tune to: QAM_256  f = 402000 kHz S6900C0  (61441:61449:10023) (time: 08:57.564) 
	service = ONE HD (Digital Free)
	service = RTLZWEI HD (Digital Free)
	service = QVC Style HD (Digital Free)
	service = TLC HD (KD Home)
	service = DELUXE MUSIC HD (Digital Free)
	service = 1-2-3.tv HD (Digital Free)
	service = QVC2 HD (Digital Free)
tune to: QAM_256  f = 410000 kHz S6900C0  (61441:61449:10020) (time: 09:09.562) 
	service = DMAX HD (Digital Free)
	service = ProSieben MAXX HD (Digital Free)
	service = sonnenklar.TV HD (Digital Free)
	service = Discovery Channel HD (VF) (KD Home)
	service = SAT.1 Gold HD (Digital Free)
	service = HSE Extra HD (Digital Free)
	service = DAZN 2 (KD Home)
	service = DAZN 2 (Sky) (KD Home)
tune to: QAM_256  f = 418000 kHz S6900C0  (61441:61449:10014) (time: 09:21.564) 
	service = arte HD (ARD)
	service = hr-fernsehen HD (ARD)
	service = kabel eins HD (Digital Free)
	service = SPORT1 HD (Digital Free)
	service = Shop LC HD (Digital Free)
tune to: QAM_256  f = 426000 kHz S6900C0  (61441:61449:10013) (time: 09:33.569) 
	service = Warner TV Serie HD (VF) (KD Home)
	service = Warner TV Film HD (KD Home)
	service = 13th Street HD (VF) (KD Home)
	service = VOX HD (Digital Free)
	service = ProSieben HD (Digital Free)
	service = ProSieben FUN HD (KD Home)
	service = Kabel eins Doku HD (KD Home)
tune to: QAM_256  f = 434000 kHz S6900C0  (61441:61449:10012) (time: 09:46.554) 
	service = Sky One HD (KD Home)
	service = NatGeo HD (VF) (KD Home)
	service = SYFY HD (VF) (KD Home)
	service = Warner TV Comedy HD (KD Home)
	service = Romance TV HD (KD Home)
	service = KinoweltTV HD (KD Home)
	service = GEO TV HD (KD Home)
tune to: QAM_256  f = 474000 kHz S6900C0  (61441:61449:10009) (time: 09:58.561) 
WARNING: received garbage data: crc = 0xf486e566; expected crc = 0xe41451cd
increasing filter timeout to 31 secs (pid:0 table_id:0 table_id_ext:-1).
WARNING: received garbage data: crc = 0x1d20af39; expected crc = 0xea9a1408
increasing filter timeout to 31 secs (pid:103 table_id:2 table_id_ext:-1).
WARNING: received garbage data: crc = 0x4c33c79b; expected crc = 0xbd2aa4d8
increasing filter timeout to 31 secs (pid:106 table_id:2 table_id_ext:-1).
WARNING: received garbage data: crc = 0x8b9cc245; expected crc = 0x688a8f39
increasing filter timeout to 31 secs (pid:102 table_id:2 table_id_ext:-1).
WARNING: received garbage data: crc = 0x969d8a16; expected crc = 0xea9a1408
WARNING: received garbage data: crc = 0xc11129d1; expected crc = 0xbd28a4d8
WARNING: received garbage data: crc = 0x71ca6ead; expected crc = 0xea9a1408
WARNING: received garbage data: crc = 0x6442bfae; expected crc = 0x26d31e97
increasing filter timeout to 32 secs (pid:17 table_id:66 table_id_ext:-1).
WARNING: received garbage data: crc = 0xbe064720; expected crc = 0x8ca83c1f
increasing filter timeout to 40 secs (pid:16 table_id:64 table_id_ext:-1).
WARNING: received garbage data: crc = 0xe17324c1; expected crc = 0x26d31e97
WARNING: received garbage data: crc = 0xeea23c08; expected crc = 0x8da83c1f
WARNING: received garbage data: crc = 0xf2202996; expected crc = 0x12d5aad3
WARNING: received garbage data: crc = 0x1b31a458; expected crc = 0x26d31e97
WARNING: received garbage data: crc = 0xbe2faf6a; expected crc = 0xf027291f
WARNING: received garbage data: crc = 0x94bb2da0; expected crc = 0x12d5aad3
	service = PHOENIX HD (ARD)
	service = zdf_neo HD (ZDFvision)
	service = KiKA HD (ZDFvision)
	service = DOKUSAT HD (KD Home)
	service = TELE 5 HD (Digital Free)
	service = CNN HD (eng) (Digital Free)
WARNING: received garbage data: crc = 0x224ef64e; expected crc = 0xf027291f
WARNING: received garbage data: crc = 0xf30125c6; expected crc = 0x8ca83c1f
WARNING: received garbage data: crc = 0xc16dff6a; expected crc = 0x1912d5aa
WARNING: received garbage data: crc = 0xf43b70d4; expected crc = 0xf027291f
WARNING: received garbage data: crc = 0x66929814; expected crc = 0x8ca83c1f
WARNING: received garbage data: crc = 0x2b46a55c; expected crc = 0xf027291f
WARNING: received garbage data: crc = 0xd46dfcb7; expected crc = 0x12d5aad3
WARNING: received garbage data: crc = 0xbc01b06c; expected crc = 0xe716000d
WARNING: received garbage data: crc = 0x379a0dec; expected crc = 0x12d5aad3
WARNING: received garbage data: crc = 0xc2438391; expected crc = 0xf027291f
WARNING: received garbage data: crc = 0x0e9720cd; expected crc = 0x12d5aad3
        Info: no data from NIT(actual )after 40 seconds
tune to: QAM_256  f = 482000 kHz S6900C0  (61441:61449:10010) (time: 10:39.551) 
	service = Nick Jr. HD (KD Home)
	service = Comedy Central HD (Digital Free)
	service = Nick HD (Digital Free)
	service = SPORTDIGITAL FUSSBALL HD (KD Home)
	service = RTL Living HD (KD Home)
	service = DAZN 1 (KD Home)
	service = DAZN 1 (Sky) (KD Home)
tune to: QAM_256  f = 490000 kHz S6900C0  (61441:61449:10011) (time: 10:51.570) 
	service = Bibel TV HD (Digital Free)
	service = kabel eins CLASSICS HD (KD Home)
	service = SAT.1 emotions HD (KD Home)
	service = ntv HD (Digital Free)
	service = C+I HD (KD Home)
	service = RTLup HD (KD Home)
	service = Eurosport 1 HD (Digital Free)
tune to: QAM_256  f = 498000 kHz S6900C0  (61441:61449:10006) (time: 11:04.554) 
	service = sixx HD (Digital Free)
	service = DF1 HD (Digital Free)
	service = HSE HD (Digital Free)
	service = QVC HD (Digital Free)
	service = SUPER RTL HD (Digital Free)
	service = NITRO HD (Digital Free)
	service = Eurosport 2 HD (KD Home)
tune to: QAM_64   f = 610000 kHz S6900C0  (61441:61449:10021) (time: 11:16.553) 
	service = AlJaz Balkan/AlJaz (bos/ara) (Digital Free)
	service = France24 (fre/eng) (Digital Free)
	service = PCNE/ NTD TV (chi) (Digital Free)
	service = Arirang/Fashion TV (eng/spa) (Digital Free)
	service = Tunisie 1 (ara) (Digital Free)
	service = Duna TV (hun) (Digital Free)
	service = France 3 (fre) (Div. Sprachen)
	service = Pro TV/TVR (rum) (Digital Free)
	service = GINX TV (eng) (Digital Free)
	service = BVN (dut) (Digital Free)
	service = CNBC Europe (eng) (Digital Free)
	service = 1+1 International (Digital Free)
	service = Halk TV / Kanal Avrupa (tur) (KD Home)
tune to: QAM_256  f = 562000 kHz S6900C0  (61441:61449:10017) (time: 11:29.565) 
	service = 3sat HD (ZDFvision)
	service = DRadio DokDeb (Digital Free)
	service = RPR1. (Digital Free)
	service = MTV HD (Digital Free)
	service = kabel eins Doku (Digital Free)
	service = UNSERDING (Digital Free)
	service = AntenneSaar (Digital Free)
	service = WDR 2  Ostwestfalen/Lippe (Digital Free)
	service = Schwarzwaldradio (Digital Free)
	service = QVC2 (Digital Free)
	service = HSE EXTRA (Digital Free)
	service = ProSieben MAXX (Digital Free)
	service = SONLife (eng) (Digital Free)
	service = DMF (Digital Free)
	service = eSports1 HD (KD Home)
	service = DELUXE MUSIC (Digital Free)
	service = SAT.1 Gold (Digital Free)
	service = Astro TV (Digital Free)
tune to: QAM_256  f = 538000 kHz S6900C0  (61441:61449:10003) (time: 11:41.566) 
	service = Heimatkanal (KD Home)
	service = Silverline (KD Home)
	service = HOME & GARDEN TV (Digital Free)
	service = Cartoon Network (KD Home)
	service = Nick (Digital Free)
	service = Cartoonito (KD Home)
	service = N24 Doku (Digital Free)
	service = CGTN (Div. Sprachen)
	service = K-TV (Digital Free)
	service = Comedy Central (Digital Free)
	service = QVC (Digital Free)
	service = DMAX (Digital Free)
	service = VOXup (Digital Free)
	service = HSE (Digital Free)
tune to: QAM_256  f = 546000 kHz S6900C0  (61441:61449:10005) (time: 11:54.557) 
	service = 90s90s (Digital Free)
	service = MTV (Digital Free)
	service = DOKUSAT (Digital Free)
	service = RTLup (Digital Free)
	service = SCHLAGER DELUXE (Digital Free)
	service = Bibel TV (Digital Free)
	service = Super RTL (Digital Free)
	service = RTLZWEI (Digital Free)
	service = VOX (Digital Free)
	service = ntv (Digital Free)
	service = Al Jazeera Int (eng) (Digital Free)
	service = sonnenklar.TV (Digital Free)
	service = SPORT1 (Digital Free)
	service = bigFM (Digital Free)
	service = Oldie Antenne (Digital Free)
	service = RADIO BOLLERWAGEN (Digital Free)
	service = Disney Channel (Digital Free)
	service = Fix & Foxi (KD Home)
	service = Hip-Hop Deutschland (Kabel Digital)
	service = 80er & 90er Hits (Kabel Digital)
	service = Kuschelsongs (Kabel Digital)
	service = Blues (Kabel Digital)
	service = Soul & R'n'B (Kabel Digital)
	service = Reggae (Kabel Digital)
	service = Cool Jazz (Kabel Digital)
	service = Radio Horeb (Digital Free)
	service = ERF Plus (Digital Free)
	service = BBC World Service (Englisch)
	service = sunshine live (Digital Free)
	service = RTL Radio (Digital Free)
	service = Radio Paloma (Digital Free)
	service = JAM FM (Digital Free)
	service = METROPOL FM (Türkisch)
	service = Beats Radio (Digital Free)
	service = 80s80s (Digital Free)
	service = HRT HR1 (Kabel Digital)
	service = Pink Radio (Kabel Digital)
	service = Rai Radio 1 (Kabel Digital)
	service = Radio ZET (Kabel Digital)
	service = Radio Exterior (Kabel Digital)
	service = RDP Internacional (Kabel Digital)
tune to: QAM_AUTO f = 530000 kHz S6900C999  (0:0:0) (time: 12:07.557) 
        Info: no data from PAT after 2 seconds
        Info: no data from PAT after 2 seconds
        Info: no data from SDT(actual) after 3 seconds
        Info: no data from NIT(actual )after 11 seconds
(time: 12:23.589) dumping lists (311 services)
..
Done, scan time: 12:23.593
```

[^1]: An [alternative download link](https://palosaari.fi/linux/v4l-dvb/firmware/MN88473/01/latest/dvb-demod-mn88473-01.fw) might be the website from the Linux Kernel developer Antti Palosaari.

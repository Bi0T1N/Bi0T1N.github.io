---
layout: default
title:  "HDMI issues with AMD Ryzen™ 7 PRO 4750U on Linux kernel 5.4"
categories: [thinkpad, L15, AMD Ryzen Pro 4750U, linux, Mint, kernel]
---

# {{ page.title }}

## Introduction
I've installed Linux Mint 20 Ulyana (MATE desktop) on my new Lenovo ThinkPad® L15 20U8 and after setting it up I tried to connect my monitor through the HDMI port - my monitor woke up from the standby mode but then just said that there is no connection.  
One of the first things which came to my mind was that its caused by the new Accelerated Processing Unit (APU - CPU with integrated GPU) generation which might not be fully supported yet. Following commits [[drm/amd/display: Add Renoir DML](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b04641a3f4c54b00dab7ccd49fd45909c42c3fc2), [drm/amd/display: Add Renoir registers (v3)](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b593bce59bfa25d9abbf220b6614396ccd965b1b), [drm/amd/display: Add Renoir resource (v2)](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=6f4e6361c3ff8457d45d2a898c418e3495e85e93)] indicate that the AMD Ryzen 4000 series (Renoir) should be supported by the Linux kernel 5.4 but there is another commit that marks the support as experimental [[drm/amdgpu: flag renoir as experimental for now](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8cf3219ccd5c0f05f6265fff26cce0de8061e38)] - probably because it doesn't work flawless.  
I decided that the easiest way is to try a newer kernel and see if its working out-of-the-box then.

## Steps to update the kernel
1. Go to [https://kernel.ubuntu.com/~kernel-ppa/mainline/](https://kernel.ubuntu.com/~kernel-ppa/mainline/)
2. Select the kernel version you would like to install (I've used [v5.6.19](https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.6.19/))
3. Verify that the tests for your architecture were successful
4. Download the linux-headers, linux-image and linux-modules
    * In my case:
      * linux-headers-5.6.19-050619-generic_5.6.19-050619.202006171132_amd64.deb
      * linux-headers-5.6.19-050619_5.6.19-050619.202006171132_all.deb
      * linux-image-unsigned-5.6.19-050619-generic_5.6.19-050619.202006171132_amd64.deb
      * linux-modules-5.6.19-050619-generic_5.6.19-050619.202006171132_amd64.deb
5. Install the new kernel version via `sudo dpkg -i *.deb`
6. Reboot
7. Verify that the kernel was installed successfully via `uname -sr`

## Result
After updating and rebooting my system the HDMI port is working fine and I can use my additional monitor without problems. The new kernel also works without any other issues so far.

## Sources
[AMD Ryzen 4000 Mobile Series "Renoir" Graphics No Longer Experimental With Linux 5.5](https://www.phoronix.com/scan.php?page=news_item&px=Renoir-Graphics-Not-Experiment)  
[Linux kernel source tree](https://git.kernel.org/)  
[How to upgrade Linux Kernel in Ubuntu and Linux Mint](https://www.fosslinux.com/7167/how-to-upgrade-linux-kernel-in-ubuntu-and-linux-mint.htm)

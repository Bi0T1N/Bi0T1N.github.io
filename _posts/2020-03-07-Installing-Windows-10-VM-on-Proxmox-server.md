---
layout: default
title:  "Installing Windows 10 VM on Proxmox server"
categories: [proxmox, windows10, setup, securing rdp]
---

# Installing Windows 10 VM on [Proxmox server](https://www.proxmox.com/)

## Windows 10 ISO
At first you need to [create a Windows 10 installation media](https://www.microsoft.com/en-gb/software-download/windows10). This automatically downloads the latest version of Windows which avoids useless downloads of windows updates on all new VMs.

## VirtIO drivers
You also need to download the [VirtIO drivers](https://fedoraproject.org/wiki/Windows_Virtio_Drivers) as [ISO file](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso).
The VirtIO Drivers allow direct (paravirtualized) access to device and peripherals from inside the VMs instead of using slower, emulated ones.

## Proxmox GUI VM setup
1. General
	- change VM ID and insert Name
2. OS
	- select ISO image
	- Type Microsoft Windows
	- Version 10/2016
3. System
	- Graphic card Default and SCSI VirtIO SCSI
4. Hard Disk
	- Bus: VirtIO Block
	- Cache
        - Default (No cache) -> _safer_
        - Write back -> _best performance_
	- adjust Disk size
	- Disk-Image Format:
        - Raw file format provides slightly better performance
        - qcow2 offers advanced features such as copy on write and Live Snapshots
5. CPU
	- Sockets/Cores as you have/want
	- Type: Default (kvm64)
6. Memory
	- set Memory
7. Network
	- Model: VirtIO (paravirtualized)
	- enable Firewall
8. Confirm
	- click finish

9. Add VirtIO drivers
	- create a new CDROM drive (use "Add -> CD/DVD drive" in the hardware tab)
	- load the VirtIO Drivers ISO in the new virtual CDROM drive

## Windows installation setup
Start the VM, just follow the Windows installer (e.g. install Windows 10 Pro 64bit) and follow the installer steps until you reach the installation type selection where you need to select "Custom (advanced)".
Now click "Load driver" to install the VirtIO drivers for hard disk and the network:
- Hard Disk: Browse to the CD drive where you mounted the VirtIO driver and select folder "viostor\w10\amd64" and confirm. Select the "Red Hat VirtIO SCSI controller" and click next to install it. Now you should see your drive.
- Network: Repeat the steps from above (click again "Load driver", etc.) and select the folder "NetKVM\w10\amd64", confirm it and select "Redhat VirtIO Ethernet Adapter" and click next.
- Memory Ballooning: Again, repeat the steps but this time select the "Balloon\w10\amd64" folder, then the "VirtIO Balloon Driver" and install it by clicking next.  

The Memory ballooning (KVM only) allows your guest to dynamically change its memory usage by evicting unused memory during run time.
If all drivers are installed, click format and choose the drive and continue the Windows installer steps.

**Source**: https://pve.proxmox.com/wiki/Windows_10_guest_best_practices

## Windows installation procedure
1. Select region and keyboard layout
2. Set up for personal use
3. Select Offline Account as we don't want/need to have a Microsoft Account
4. Insert Account + Password + Security Questions
5. Deny the 'more across devices history'
6. Deny 'digital assistant'
7. Select 'Don't use online speech recognition'
8. Select 'No' for location
9. Say 'No' to 'Find my device'
10. Send just 'Basic' diagnostic data to Microsoft
11. Don't 'Improve inking & typing'
12. Don't 'Get tailored experiences'
13. Say 'no' to 'advertising ID'

If its up and running, check 'Device Manager' for missing drivers, if all is installed you can remove the CDROM drive with the VirtIO drivers in the Proxmox GUI.

## Windows modifications
Now the Windows VM is ready to use but I'd recommend following steps:
1. Download [Firefox Browser](https://www.mozilla.org/en-US/firefox/new/) and install.
2. Download [O&O ShutUp10](https://www.oo-software.com/en/shutup10) and install.
3. Securing the Remote Desktop (RDP)
4. Activate Windows

### Securing the Remote Desktop (RDP)
1. Hit Windows key + R to bring up a Run prompt, and type _sysdm.cpl_
    - On 'Computer Name' tab click on 'Change...' to give the computer a name
    - Goto Remote tab and tick 'Allow remote connections to this computer' and also 'Only allow connections from computers running Remote Desktop with Network Level Authentication'
2. If you want to give access to other users, click 'Select Users...'
    - Any accounts in the Administrators group will already have access. 
    - If you need to grant Remote Desktop access to any other users, just click _Add_ and type in the usernames.

    **NOTE**: All users with Remote Desktop access should have strong passwords!

3. Run prompt (Windows Key + R) and type _secpol.msc_ to open the Local Security Policy menu.
	1. expand 'Local Policies' and click on 'User Rights Assignment.'
	2. Double-click on the 'Allow log on through Remote Desktop Services' policy listed on the right  
    A recommendation is to remove both of the groups already listed in this window, _Administrators_ and _Remote Desktop Users_.  

    After that, click 'Add User or Group' and manually add the users you'd like to grant Remote Desktop access to. This isn't an essential step but it gives you more power over which accounts get to use Remote Desktop.  
    If, in the future, you make a new Administrator account for some reason and forget to put a strong password on it, you're opening your computer up to hackers around the world if you never bothered removing the 'Administrators' group from this screen.

4. Close the Local Security Policy window and open the Local Group Policy Editor by typing _gpedit.msc_ into either a Run prompt or the Start menu.  
    When the Local Group Policy Editor opens, expand Computer Configuration > Administrative Templates > Windows Components > Remote Desktop Services > Remote Desktop Session Host, and then click on Security.  
    Use double click to open following settings:  
    1. Set client connection encryption level – Set this to High Level so your Remote Desktop sessions are secured with 128-bit encryption.  
	2. Require secure RPC communication – Set this to Enabled.  
	3. Require use of specific security layer for remote (RDP) connections – Set this to SSL (TLS 1.0).  
	4. Require user authentication for remote connections by using Network Level Authentication – Set this to Enabled.

    Once those changes have been made, you can close the Local Group Policy Editor.

5. Change the default port that Remote Desktop listens on.  
    By default, Remote Desktop listens on port 3389. Pick a five digit number less than 65535
	1. open up the Registry Editor by typing _regedit_ into a Run prompt or the Start menu.
	2. When the Registry Editor opens up, expand HKEY_LOCAL_MACHINE > SYSTEM > CurrentControlSet > Control > Terminal Server > WinStations > RDP-Tcp > then double-click on 'PortNumber' in the window on the right.
	3. With the PortNumber registry key open, select 'Decimal' on the right side of the window and then type your five digit number under 'Value data' on the left.
	4. Since we've changed the default port that Remote Desktop uses, we'll need to configure Windows Firewall to accept incoming connections on that port. Go to the Start screen, search for _Windows Defender Firewall_ and click on it.
	5. When Windows Defender Firewall opens, click 'Advanced Settings' on the left side of the window. Then right-click on 'Inbound Rules' and choose 'New Rule.'
	6. The 'New Inbound Rule Wizard' will pop up, select Port and click next. On the next screen, make sure TCP is selected and then enter the port number you chose earlier, and then click next. Click next two more times because the default values on the next couple pages will be fine. On the last page, select a name for this new rule, such as 'Custom RDP port' and then click finish.
	7. To keep track of who is logging into your PC (and from where), you can open up _Event Viewer_. Once you have Event Viewer opened, expand Applications and Services Logs > Microsoft > Windows > TerminalServices-LocalSessionManger and then click Operational.

**Source**: https://www.howtogeek.com/175087/how-to-enable-and-secure-remote-desktop-on-windows/

### Activate Windows
1. Use your license
2. Use HWID activator tool
    1. Windows Defender will bark about activator tools when downloaded, so disable it
	    1. Open Windows Defender
	    2. Go to 'Virus & thread protection' and click on 'Manage settings'
	    3. Disable 'Real-time protection'
    2. Double click executable and click on 'More info' when 'Windows protected your PC' appears, click 'Run anyway'
    3. Follow activator tool instructions


## How to remove RDP connections from the connection cache
1. Press Windows + R and then type _regedit_ in the Run dialog box to open Registry Editor.
2. Next, in the left Window of Registry Editor, move to following registry key:
	- HKEY_CURRENT_USER\Software\Microsoft\Terminal Server Client\Default
3. In the right window, navigate to the registry string named _MRU[number]_ where the _[number]_ may be `0, 1, 2, …`. You need to right-click on this string and then select 'Delete'.
4. Now close Registry Editor and restart your PC.

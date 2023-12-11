# wifi mon mode on linux

### Getting Started with iw

First be aware that iw distinguishes between wireless LAN hardware devices (the physical layer, referred to as phy) and the network interface configured to use that hardware (e.g. wlan0, similar to an Ethernet eth0 interface). To see the list of devices, and interfaces for each device:

    $ iw dev
    phy#0
    	Interface wlan0
    		ifindex 3
    		type managed
    

In my case (and most likely for most typical computers) the hardware is phy0 and my network interface is wlan0. You can see detailed information about the hardware using:

    $ iw phy phy0 info
    Wiphy phy0
    	Band 1:
    		Capabilities: 0x172
    			HT20/HT40
    ...
    	Supported interface modes:
    		 * IBSS
    		 * managed
    		 * AP
    		 * AP/VLAN
    		 * WDS
    		 * monitor
    		 * mesh point
    	software interface modes (can always be added):
    		 * AP/VLAN
    		 * monitor
    ...
    

Of importance for the next step is the supported/software interface modes should include entry for "monitor", meaning your hardware supports monitor mode. If there is no "monitor" entry, then you will not be able to capture other peoples data using the next steps.

### Capturing in Monitor Mode

If your hardware device supports monitor mode then you must add a monitor interface called mon0.

    $ sudo iw phy phy0 interface add mon0 type monitor
    

You can check that it is added:

    $ iw dev
    phy#0
    	Interface mon0
    		ifindex 4
    		type monitor
    	Interface wlan0
    		ifindex 3
    		type managed
    

We will capture with the mon0 interface, so you can delete the normal wlan0 interface:

    $ sudo iw dev wlan0 del
    

Now enable the mon0 interface using ifconfig:

    $ sudo ifconfig mon0 up
    

Before capturing, specify the wireless LAN frequency you want to capture on. You should choose the frequency based on the channels used by neighbouring access points. The frequency is given in MHz, e.g.\\ channel 6 is 2437.

![wlan-channels.png](https://sandilands.info/sgordon/images/wlan-channels.png)
[2.4 GHz Wi-Fi channels (802.11b,g WLAN)](https://commons.wikimedia.org/wiki/File:2.4_GHz_Wi-Fi_channels_%28802.11b,g_WLAN%29.svg), Michael Gauthier / Wikimedia Commons / CC-BY-SA-3.0 /

    $ sudo iw dev mon0 set freq 2437
    

To check that your interface is in monitor mode and using the correct frequency you can use iwconfig:

    $ iwconfig mon0
    mon0      IEEE 802.11bgn  Mode:Monitor  Frequency:2.437 GHz  Tx-Power=20 dBm   
              Retry  long limit:7   RTS thr:off   Fragment thr:off
              Power Management:on      
    

Now you can capture, e.g. using tcpdump:

    $ sudo tcpdump -i mon0 -n -w wireless.cap
    

Ctrl-C to stop the capture, then view with [Wireshark](https://www.wireshark.org/). To display select wireless LAN frames in Wireshark use the [wlan](https://www.wireshark.org/docs/dfref/w/wlan.html) and [wlan_mgt](https://www.wireshark.org/docs/dfref/w/wlan_mgt.html) filters. (My [brief summary](http://ict.siit.tu.ac.th/~sgordon/netlab/its332ch3.html#x5-370003.3.3) of Wireshark and WLAN filters)

### Returning to Managed Mode

If after monitoring you want to revert the changes and continue using the wlan0 interface in managed mode (e.g. connect to an AP), then delete the mon0 interface and add the wlan0interface:

    $ sudo iw dev mon0 del
    $ sudo iw phy phy0 interface add wlan0 type managed
    $ iw dev
    phy#0
    	Interface wlan0
    		ifindex 5
    		type managed
    $ iwconfig wlan0
    wlan0     IEEE 802.11bgn  ESSID:off/any  
              Mode:Managed  Access Point: Not-Associated   Tx-Power=20 dBm   
              Retry  long limit:7   RTS thr:off   Fragment thr:off
              Power Management:on
    

### What Can Go Wrong?

If you get errors with some of the above iw commands, then:

1. Check that the wireless device is not soft/hard blocked by rfkill and unblock it if it is:
	
	    $ rfkill list
	    0: phy0: Wireless LAN
	    	Soft blocked: yes
	    	Hard blocked: no
	    $ rfkill unblock 0
	    
	
2. Make sure you are using the correct interface/device. In my examples I use phy0, wlan0 and mon0. Yours may be different.

If the commands work, but in Wireshark you can only see packets either to your computer or broadcast/multicast (i.e. you cannot see any packets from one computer to another computer, such as HTTP or SSH), then:

1. Make sure the frequency you selected is being used by others.
2. Check that your device supports monitor mode (look in the output of iw phy phy0 info). Some wireless cards do not support monitor mode, and even if they do, some drivers do not support it.

### Selecting a Wireless Adapter that Supports Monitor Mode

On the last point above, finding a wireless adapter that supports monitor mode to allow capturing of data packets in Linux can be troublesome. It depends on both the hardware and driver support. The [Linux wireless drivers page](https://wireless.wiki.kernel.org/en/users/drivers) provides a quick summary of monitor mode support in different drivers. To find out which devices use which drivers you can search on [WikiDevi](https://wikidevi.com/wiki/Main_Page). As of March 2015, devices that use Atheros, Intel, RaLink or Broadcom chipsets seem to have good monitor mode support.

When looking to buy a wireless USB (or PCI) adapter that will support monitor mode, find some devices that are available, look them up on [WikiDevi](https://wikidevi.com/wiki/Main_Page) to see the drivers, and then check the [driver support for monitor mode](https://wireless.wiki.kernel.org/en/users/drivers). Be especially careful with hardware versions: many branded devices are updated over time and although they have the same model number, the internal wireless chipset may change. Some devices I have successfully used include:

* USB (2014): generic brand, made by Shenzhen ([pic](http://thaieasyelec.com/products/development-boards/wireless-usb-2.0-adapter-with-antenna-detail.html)), similar to Tenda W311MI but with attachable antenna, RaLink RT5370 (driver: rt2800usb)
* USB (2012): D-Link DWA-160 HW version A2, Atheros AR9170 (carl9170)
* USB (2011): Alfa Networks AWUS036H, Realtek 8187 (rtl8187)
* MiniPCI (2011): Intel Centrino Wireless-N 1000 (iwlwifi)
* MiniPCI (2009): Atheros (ath5k)
* PCI (2009): SMC WPCIG Atheros AR5007G (ath5k?)

I recently ordered a batch of TP-Link TL-WN821N after checking that it supported monitor mode, but when delivered it was hardware version 4 which used a Realtek chipset (rtl8192cu) that did NOT fully support monitor mode (version 1 to 3 used Atheros chips, which did support monitor mode). Be carefully in checking the specific hardware versions when purchasing wireless devices.

Created on Mon, 09 Mar 2015, 6:51pm

Last changed on Tue, 10 Mar 2015, 8:15pm

# wwanHotspot

It is designed to connect an OpenWrt router to devices that share the Internet connection through a wifi Hotspot, such as mobile phones. In one place there may be several Hotspots that may be available or not according to the comings and goings of their owners; we will enter the parameters of each one of them in the configuration file therefore wwanHotspot will connect and disconnect the OpenWrt HotSpot client to one of them as they become available.

# Installation
Configure wifi interfaces according to the OpenWrt wiki.

https://wiki.openwrt.org/doc/recipes/ap_sta

and install the package wwanHotspot via ssh or telnet,

1- Install the ipk:
   ```
   opkg install wwanHotspot_VERSION_all.ipk
   ```
2- edit the config file and set your HotSpots parameters.
   ```
   vi /etc/config/wwanHotspot
   ```

3- enable the daemon and start it
   ```
   /etc/init.d/wwanHotspot enable
   /etc/init.d/wwanHotspot restart
   ```
# Configuration

Variables named Debug and ScanAuto are indicators, value is false when null or not set, true when they have any value.

Debug enabled will send a shell command log to the file "/var/log/wwanHotspot".

We can enable ScanAuto to look periodically for a Hotspot only when the Wan interface is disconnected, the time interval is stored in variable Sleep. Setting ScanAuto to the special value "allways" makes wwanHotspot not care of the Wan interface status and will scan periodically, but is better request an scan to the daemon via ssh or telnet. ScanAuto "allways" is not recommended because overloads too much the wifi interface, to avoid this the program increases the time between scans; the value of the large time lapsus is SleepScanAuto.

Set the list of network values for your Hotspots. Multiple HotSpots are allowed, wwanHotspot will try to connect to any of them by rotation. If the list is not populated then wwanHotspot will use the current configuration for this interface.

After changing the config file we must reload the daemon.

# Operation

At start wwanHotspot will look once for a HotSpot.
Also will look for availability of another HotSpot after disconnection.

Automatically receives an scan signal when the HotSpot is disconnected, wwanHotspot deactivates the Hotspot client interface and maintains AP allways up.

When the Hotspot client is disconnected we can request a wifi scan issuing the following command when we know that a HotSpot is available:
   ```
   /etc/init.d/wwanHotspot scan
   ```
If we update the config file while wwanHotspot is running we must reload the daemon, the change is not detected in my current version of OpenWrt.
   ```
   /etc/init.d/wwanHotspot reload # maybe required or not
   ```
A log is written to the file "/var/log/wwanHotspot".

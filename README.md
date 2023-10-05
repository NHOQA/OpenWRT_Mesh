# OpenWRT_Mesh
OpenWRT based 802.11s mesh running on RPi. 

Based pretty heavily on the work done by the guys at ATAK HQ  https://atakhq.com/en/manet/openwrt
Gives data access to ATAK in excess of what is available with the Meshtastic forwarder plugin.
Have successfully send location, markers, test and pictures through ATAK with this hardware. The built in ICE plugin for voice also works, but I have not purchased the licesnes to do any extended testing
Currently I have not found an integrated voice solution.

I am certainly no networking guy, this is a process I've pieced together and may not be the best way to do this. This is just what I've done to get a functional mesh running. 
This was written for a Pi 4b with 4GB of memory. general process should apply to any pi

1. Download Firmware from OpenWRT
2. Flash to SD card
3. Boot Pi
4. Set Timezone, Password, Hostname
5. Set Static IP address
6. Connect Pi to Internet
7. Package Install/Uninstall
   - WPAD-Mesh-wolfSSL: install, allow overwrite
   - WPAD-Basic-wolfSSL: uninstall, remove unused packages
   - MESH11sd: install, allow overwrite
   - KMOD-MT76-core: install, allow overwrite
   - KMOD-MT76-USB: install, allow overwrite
   - KMOD-MT76x2u: install, allow overwrite
8. Create Mesh Network
   - Operating Frequency
   - Channel
   - Transmit Power
   - Mode
   - Mesh ID
   - Network
9.Complete



   

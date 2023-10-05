# OpenWRT_Mesh
OpenWRT based 802.11s mesh running on RPi. 

Based pretty heavily on the work done by the guys at ATAK HQ  https://atakhq.com/en/manet/openwrt
Gives data access to ATAK in excess of what is available with the Meshtastic forwarder plugin.
Have successfully send location, markers, test and pictures through ATAK with this hardware. The built in ICE plugin for voice also works, but I have not purchased the licenses to do any extended testing
Currently I have not found an integrated voice solution. Potentially Larix broadcaster? 

I am certainly no networking guy, this is a process I've pieced together and may not be the best way to do this. This is just what I've done to get a functional mesh running. 
This was written for a Pi 4b with 4GB of memory with an Alfa AWUS036ACM external dongle. I'm using the 2.4 ghz band for these units. General process should apply to any pi

1. Download Firmware from OpenWRT. Grab the EXT4 package ![firmware select](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/8ed6b890-0aa1-484e-bd88-4a1bc200303e)

2. Flash to SD card
3. Boot Pi
4. Set Timezone, Password, Hostname
5. Set Static IP address
6. Connect Pi to Internet
7. Package Install/Uninstall
   - WPAD-Mesh-wolfSSL: install, allow overwrite. Do this before removing wpad-basic 
   - WPAD-Basic-wolfSSL: uninstall, remove unused packages
   - Mesh11sd: install, allow overwrite
   - kmod-MT76-core: install, allow overwrite
   - kmod-MT76-USB: install, allow overwrite
   - kmod-MT76x2u: install, allow overwrite
8. Create Mesh Network
   - Operating Frequency
   - Channel
   - Transmit Power
   - Mode
   - Mesh ID
   - Network
9.Complete



   

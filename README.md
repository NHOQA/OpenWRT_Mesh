# OpenWRT_Mesh
OpenWRT based 802.11s mesh running on RPi. 

Based pretty heavily on the work done by the guys at ATAK HQ  https://atakhq.com/en/manet/openwrt
Gives data access to ATAK in excess of what is available with the Meshtastic forwarder plugin.
Have successfully send location, markers, test and pictures through ATAK with this hardware. The built in ICE plugin for voice also works, but I have not purchased the licenses to do any extended testing
Currently I have not found an integrated voice solution that doesnt require purchasing a license.

I am certainly no networking guy, this is a process I've pieced together and may not be the best way to do this. This is just what I've done to get a functional mesh running. 
This was written for a Pi 4b with 4GB of memory with an Alfa AWUS036ACM external dongle. I'm using the 2.4 ghz band for these units. General process should apply to any pi

1. Download Firmware from OpenWRT. Grab the EXT4 package ![firmware select](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/8ed6b890-0aa1-484e-bd88-4a1bc200303e)

2. Flash to SD card. I use Balena Etcher for this ![balena](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/e26924e1-faf0-49be-a811-da49508a7cbd)

3. Boot Pi. This part gets a little complex, may only apply to my particular setup though. 
   - My router is locked to the 192.168.1.1 IP address and I cannot change it (thanks spectrum). To get around this I created a second network that is completely isolated from my spectrum network. I did this because the default IP that the OpenWRT image you just flashed will also try to use 192.168.1.1, causing a conflict. My setup has a dumb TP link ethernet switch with both the Pi I'm booting and a laptop (with wifi off) connected to it. Nothing else, we'll need to run like this just long enough to get the static IP changed
4. Set Timezone, Password, Hostname: starting username is root, no password.
   - Timezone and Hostname are located at System>System
   - Password is at System>Administration
6. Set Static IP address![Inkedinterface scree for static IP](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/eafa0a34-0aca-4e6a-b199-a03ff24e24fe)

   - Navigate to Network>Interfaces
   - Click on the "Edit" button
   - Change IPV4 Address to whatever you want. Note, each node will need to have a different static IP. I just sequenced numerically, 1.1,1.2,1.3 etc. Good idea not to have it conflicting with anything currently on your network, as we'll need to get back onto the main network for internet access soon.
 7. Connect Pi to internet.
   - Connect 
9. Package Install/Uninstall
   - WPAD-Mesh-wolfSSL: install, allow overwrite. Do this before removing wpad-basic 
   - WPAD-Basic-wolfSSL: uninstall, remove unused packages
   - Mesh11sd: install, allow overwrite
   - kmod-MT76-core: install, allow overwrite
   - kmod-MT76-USB: install, allow overwrite
   - kmod-MT76x2u: install, allow overwrite
10. Create Mesh Network
   - Operating Frequency
   - Channel
   - Transmit Power
   - Mode
   - Mesh ID
   - Network
9.Complete



   

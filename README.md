# OpenWRT_Mesh
OpenWRT based 802.11s mesh running on RPi. 

This was written for OpenWRT 22.03, there were some changes to the package list with 23.05 and this guide will not be directly applicable 

BoM:
 - 1 ea. Raspberry pi 4, 4gb  https://www.mouser.com/ProductDetail/Raspberry-Pi/SC01949?qs=T%252BzbugeAwjjISb%252BwlagpRw%3D%3D
 - 1 ea. MicroSD card
 - 1 ea. Alfa AWUS036ACHM wifi dongle https://www.amazon.com/AWUS036ACHM-802-11ac-Range-Boost-Adapter/dp/B08SJBV1N3/ref=rvi_sccl_6/133-1308152-8998443?pd_rd_w=LGdq0&content-id=amzn1.sym.f5690a4d-f2bb-45d9-9d1b-736fee412437&pf_rd_p=f5690a4d-f2bb-45d9-9d1b-736fee412437&pf_rd_r=EAZ7EG9WJCSMH5A22JC5&pd_rd_wg=cZVZR&pd_rd_r=071d966d-8f38-453d-97a1-3daad8746531&pd_rd_i=B08SJBV1N3&psc=1
 - 1 ea. 90 degree USB C cable (provide power to Pi) https://www.amazon.com/dp/B0BN1NJQFX?ref=ppx_yo2ov_dt_b_product_details&th=1
 - 1 ea. USB splitter https://www.amazon.com/dp/B08C5FWQND?ref=ppx_yo2ov_dt_b_product_details&th=1
 - 1 ea. USB to ethernet adapter https://www.amazon.com/dp/B09Q5XC7T9?psc=1&ref=ppx_yo2ov_dt_b_product_details
 - 1 ea. USB C cable (charge phone) https://www.amazon.com/etguuds-Charging-Charger-Compatible-Samsung/dp/B0924GYSVS/ref=sr_1_19?crid=1JR78MZOIYX1C&keywords=usb%2Bc%2Bmale%2Bto%2Bmale&qid=1697206580&sprefix=usb%2Bc%2Bmale%2Bto%2Bmale%2Caps%2C85&sr=8-19&th=1
 - 1 ea. 2' ethernet cable https://www.amazon.com/dp/B003EE30JM?ref=ppx_yo2ov_dt_b_product_details&th=1
 - 1 ea. Ravpower 20k mAh power bank https://www.ravpower.com/products/rp-pb201-pd-60w-20000mah-portable-charger

 - Fusion360 archive for 3d printed case also uploaded, has been updated for the newest dongle. Still in development, will upload versions as they are tested. V24 is the first version using the new wifi dongle, previous versions will not work with this BoM

Based pretty heavily on the work done by the guys at ATAK HQ  https://atakhq.com/en/manet/openwrt
Gives data access to ATAK in excess of what is available with the Meshtastic forwarder plugin.
Have successfully send location, markers, text, audio, and pictures through ATAK with this hardware. Voice plug in is functional, but have not spent the time to find the solution to getting that audio piped into a headset. Have also sent "video" between EUD's but it was unusable, have not expended any effort on that yet, I'm sure i was just doing something wrong. Testing still needs to happen to look at ranges/bandwidth/battery drain out in the field. This is all coming after getting disappointing results with the Alfa AWUS036ACM (much lower power dongle)


I am certainly no networking guy, this is a process I've pieced together and may not be the best way to do this. This is just what I've done to get a functional mesh running. 
This was written for a Pi 4b with 4GB of memory with an Alfa AWUS036ACHM external dongle. I'm using the 2.4 ghz band for these units. General process should apply to any pi

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
 7. Connect Pi to internet.![connectwifi](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/8a453488-0bdc-4e73-a976-b7784efbf745)

   - At this stage I connected the Pi to my home network wifi. Navigate to Network>wireless. Hit scan beside the "Cypress CYW..." (this is the Pi onboard wifi), and log onto your wireless network as normal. 
9. Package Install/Uninstall. ![softeware](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/6ddfa4cf-4e35-42e4-8e61-5120f397fe44)
   - Navigate to System>Software, hit "update lists" wait for list to populate then search for packages below. to uninstall the wpad-basic, you need to change over to the "installed" tab
   - WPAD-Mesh-wolfSSL: install, allow overwrite. Do this before removing wpad-basic 
   - WPAD-Basic-wolfSSL: uninstall, remove unused packages
   - Mesh11sd: install, allow overwrite
   - kmod-mt76x0u: install, allow overwrite ( this is the driver set for the Alfa dongle)
     
10. Create Mesh Network.  now we have the packages installed, navigate back to Network>Wireless. You should see a new interface "Generic 802.11acbg", this is the Alfa dongle. Click "add" beside that interface![Dongle in](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/4337cfec-694f-4267-9375-da31148baade)

   - Operating Frequency
   - Channel (all nodes must be on the same channel)
   - Transmit Power
   - Mode
   - Mesh ID (all nodes must share the exact same mesh ID)
   - Network
     These are the settings I used (note this is a screenshot from a previous build with the ACM dongle, power setting may vary with the ACHM. Also note the power setting here is likely bugged, theres some talk about this on the forums. something about an internal amp bringing the output to 30 dbm even though it shows much less in the GUI) ![Meshconfig](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/fd850d73-78a4-400c-b8ab-36e446609682)

9.Complete: once that's done you should be online. I disabled my home wifi at this point and can see that I have 2 other nodes online and they are connected to this node. ![Meshactive](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/229b6161-5f17-48c4-9968-9fbf14f73cc0)




   

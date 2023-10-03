# OpenWRT_Mesh
OpenWRT based 802.11s mesh running on RPi. 

Based pretty heavily on the work done by the guys at ATAK HQ  https://atakhq.com/en/manet/openwrt
Gives data access to ATAK in excess of what is available with the Meshtastic forwarder plugin.
Have successfully send location, markers, test and pictures through ATAK with this hardware. The built in ICE plugin for voice also works, but I have not purchased the licesnes to do any extended testing
Currently I have not found an integrated voice solution.

I am certainly no networking guy, this is a process I've pieced together and may not be the best way to do this. This is just what I've done to get a functional mesh running. 
This was written for a Pi 4b with 4GB of memory. general process should apply to any pi

1. First step is to grab the correct firmware from OpenWRT. https://openwrt.org/toh/raspberry_pi_foundation/raspberry_pi. make sure to download the "install" version of the firmware and not the "upgrade" version.
![firmware page 1](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/f695b218-ec79-4328-95e8-2fe822c54435)
2. Once that's downloaded, flash it to your microSD card. I use Balena etcher, had a bunch of 8GB SD cards which were far larger than needed.
![flash 2](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/efa10fcc-7bd0-4424-b962-ee3864d4e90d)
3. Go ahead and insert the SD card into the Pi but hold off on powering up until we talk over step 4.
4. This is also covered on the OpenWRT page above under "how to connect via ethernet". But the issue here is my router is locked onto 192.168.1.1 and I cannot change it (thanks spectrum). The Pi with the OpenWRT image you just flashed also will try to use 192.168.1.1 Obviously there will be a conflict here. The way I got around this was to connect the Pi ethernet to a spare dumb TP link switch I had laying around. I then connected my laptop (that is not connected to my main home network) to the same switch. Those are the only 2 connections, I isolated the Pi from my home network so while there is no internet access, it is also not conflicting with my router. The next few steps will be performed on the internet isolated laptop/Pi mini-network. I am sure there are better ways to do this, but with my knowledge level, this is what I had to do to make it work.
5. Power everything up and use your browser to navigate (in my case on the laptop) to 192.168.1.1. username should be pre-populated with "root" and there is no password. Just hit "login"
6. First thing we're doing is changing the static IP to something that will not conflict with the network so we can get back to the main, internet connected network for later steps
7. Navigate to Network>interfaces
8. There should be a single interface "lan", hit the edit button.
9. Change the IPv4 address to something that will not conflict with anything else on your main network. hit save.
10. Back on the network interfaces page, hit the "save" button, then "save and apply"
11. Wait for ~1 minute then navigate to the IP address you just set to confirm it works. If it does, power the pi down, plug the ethernet cable back into your main network and power the pi back up.
12. Navigate to system>system and set hostname and time zone. Make sure to hit "save" then "save and apply", do this for every step requiring a save 
![timezone 3](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/82bd3e40-556b-49fb-b96e-95577a358cad)
13. Navigate to system>administration to set a password, save
14. Navigate to network>wireless. If you're running a Pi 4 you should see "cypress cyw..." with a few buttons beside it. Hit "scan" and connect to your wifi network
15. Navigate to system>software and hit "update lists"
![list 4](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/56303ab9-260c-4910-9e4e-7253015afa88)
16. First we need to install 2 packages from the "available" tab. use the search function, fins and install wpad-mesh-wolfssl and mesh11sd allow overwrite
17. Then move over to the tab beside "available" to look at your "installed" packages. Search and UN-install wpad-basic-wolfssl
18. next if you are using the Alfa AWUS036ACM wifi dongle, we'll need to install 2 more packages. Move back to the "available" tab and install kmod-mt76-usb (allow overwrite) and kmod-mt76x2u. for this step i do not allow overwriting of any preinstalled packages.
19. Once that's complete, navigate back to Network>wireless. it should look something like this, assuming you have the Alfa dongle plugged in. The "cypress..." entry is the Pi's onboard wifi, and the "generic MAC 80211...." is the Alfa dongle
![6wirelss](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/f007c590-b416-4259-a19e-904564698e54)
20. remove the connection to your home wifi network
21. On the "generic MAC..." device hit "add", then fill out the connection info as below. Mode:band: 2.4ghz (if that's what you want) channel: whichever you desire, but all nodes need to be on the same channel. transmit power: I max it out, not sure if the driver/hardware can actually hit that power. Under interface configuration mode: 802.11s and Mesh ID: enter whatever you desire, but every node in the mesh youre building must ahve the same mesh ID. Then under the network dropdown , select "lan"
![7mesh](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/090cf086-516a-4d70-bbf1-63584b732868)
22. hit save, then back on the main screen hit save then save and apply



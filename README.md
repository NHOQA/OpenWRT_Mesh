# OpenWRT_Mesh
OpenWRT based 802.11s mesh running on RPi. 

Based pretty heavily on the work done by the guys at ATAK HQ  https://atakhq.com/en/manet/openwrt
Gives data access to ATAK in excess of what is available with the Meshtastic forwarder plugin.
Have successfully send location, markers, test and pictures through ATAK with this hardware. The built in ICE plugin for voice also works, but I have not purchased the licesnes to do any extended testing
Currently I have not found an integrated voice solution.

I am certainly no networking guy, this is a process I've pieced together and may not be the best way to do this. This is just what I've done to get a functional mesh running. 
This was written for a Pi 4b with 4GB of memory. general process should apply to any pi

1. First step is to grab the correct firmware from OpenWRT. https://openwrt.org/toh/raspberry_pi_foundation/raspberry_pi. make sure to download the "install" version of the firmware and not the "upgrade" version.
2. Once that's downloaded, flash it to your microSD card. I use Balena etcher, had a bunch of 8GB SD cards which were far larger than needed.
![firmware page 1](https://github.com/boyette2001/OpenWRT_Mesh/assets/74009174/f695b218-ec79-4328-95e8-2fe822c54435)

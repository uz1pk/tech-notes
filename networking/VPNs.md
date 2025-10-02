# What is a VPN (Virtual Private Network)

Before going into what is a VPN is lets talk about what it aims to address.
- In cases where data isn't encrypted (HTTP), VPNs encrypt the data locally on your machine, tunnel the traffic to some VPN server, decrypt it and send it to the destination. This prevents local network sniffers and ISPs from logging/seeing your data.
	- Caveat here is if the host machine itself has malware, they data could be read prior to even leaving the machine. Alongside this from the VPN server to the destination server the data is unencrypted
- Anonymity, if you are making requests to certain IPs every intermediate network hop can see both Your public IP alongside the destination IP you are connecting too.
	- We will discuss how later but VPNs hide that association of your public IP <> destination IP by introducing an intermediary (basically a proxy)
- Bypassing regional IP bans, since IP address spaces are assigned to different regions. There might be government or ISP level blocking happening when attempting to connect to a blocked website.
	- You can now access that destination via VPN (assuming the VPN server itself is not blocked)
	
There are many more use cases but this is just to list a few.

A VPN itself is a secure, logical communication channel built over a public or untrusted network that enables remote systems or networks to behave as if they are directly connected on the same private network. This unlocks a few major things, security, reliability and anonymity 

VPN = abstraction, and depending on implementation, it can look like a “virtual cable” (Layer 2), a “virtual router” (Layer 3), or just a “secure transport channel” (Layer 4). A **VPN is not one protocol**, it’s a concept: _create a secure, logical connection over an insecure network_.

The key to VPNs is you are shifting trust of you data to different entities and hiding it from others. Alongside introducing a layer of anonymity to your internet browsing.
# Tunneling

TODO
# VIN (Virtual Network Interface)
# Layer 2 VPNs

TODO
# Layer 3 VPNs

TODO
# Layer 4 VPNs

TODO
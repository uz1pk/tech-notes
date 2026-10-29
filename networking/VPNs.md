
TODO better organize these notes and break it up into more digestible sections
# What is a VPN (Virtual Private Network)

TODO(uzaheer): The key to understanding VPNs is understanding how data changes going up and down the OSI model and at what points. Understanding L3 <> L2 interaction is extremely important. Alongside TUN/TAP linux network interfaces

Before going into what is a VPN is lets talk about what it aims to address.
- In cases where data isn't encrypted (HTTP), VPNs encrypt the data locally on your machine, tunnel the traffic to some VPN server, decrypt it and send it to the destination. This prevents local network sniffers and ISPs from logging/seeing your data.
	- Caveat here is if the host machine itself has malware, they data could be read prior to even leaving the machine. Alongside this from the VPN server to the destination server the data is unencrypted
- Anonymity, if you are making requests to certain IPs every intermediate network hop can see both Your public IP alongside the destination IP you are connecting too.
	- We will discuss how later but VPNs hide that association of your public IP <> destination IP by introducing an intermediary (basically a proxy)
- Bypassing regional IP bans, since IP address spaces are assigned to different regions. There might be government or ISP level blocking happening when attempting to connect to a blocked website.
	- You can now access that destination via VPN (assuming the VPN server itself is not blocked)
- Making it seem as if you are connecting too a private network server
	- The VPN client on your machine will do some IP overrides, DNS entry manipulation, etc. But from the client's end it looks like `private-server.com` just works
	
There are many more use cases but this is just to list a few.

A VPN itself is a secure, logical communication channel built over a public or untrusted network that enables remote systems or networks to behave as if they are directly connected on the same private network. This unlocks a few major things, security, reliability and anonymity 

VPN = abstraction, and depending on implementation, it can look like a “virtual cable” (Layer 2), a “virtual router” (Layer 3), or just a “secure transport channel” (Layer 4). A **VPN is not one protocol**, it’s a concept: _create a secure, logical connection over an insecure network_.

The key to VPNs is you are shifting trust of you data to different entities and hiding it from others. Alongside introducing a layer of anonymity to your internet browsing

- VPNs can be TLS terminating
### What packet encapsulation achieves

After you login to a VPN client (authenticate and authorize the host/client device). Your device is now validated and remembered by the VPN client. So now any traffic directed to a VPN specific IP (10.0.0.2) can be routed too by the VPN encapsulating your destination IP and wrapping it in the VPN servers IP while also performing encryption on the inner packet itself.

Then when the VPN server receives the outer packet decrypts and reads the inside packet. It will see another L3 hop to the private IP and as such route the actual traffic intended by the host device.

This unlocks
- Login once, auth'd everywhere in a private network vs having an API / application level path based routing from the public internet via VPN gateway (see below) which would require logging into to each individual server
- TODO
### VPN Gateway

Is simply some public server/IP that routes request using some mechanism (and example is maybe API route path based routing /my-api/server1) to internal server.
### How decapsulation and routing works

1. The VPN server is just running VPN software (e.g., OpenVPN, WireGuard, IPSec).
2. When it receives the outer packet on its public NIC, it sees:
3. Outer IP header: YourPublicIP → VPNServerIP
4. Transport header: UDP/TCP (whatever protocol the VPN tunnel runs on)
5. Encrypted payload blob.
6. The VPN process (in userspace or kernel module) decrypts the blob using the session key, which yields the inner packet.
7. That inner packet is a normal IP packet (e.g., 10.8.0.2 → 10.0.0.5).
8. The VPN software then injects this inner packet into the OS networking stack as if it arrived directly from a NIC.
# Tunneling

Secure pipe sending data back and forth, generally abstracts L3 and lower networking from the user / host.
# VNIC (Virtual Network Interface Card)

At a high level the VPN client asks the operating system to create a virtual NIC on your host machine alongside giving an IP address for your NIC (for your router / gateway device to know where to route traffic back too). Then an IP address that's on-prem / in the private network when resolved via ARP will be routed to the Virtual NIC.

The packet is simply sent to the process that is assigned / "listening" on that virtual NIC and does the actually encapsulation of re-applying L3/L2 data and encapsulating the inner packet (while also encrypting ofc by some shared key between VPN client and server)

For regular IPs aka if the packet isn’t meant for the VPN (e.g., you’ve configured split tunneling, or it’s just a normal public IP not routed through VPN), the OS routes it out of your normal NIC without encapsulation since the IP table won't be overwritten for that private IP from the VPN client.

Virtual NICs in general are exposed via TUN/TAP devices on linux and the operating itself generally give you an API to create V NICs and have processes capture the data. I guess this is similar to the loopback interface?
# Layer 2 VPNs

TODO
# Layer 3 VPNs

TODO

### VPNs via UDP

TODO
# Layer 4 VPNs

TODO

# Consumer vs Corporate VPNs

I personally found confusing when reading into VPNs since it's generally framed as "prviate network access" etc. 

But when looking at consumer VPNs you aren't exactly using these VPNs to access a private network.

To clarify consumer VPNs (nord VPN and others) aren't fully technically VPNs at all. Like yes it does technically simulate a virtual private network but you're note REALLY connecting to any private networks. Instead consumer VPNs are there to:
1. Manage who can see/log your data. When using a VPN you are encrypting on the host machine to the destination VPN server. As such you essentially remove trust from the ISP and regional internet + local network and allocate it all to the VPN server you are using
2. Privacy: From anyone on your local network, ISP up until the VPN server. It looks like your computer is always connecting to the same IP (real IP is encapsulated in inner packet). Then from the destination server the Source IP it see's isn't your computer but rather the VPN server. As such there is full anonymity across the entire network flow EXCEPT at the VPN server level.

Corporate VPNs achieve all the same as the above. WITH the caveat that you can now connect to private networks managed by corporations. Example inside a corporate network there is a SQL server under 10.0.0.1. When you computer makes a request to the SQL server IP (ignore DNS) during L2 phase when deciding which NIC to use for the IP. Because the VPN client (host machine) overwrote the host's IP <> MAC address table, the host's packet is now routed to the Virtual NIC. Then the VPN client will intercept this packet, encrypt it, and re-route it to the VPN server's public IP. Then the VPN server rips off the outer packet and sees the "real" destination IP you were connecting too

As such from an application/programming perspective you just connected to a private IP over the public internet. This is the 3rd and huge reasons VPN is used heavily in corporate and why a lot of VPN explanations use private networks as their examples.
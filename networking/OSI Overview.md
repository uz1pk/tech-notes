## What is it

Really this section is talking about the TCP/IP model which ignores anything higher than layer 4 (transport layer) since everything is somewhat irrelevant and is only operated one once data has successfully moved from one host to another

It's just the abstraction for how networking should be organized and thought about. When sending data from 1 host to another host, i.e. host <> host communication.

It's important to note at each layer below a "header" (some binary) is added to the data to help distinguish source/destination info at each layer

**OSI Summary**: [Picture here](../pictures/osi-summary.png)
## Layer 1 - Physical
- Devices: Wires, Ethernet, Wifi, etc
- The goal is to move raw bits directly from one machine to connected to another and is connected VIA datalink / NICs
## Layer 2 - DataLink (Hop 2 Hop)
- **MAC Address**: 48 bit, 12 hexadecimal string representing the unique identifier for any NIC devices or similar datalink layer devices. `00:1A:2B:3C:4D:5E`
- **Frame**: Packet + source and destination MAC Addresses header (L2)

- Sends/Retrieves bits from the physical layer to the next connected device. 
- Devices: Network Interface Card (NIC), Wifi access card, etc. 
- Determines next computer to send to via MAC addresses.
- Manages all machine to machine hops
## Layer 3 - Network (End 2 End)
- **IP Address**: is a 32 bit, 4 octet string representing the address of some machine within a network (internet, private network, etc). `172.217.160.142`
- **Domain Name System** (DNS): A protocol that maps a domain name to an I.P. address (L3). Ex. google.com -> 172.217.160.142
- **Packet**: Segment + source and destination I.P. Address header (l3)

- Devices: Hosts and routers are all network level technologies since they can have IP adresses
	- Routers forward packets not meant for itself. Hosts are not routers
	- Routing tables and protocols [here](../pictures/routing.png)
- Follows the IP Protocol uniquely identifying a machine within some network (internet, subnet, local network, etc.)
## Layer 4 - Transport (Service 2 Service)
- **Segment**: Data + Layer 4 header. Aka source and destination TCP/UDP + Port header
- **Port**: 0-65536 are the port numbers available via TCP or UDP.
- **TCP**: A strategy for distinguishing data streams. TCP favors reliability and correctness
- **UDP**: A strategy for distinguishing data streams. UDP favors speed and efficiency

- When data is sent back and forth between 2 hosts it determines which program/service a data stream the data belongs too. 
- Let's say I have Fortnite + Valorant open, both needed their independent data over the network, L4 determines who that data/data stream belongs too. [See this picture](../pictures/osi-l4-ex.png)
- Uses an addressing scheme to determine which program gets which data stream using **Ports** similar to how L2/L3 use IP/MAC
- Each port uses either the TCP or UDP strategy to determines how to handle each data stream and map it to the correct service/program
## Layer 5/6/7 - Session/Presentation/Application
- In the OSI model these 3 layers are meant ot be distinct. But in modern day applications the lines are so blurred people ended up using the TCP/IP model to represent the internet/networking. [See picture here](../pictures/osi-tcpip-diagram.jpg)
## Layer interactions and others
- When connecting a computer to the internet. 3 things are needed to do this
	- IP address for host machine
	- Subnet mask
	- Default Gateway IP (Router)
		- The default gateway is used if a computer knows the IP it is trying to reach is on a different network.
		- It knows its on a diff network by comparing it's own IP with its subnet mask
### L3 <> L2
- When creating L3/L2 headers, the first thing needed to be confirmed is whether or not the IP is on a foreign or local network.
- Subnetting / Subnet Mask
	- Remember IP addresses are logical groups of networks split by octets. XX.XX.XX.XX.
	- Using this we can logically group networks using each of the octet pairs
	- Subnetting classifies that all IPs matching something like 117.54.X.X should all be routed to router X. This makes it more efficient and less storage for routers routing tables since they do not have to store each individual IP address. Rather you have a router designated to some network and logically divide the IPs via subnet for the routers to handle
	- This is called Route Summarization
	- [Picture here](../pictures/basic-subnet.png)
- **Address Resolution Protocol (ARP)**: Links IP address to a corresponding MAC address based on the current host/machine that has the packet.
	- Within the same network ARP does this by sending an ARP request to all MAC addresses it's conncted to asking all of them "hey is this the machine <ip_adress>'s MAC address". And at least 1 machine will respond yes and return the MAC address. 
		- The ARP request itself is just a data payload which the machines NIC knows how to unpack
		- The L2 (datalink) header sent with the payload is the mac address `ffff.ffff.fff`
		- `ffff.ffff.fff` is a reserved mac address meant to signify a request is for everyone
		- ARP cache stores mapping of IPs to MAC addresses on the host
		- On the response Host B will send directly to host A since it knows the destination based on the header above which included a source mac address
- [Encapsulation + Decapsulation based on above, picture here](../pictures/encap-decap.png)


# TLS/SSL

Going to cover TLS/SSL since it doesn't really belong in any of the other sections and is relatively straight forward to understand.
# Important notes

- For ANY device to have connectivity. It must have an assigned IP address, subnet mask, default gateway (router IP) and DNS server (Also an IP address) configureed.
- Useful docs for learning [Cloudflare Learning Center](https://www.cloudflare.com/en-gb/learning/
- )


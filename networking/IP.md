# What is IP

As mentioned in the overview an IP address is a unique logical address assigned to a device as a unique identifier on a network (a network is just an abstraction of interconnected hosts).

IPv4: has the form `255.255.255.255` (4.2 billion different IPs)
- each portion delimited by `.` is an octet. An octet is simply 8 bits used to represent a decimal number (11111111 == 255)

IPv6 has the form `2001:0db8:85a3:0000:0000:8a2e:0370:7334` ($\text{2}^{128}$ unique IPs available)
- You may also see the trailing 0's being omitted from the address

The governance and hierarchy of assigned public IPs is as follows:
1. IANA (global level)
2. Regional Assignment (RIRs)
3. Local Distribution (ISPs

The reason you cannot just "take" a public IP is because all traffic from some local network at some point needs to go through an ISP or greater when communicating over the internet. And unless you can literally hack ISPs network infrastructure there routers will recognize your "stolen IP" as the one it rightfully belongs too
# Subnet masks and CIDR (same thing)

**History**: Before private IPs were invented, public IPs were being wasted. Since the only way to assign IPs was the class system. AKA giving a network the first 8, 16 or 24 bits (1 - 3 of the octets in the IP) of an IP address space (class a, b, and c). However this became an issue since jumping from class C (0 - 255 hosts) to class B (0 - 65,535) was wayy to big of a jump and most of the times IPs were being unused.

**BLUF**: Subnet masks/CIDR were then invented to more granularly split up IP addresses based on individual bits of the IP address to determine whether an IP address is on in or out of network.

**Problem**: You have a Class C address space (198.172.0.0 - 198.172.0.255). But you now need to support 500 devices / hosts within your network. You would then need to bump up to the next "class" size of (198.172.0.0 - 198.172.255.255) which amounts to 65k IPs even thought you only needed 250 more. Thus you are now wasting a  ton of IP address space for no reason and it would be more ideal if we can somehow use the IP address space 198.172.0.0 - 198.172.1.255 which would give us 500 hosts.

**Solution**: Subnet masking allows us to do this more granularly distinguishing between what IPs are in a local network topology or outside the network. Instead of jumping from class C to class B, we can specify how many of the bits in the 32-bit IP address represent a local network vs remote network.

**Example** (using CIDR notation): Let's say we start with a class C address space (which I will annotate using CIDR notation, see below), `198.172.0.0/24` (class C). instead of upgrading to class B (`198.172.0.0/16`) to get more host/in-network IPs, we can simply reduce the subnet mask to `198.172.0.0/23`. What this means is that: if the first 23 bits of an IP address matches the first 23 bits defined by our address space AKA the binary representation of (`198.172.0.xxx` and `198.172.1.xxx`) this would mean that the IP address is in-network. Otherwise if even 1 bit was different for example `198.172.3.xxx` then the router would recognize this as out of network since there is a difference in the "network section" of the IP address's bit representation

**CIDR vs Subnet masks Notation**: They are the exact same thing but represented in two different notations. It's worth noting while subnet masks used to allow non-contiguous 1's, meaning some bits can represent hosts and later bits can represent networks. Now all "network" bits must be contagious and specified before host bits as you see below
**CIDR**: `198.172.0.0/24` (This means everything from 198.172.0.0 - 198.172.0.255 is the local network address space)
**Subnet masking**: 
- Address space: 198.172.0.0
- Subnet mask: 255.255.255.0 (same thing as CIDR but is more explicit)

An important thing to note that even though a subnet mask could be something like 
(`255.255.255.167` == `11111111.11111111.11111111.10100111`) 
this is no longer allowed in modern networking as the subnet mask/CIDR notation must have all 1's in contiguous order  before any 0's
# Private vs Public IPs

[RFC 1918](https://datatracker.ietf.org/doc/html/rfc1918) introduces private IPS

There are private IP ranges designated ONLY for private networks and are not accessible over open internet. Meaning if you attempt to access one of these private IPs your router will attempt to route the request as a "public IP" and get rejected at the ISP/Internet level

This allows for better reuse of IP addresses since something like `192.168.0.0` can be used N different times where N is a unique private network.

The range includes:
Class A: `10.0.0.0 to 10.255.255.255`
Class B: `172.16.0.0 to 172.31.255.255`
Class C: `192.168.0.0 to 192.168.255.255`

Because of the invention of Private IPs which came after the invention of subnet masking, we no longer actually need subnet masks to divide up public IPs since we can just use private IPs for networks that don't need to have devices accessible to the internet publically (example your phone connected to your home wifi)

In modern networking subnet masking is now used to simply better logically divide up address space in networks/subnets to achieve
- better debugability
- efficiency (example broadcast calls in IP / network layer)
- security / isolation to segregate traffic coming in / out
- scaling etc. etc.
# NAT (Network Address Translation)

Responsible for translating Public <> Private IP translations to allow for private network IPs to communicate to the public internet over a single public IP address

The same way `host <> host` direct communication uses 5 tuple networking / sockets to determine which connection belongs to which process in an OS/machine.
NAT on routers will do the same thing except instead of processes it is mapping private IP + port to public IP + port. Thus when the router receives data from a public server, it uses the IP + port pair to figure out which host + port on the private network that packet belongs too, and vice versa.
# DHCP (Dynamic Host Configuration Protocol)

[RFC 2131](https://datatracker.ietf.org/doc/html/rfc2131) Covers the details of how to implement the DHCP protocol

DHCP is a server protocol who's main purpose is to assign devices on a network (example home network) and IP address, default gateway, subnet mask, and DNS server IP for network communications

**History and Problem**: Previous for every new computer on a network, you'd need to first assign the default gateway, subnet mask, and DNS server manually on the  computer than update the config of the default gateway to now be aware and add the new devices IP to the IP table. This became increasingly repetitive and tedious for now reason

**Solution**: A server protocol whos' main purpose is to assign hosts on a network all the information they need to have connectivity alongside managing state of which device an IP address belongs too. Hence the name dynamic IP which is "dynamically" assigned by a DHCP server to hosts on a network

An important thing to note is that once a DHCP server assigns a host a dynamic IP. That IP is actually on a *lease* meaning after some specified number of time, the DHCP server will remove the IP address of any host that has not requested a *renew* on it's leased IP.

You can also manually go into DHCP server settings and set a *reservation*. This just means that you can specify that any device with X mac address MUST be assigned the IP you specify

**DORA process**: In order to find networks to connect to and receive DHCP data, a process much occur at layer 2 of the OSI model in order to discover networks to begin with. 
1. **Discover**: The DHCP client (e.g., your computer) sends a broadcast message to the network to find available DHCP servers and request an IP address. 
2. **Offer**: A DHCP server on the network receives the Discover message and responds with an Offer message. This message contains a proposed IP address, along with other network configuration details like the subnet mask and default gateway. 
3. **Request**: The client receives the Offer and selects the best one, then sends a broadcast Request message back to the server, asking to be allocated the offered IP address. 
4. **Acknowledge**: The selected DHCP server confirms the assignment by sending an Acknowledgement (ACK) message to the client. The client then configures its TCP/IP settings with the received information and can start communicating on the network
# DNS (Domain Name System)

Responsible for translating domain names like `google.com` to actual IP addresses
### DNS over HTTPS or TLS

TODO
### Dynamic DNS

TODO
# Layer 3 VPNs

TODO

# IP Protocols

TODO: https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers

# Routing Protocols

- BGP (Border Gateway Protocol)
- EGP (Exterior Gateway Protocol)
### ASN (Autonomous System Number)

TODO
# VLAN (Virtual Local Area Network)

VLAN is simply logically dividing a physically connected network into distinct "virtual" or fake networks that are still physically connected to improve network traffic, broadcasting, security, and making the network topology simpler and easier to handle, etc.
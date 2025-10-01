# What is TCP

TODO
# What is UDP

TODO
# What is a Port

Simply a logical connection on a host (computer) to determine which process/service has access to send/receive on that port. It's managed by the OS and generally there is a port table / mapping of ports being used alongside 5 tuple connection representations (source IP, source port, destination IP, destination port, L4 protocol like TCP/UDP)

Port ranges include:

- **System ports** (0 - 1023), ex. 443, 80, 21 (ftp), etc.
- **Registered ports** (1024 - 49151), apps like adobe, mssql, etc. can request designated ports for their apps from the IANA. Basically allowing them to tell users "well you have to use port N for my app otherwise you cant". More of a formality seems like + implementation detail but you can freely use these ports if they're unused
- **Dynamic ports** (49152 - 65535), free to use ports for anything and also what the computer uses to temporarily assign for connections. These should be used in a ephemeral / short-lived manner
# Port forwarding

The NAT protocol allows routers to having a table/mapping entry of private IP/Port to public IP/Port. However what if ingress is required? Aka some IP on the internet wants to communicate with your computer which is in a private network. At a high level port forwarding tells your router / gateway that if any network requests come into the router's public IP on a specific port, to route that traffic to a specific host on a network. [See an example here](../pictures/port-forwarding.png)
# Loopback interface

Also known as localhost, is simply a interface in your machine (OS level) that implements and acts like a network. So you can make "network" requests to your own machine without ever going over even L1 networking.

Generally the address reserve for the loopback interface is: `127.0.0.1`
# Layer 4 VPNs

TODO
## TCP over TCP meltdown

TODO
## UDP over TCP tunneling

TODO

## Why UDP is good for VPNs

TODO

# TCP FSM

TODO

### Three way establishing handshake
- Syn
- Syn-Ack
- Ack

### Four way closing handshake

TODO
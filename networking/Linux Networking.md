# What is a socket

A software API or abstraction for some Layer 4 connection (TCP, UDP, etc.) that is managed by the operating system. Without sockets it would be up to the programmer to implement, IP packet creation, managing connections, assigning connections, managing headers, handling ACKs, sequencing and transmission which would cause a lot of friction, overhead and burden.

Thus a standard interface is provided that gives you a connection to send/receive data over and the details of layer 4 networking and below are all abstracted and handled by the OS without even having to worry about the underlying protocols being used.
# 5-tuple networking

Layer 3 makes sure a packet reaches the correct host, but the host still needs to know which process (or thread) the data belongs to. That’s where Layer 4 protocols and the 5-tuple system come in.

**Ports** are just numbers that represent ownership of a connection. For example, if one process uses port 8080, no other process on the same host can use that port at the same time.

**5-tuple networking** is how the operating system keeps track of TCP/UDP connections.

A **socket** is simply the programming interface (API) that lets applications send and receive data using Layer 4 protocols. Behind the scenes, the OS maps each socket to a real network connection using the 5-tuple:

`(source IP, source port, destination IP, destination port, protocol)`
This tuple uniquely identifies a network session between a local process and a remote process, and the OS manages it automatically.
# tcpdump

TODO
# TUN/TAP devices

TUN/TAP devices are virtual / fake interfaces that represent physical hardware, this unlocks the ability for programmers to interface with the kernels network stack to add any custom logic they'd like to implement.

### TUN (L3 virtual network interface)

A TUN device exposes a L3 virtual network interface which allows programs to listen for packets on the allocated TUN device (Same way you can listen on specific ports using the sockets API). You can then assign an IP address to that device alongside configure in your system's kernel what destination IP addresses should be routed to your TUN device.
### TAP (L2 virtual ethernet interface)

Same thing as the TUN device except it works at the L2 level. Instead of working with IP packets it works directly with L2 ethernet data frames. This layer is generally used in VMs, simulated LANs by bridging virtual hosts.
# netstat

TODO
# ping

- Implements ICMP protocol

TODO
# Containers vs VMs

TODO
# BPF and eBPF

TODO

# User vs Kernel Space

User space == programs running outside of the operating system's kernel (non-privileged)
- No direct access to physical memory (rather APIs exposed by OS, malloc for ex)
- No direct access to all hardware (also APIs)
- More important no/little control over CPU, other resources and security
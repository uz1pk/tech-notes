# What is a socket

# 5-tuple networking

Layer 3 makes sure a packet reaches the correct host, but the host still needs to know which process (or thread) the data belongs to. That’s where Layer 4 protocols and the 5-tuple system come in.

**Ports** are just numbers that represent ownership of a connection. For example, if one process uses port 8080, no other process on the same host can use that port at the same time.

**5-tuple networking** is how the operating system keeps track of TCP/UDP connections.

A **socket** is simply the programming interface (API) that lets applications send and receive data using Layer 4 protocols. Behind the scenes, the OS maps each socket to a real network connection using the 5-tuple:

`(source IP, source port, destination IP, destination port, protocol)`
This tuple uniquely identifies a network session between a local process and a remote process, and the OS manages it automatically.
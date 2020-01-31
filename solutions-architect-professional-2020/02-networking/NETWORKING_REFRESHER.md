# Networking Refresher

## The 7 Layers of OSI Model

Networking exists allows person A to communicate with person B. Person A can be a server and Person B is generally a user running an application (website).

The 7 Layer model allows communication across devices though they may be on different versions of IPv4.

Each of the following layers are self contained. For example in the example above we don't care how the physical layer works as long as machine A and B both use the same one. In the the case above it's an ethernet cable.

7. Application
6. Presentation
5. Session
4. Transport
3. Network
2. Datalink
1. Physical

### Physical (1)

When Server A sends data to User B, the data coming from Server A will travel from the Application layer on that machine down through the physical layer. Then User B will receive the data through the physical layer and travel up to the application layer.

At the physical layer machine A does not know about machine B all it knows is how to communicate and send data across the physical layer. The two machines can talk to each other even though they don't know what each other are.

### Data Link (2)

At the data link layer is where machine A can know (assuming it knows the MAC address) of machine B. 

At this layer there are frames of data that contain the mac address. This layer also is in charge of sequencing and flow control, meaning that machine B can communicate to machine A that it is sending data too fast and machine A can slow it down. In addition if data has been damaged, this is where correction can happen as the data link layer will know and can rend the data.

### Network (3)

At this point the machines are no longer communicating through frames, but now are communicating through packets. In addition they are no longer using MAC address, but are now using IP addresses.

The packets contain a source and destination on them, and often the machine that they are current on is not the final destination. For example with a router. 

Layer 3 does no error checking the protocols that use this are:

- IP
- IPX
- ICMP - ping

### Transport (4)

Here we no longer communicate through packets, we use segments or data segments. Host to host flow control is added and error checking.

Protocols
- TCP - the most reliable
- UDP - the most lean method of transit

There is no form of ongoing connection

### Session (5)

This is where we can create a connection between machines and keep said connection open. It also adds the concepts of ports, allowing for multiple services to be addressed on one host.

### Presentation (6)

### Application (7)

This is where server actually understand the protocols are, where as before they only knew that data was coming through on port 80 using TCP. Now at the application layer it understand the protocol is http. 

### The Flow

The application layer via the presentation layer passes data to the session layer. 

The transport layer and session layer accept the data and encapsulate them into a TCP segment. The original data block still exists it is just wrapped in a TCP header that contains source and destination ports. At this point there is no IP.

Then when the combined transport and session layer pass the data to the network layer it is then encapsulated in a IP packet that will contain the source address, the destination address, and protocol used. Using the network mask the network layer is capable of determining if the computer is on the local network.

If it is the destination is on the network it will go directly there otherwise it will use the route table to figure out where the packet should be sent.

At the datalink layer the data will then be encapsulated in a ethernet frame containing the mac address. Then the physical a layer will send that through the wire where the opposite process will occur.

## IPv4

The 4th generation of Internet communication.

Within an IPv4 network all devices have at least one IP address. 

An IP address is a 32 bit binary value that consists of 4 bytes. When viewed by humans it is preferred that each byte is represented as value ranging from 0 to 255. e.g. 10.0.0.1, as a result there are 2^32 possible values.

The subnet mask is how you can tell if an IP is on your current network. 

For example a /16 subnet mask would look something like this. 255.255.0.0 and if you were to compare the following IP addresses

- 10.0.32.1
- 10.0.0.1

They would be on the same network. Whereas if you used a /24 subnet mask

They would not since the 3rd octect is different.

Masks

- /8 - Class A
- /16 - Class B
- /24 - Class C

Every bit doubles the available addresses.

### Broadcast Address

The Broadcast address is the largest IP in a network and is used to send messages across the entire subnet.


Research.

- Broadcast VS host address
# Networking Foundations

## Basic Network Terms

- Network Interface Card (NCI) - This is what connects devices to the network
- Network protocol - set of rules that govern communication, computers must communicate in the same language to talk to one another.
- IP Address - numerical address assigned to every device.
- Domain Name System - converts names or domain names into IP addresses


Computer network can be two computers connected via an ethernet cable into their network interface cards sharing data.

A Router receives data from a device and decides where to send the data. Moving data from one interface to another. This can be over the wireless network or over the internet.

The internet consists of thousands of routers.

Switches give you multiple ports to connect directly into the router.

A DNS server maps domain names to IP addresses.

``` powershell
ping [website]
tracert [website]
```

Networks allow you to share resources across devices.

Networks consist of two main components. A phyiscal component and a protocol. For example an ethernet cable and the IP protocol.

### Network Characteristics

Topology

- Phyiscal - arrangement of cables
- Logical - the path that data is transferred in a network
- Speed measure of the datarate the bits per seconds
- Cost - cost of installing the network 
- Avaialablity - the time that the network is available in a year.

## OSI Model

This is important to troubleshooting and understanding protocols. It makes it easy to explain issues happening within a network.

It was introduced by ISO.

This is why different computer types and servers can communicate with each other.

1. Physical
2. Datalink
3. Network
4. Transport
5. Session
6. Presentation
7. Application

Advantages:
- Standards and interoperability
- Split Development - developers working at layer 7 don't need to know what happens on layers 3 through 1
- Quicker Development - The network engineer does not need to understand your website and the developer does not need to understand the network

When saving a file you don't have to understand the details what happens in the lower  layers. You don't need to know about iscsi.

### Application, Presentation and session

Application Layer (7) - network processes to applications. This is the application protocols such as FTP, HTTP, and FTP. This layer providers network services to application processes. The applications that you use use protocol to communicate with the lower layers of the OSI Model.

- Remote file access
- resource sharing
- directory services

Presentation Layer (6) - Ensures that data is readable by receiving system. Formats the data to be presented to the application layer. This layer also provides encryption.

Session Layer (5) - Establishes maintenance and termination of session applications. Allows two application processes to communicate with security, name recognition and logging.

- RPC
- NFS

### Transport

Ensures end to end connections. It will handle message segmentation, where it will split messages into smaller units. In addition handles transportation issues between hosts.

- TCP Transmission Control Protocol - Maintains reliability using the TCP 3-way hand shake. If a packet is missing it will be retransmitted. Think about a telephone call where you acknowledgement


- UDP User Datagram protocol - does not provide reliability and if packets are dropped they are lost, and are not retransmitted. More lightweight. Used for things like VoIP. Think about the postal service and that there is no guarantee of getting data.

Flow control - manages data transmission and prevents the receiving device to slow down transmission
Session Multiplexing - multiplying several  message stream or sessions into one logical link.

### Network Layer

Provides path determination and logical addressing, but relies on other protocols for reliability at higher levels.

Data delivery
- routes data packets

Layer 3 switches
- have router capabilities
- selects the best path to deliver data
    - OSPF (Open Shortest Path First)
    - BGP (Border Gateway Protocol)
    - IS-IS (Intermediate System to Intermediate System

### Data Link

Ensures that data has been formatted according to the rules of the physical layer. In addition provides error detection and in certain cases can fix physical layer errors.


Physical Addressing 

MAC address is burned into the device.

### Physical

Binary Transmission

Defines the electrical, mechanical, procedural and functional specifications for activating, maintaining and deactivating the physical link

### Host Communication

There is a process of conversion that occurs across hosts. Data is transferred from layer 7 all the way down to layer 1.

At each layer the data is encapsulated and at the second layer where frame check sequence is added to ensure that the data is not corrupted. Finally the data is turned into bits and sent across the wire. 

At the receiving end the process is reversed. This is known as de-encapsulation where the bits are read, and the headers are removed at each layer until the original data is received

Each layer only communicates with the same layer on the receiving end.

Physical layer deals in bits
DataLink layer deals in frames
Network layer deals in packets
Transport Layer deals in Segments


### TCP/IP

- Application
- Transport
- Internet
- Network Access

### Hybrid

- Application
- Transport
- Network
- DataLink
- Physical


## Binary

Important in networking

- Subnetting
- Accessing Lists

All computers function by using a system of switches that can be off/on 0/1.

128 = 10000000
255 = 11111111

### Binary vs Decimal

Decimal - Base 10
Binary - Base 2

### IP Address to Binary

IPv4
- used to identify a device on an IP network
- 4 octets in length
- represented in binary or decimal

10.1.23.19 = 00001010.00000001.00010111.00010011

### Hex

128 = 1000 | 0000 = 80
255 = 1111 | 1111 = FF

IP Addresses

10.1.1.1 => 0A.01.01.01

## IP Addressing
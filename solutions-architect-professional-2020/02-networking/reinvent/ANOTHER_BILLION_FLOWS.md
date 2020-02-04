# Another Day, Another Billion Flows (VPC)

VPC is all built on code.

## What is VPC?

Virtual Private Cloud that you can choose your network. You can launch EC2s, containers, databases etc.

If your VPC IP Range is 192.168.0.0/16 then all things launched within your VPC will start with 192.168.

You can connect the VPC to other networks through things like Direct Connect or VPN.

Every VPC comes with

- Full programmatic access, flow log support, and audit capabilities
- Built in DHCP and DNS service, private DNS
- Built in firewall (NACLs and SGs)
- 9001 byte MTU

VPCs are free

- Useful for dev, beta, ...
- Multi-VPC architectures
- Immutable infrastructure patterns

## How does all of this work?

VPC on the wire

EC2's run on physical hosts that have a virtual/hardware router at the hypervisor level.

AWS Proprietary for VLAN tech

Traffic

- Your IP Packet
- VPC encapsulation
- IP on the physical network

There are blackfoot edge devices that encapsulate and de-encapsulate traffic and route it routes inside and outside of the AWS network.

The mapping service is in charge of mapping virtual destinations to physical destinations. There is extremely fast look ups. The mapping service does caching, preloading, so that things are extremely fast.
Supports microsecond scale latencies.

## Flows?

- SGs include stateful connection tracking
- Flow logs give per ENI aggregated audit data
- Network LBs can load balance flows natively and transparently in the VPC network
- NAT gateway brings per flow stateful NAT to VPC

How does flow tracking work?

|Protocol | Source IP | Destination IP | Source Port | Destination Port| SEQ | ACK |
|-----|-----|-----|-----|-----|-----|-----|-----|
|TCP| 192.0.2.1 | 52.84.25.90 | 33763 | 443| 6532 | 34224|

UDP packets have datagram ids instead of seq and ack
ICMP (ping)

NAT Gateway and Network LB solve similar problems if you think about it.

### HyperPlane Nodes (Special EC2s)

Make transactional decisions of what happens to your packets. For NAT guarantees that connections to the same dest IP / port have unique source port. Fro NLB select the target instance or contain that should handle the connection and keep sending to that instance.

For security best practice hyperplane does not need to know about VPC mappings just flows

This is built for huge traffic volume.

Built to handle 1000 Gbps

Shuffle sharding

## Challenges

- Setup IPv6 routes
- Setup a flow log with VPC
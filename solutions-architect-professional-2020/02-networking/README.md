# Networking

Should know before proceeding

- Physical layout of AZs and regions
- VPC concept and how to create
- How to create private and public subnets
- Know what a NAT is and what "Disable source/Destination Checks means"
- Route table and routing terminology (default routes, local routes)
- IPv4 Addressing and Subnet Mask Notation (/16, /24, etc)
- Intermediate networking terminology (MAC address, port, gateway vs router)

https://en.wikipedia.org/wiki/OSI_model
## OSI Model

1. Physical Layer
2. Data Link Layer
3. Network Link Layer
4. Transport Layer
5. Session Layer
6. Presentation Layer
7. Application Layer

## Unicast vs Multicast

Most cloud providers do not allow multicast across the network.

Multicast is where a network card starts to broadcast across the network. If this was allowed this could cause issues across the network.

## TCP vs UDP vs ICMP

- TCP (Layer 4) - connection based stateful, and acknowledges receipt. This means that whenever there is communication through this protocol there needs to be a confirmation returned that the message was received. This is normally used with Web, Email, File Transfer.

- UDP (Layer 4) - Connectionless, stateless, and simple. No retransmission delays. This is for connections that you can afford to miss data that is being sent. Think Streaming and DNS.

## Ephemeral Ports

Short lived transport protocol ports used in IP communications. Above the well known ports (1024).

They are sometimes called dynamic ports.

The general range is from 49152 - 65535
    - Linux kernals generally use 32568 to 61000
    - Windows platforms default from 1025

These ports have NACL and Security Group implications.

## Reserved IP Addresses

- AWS uses certain IP address in each VPC as reserved and you are not allowed to use them. These 5 IPs are reserved in every VPC subnet.

e.g. 10.0.0.0/24
1. 10.0.0.0 - Network Address
2. 10.0.0.1 - Reserved by AWS for the VPC router
3. 10.0.0.2 - Reserved by AWS for Amazon DNS
4. 10.0.0.3 - Reserved by AWS for future use
5. 10.0.0.255 - VPCs don't support broadcast so AWS reserves this address

## AWS Availability Zones

- The physical to logical assignment of AZ's is done at the account level. This means that your us-west-2a could be in a different location from another persons us-west-2a.

## Network to VPC Connectivity

### AWS Managed VPN

An AWS managed IPsec VPC connection over your existing internet. Good for a quick and simple way to establish a secure tunneled connection to a VPC. This can also be used as a redundant link for direct connection or other VPC VPN.

Supports static routes or BGP peering and routing.
Dependent on your internet connection.

#### How

1. Designate an appliance to act as your customer gateway (usually a router)
2. Create the VPN connection in AWS and download the configuration file for your customer gateway
3. Configure your customer gateway using the information from the configuration file.
4. Generate traffic from your side of the VPN tunnel
5. Configure BGP routing (if needed)

### AWS Direct Connect

Dedicated network connection over private lines straight into AWS backbone.

This works well when you need a "big pipe" into AWS. This also provides a more predictable network performance and potential bandwidth cost reduction. Supports BGP peering and routing and has up to 10 Gbps provisioned connections.

May require additional telecom and hosting provider relationships and/or new network circuits.

#### How

**This will take time**

- Work with your existing Data Networking Provider
- Create a Virtual Interfaces (VIF) to connect to VPCs (private VIF) or other AWS service like S3 or Glacier (public VIF)

### AWS Direct Connect Plus VPN

IPsec VPN connection over private lines. This is good if you want added security of encrypted tunnel over direct connection. 

This should end up being more secure than Direct connect alone, though it will have a higher level of complexity.

You will need to work with your existing data networking provider.

### AWS VPN CloudHub

Connects locations in a hub and spoke manner using AWS's virtual private gateway. 

This is great to link remote offices for backup of primary WAN access to AWS resources and each other. 

This reuses existing internet connections and supports BGP routes to direct traffic. (e.g. use MPLS first then CloudHub VPN as a backup). Since you rely on the internet connections stability then this can cause issues for you if the internet goes down, this also means that there is no inherent redundancy.

#### How 

Assign multiple customer gateways to a virtual private gateway, wach with their own BGP ASN and unique IP ranges.

### Software VPN

You provide your own VPN endpoint and software, you must manage both ends of the VPN connection. This could be because of compliance reasons or you want to use a VPN option not supported by AWS.

This will give you the ultimate flexibility and manageability, but you must design any needed redundancy across the entire chain.

#### How 
Install VPN software via marketplace appliance or on an EC2 Instance. For example if you wanted to use OpenVPN

### Transit VPC

Common strategy for connecting geographically dispersed VPCs and locations in order to create a global network transit center.

Locations and VPC-deployed assets across multiple regions that need to communicate with one another.

This has the ultimate flexibility and managability but also AWS managed VPN hub and spoke between VPCs.

The con with this is that you must design for any redundancy across the chain.

#### How

Providers like cisco, Juniper networks,  and riverbed have offerings which work with their equipment and AWS VPC.

## VPC to VPC Connectivity

### VPC Peering

This links two VPCs together, this is useful when multiple VPCs need to communicate or access each others resources.

This uses the AWS backbone without touching the internet.

One thing of note is that transitive peering is not supported. This means that if the following VPCs have been set up with peering A -> B -> C, A cannot communicate with C.

#### How

VPC Peering request is made accepter accepts the request either within or across accounts.

### AWS PrivateLink

AWS provided network connectivity between VPCs and AWS services using interface endpoints.

This keeps private subnets by using the AWS backbone to reach other services rather than the public internet. These can be accedded over inter-region VPC peering.

#### Types

Interface Endpoint
- Elastic Network Interface with an private IP
- Uses DNS entries to redirect traffic
- Supports API Gateway, Cloudformation, Cloudwatch, etc
- Secured through security groups

Gateway Endpoint
- a gateway that is a target for a specific route. 
- It uses prefix lists in the route table to redirect traffic. 
- The supported products are Amazon S3 and DyanmoDB.
- The security is through VPC endpoint policies

#### How

Create an endpoint for the needed AWS or marketplace service in all needed subnets, then access via the provided DNS hostname.

## Internet Gateway

Horizontally scaled, redundant and highly available allowing communication between your VPC and the internet.

There are no availability risks or bandwidth constraints

If you subnet is associated with a route to the internet, then it is a public subnet

It supports IPv4 and IPv6

The internet gateway provides NAT for instances with 
public IP addresses.

### Egress-Only Internet Gateway

- IPv6 addresses are globally unique and are public by default
- Providers outbound internet access of IPv6 addressed instances
- Prevents inbound access to those IPv6 addresses
- Stateful - forwards traffic from instance to internet and then sends back the response
- Must create a custom route for ::/0 to the egress-only internet gateway
- Use egress-only internet gateway instead of NAT for IPv6

## NAT

### NAT Instance

- EC2 Isntance with AWS provided AMI
- Translates traffic from many private IPs to a single public IP and back
- Doesn't allow public internet communcation to private instances 
- Not supported for IPv6
- Must live a public subnet with a route to the IGW
- private instances in the private subnet must have route to the NAT instance usually the default route destination of 0.0.0.0/0

### NAT Gateway

- Fully managed NAT service that replaces NAT Instances
- Must be created in the public subnet
- Uses an elastic IP for public IP for the life of the gateway
- Private instances in the private subnet must have route to the NAT Gateway usually the default route destination of 0.0.0.0/0
- Created in specified AZ with redundancy in that zone, create a gateway in each zone for multi-AZ redundancy
- Up to 5Gbps bandwidth that can scale up to 45 Gbps
- You cannot use a NAT gateway to access VPC peering, VPN or direct connection. You need to add the specific routes with the most specific routes first

### NAT Gateway vs Instance

Availability -  is highly available within the AZ / on your own.

Bandwidth - up to 45GB / depends on the bandwidth of your instance type

Maintenance - Managed by AWS / On your own

Performance - Optimized for NAT / AMI configured to perform NAT

Public IP - Elastic IP that cannot be detached / Elastic IP that can be detached

Security Groups - Cannot be associated / Can use Sgs (It's just an EC2)

Bastion Server - Not supported / Can be used as a bastion server (it's an EC2)

The recommendation is NAT Gateway because it's managed and highly available by default.

## Routing

VPCs have implicit routers and main routing table. You can modify this main routing table or create new tables. 

Each route table contains a local route for the CIDR block. Most specific route for an address wins.

### Border Gateway Protocol

- Popular routing protocol for the internet
- Propagates information about the network to allow for dynamic routing
- Required for direct connect and optional for VPN
- Alternative of not using BGP with AWS VPC is static routes
- AWS supports BGP community tagging as a way to control traffic scope and route preference
- Requried TCP port 179 + ephemeral ports
- Autonomous system number ASN = unique endpoint identifier
- Weighting is local to the router and higher weight is preferred path for outbound traffic

## Resources

- [AWS Global Networks](https://www.youtube.com/watch?v=uj7Ting6Ckk)

## Challenges

- What is hub and spoke?
- What is AWS Virtual Private Gateway
- What are BGP routes?
- What is dyanmic routing?
- What are static routes?
- What is MPLS?
- What is ASN?
- Why would you want to use OpenVPN on AWS?
- How do you add OpenVPN on AWS?
- More details about why you would want to do a transit VPC? How is Transit VPC used to connect to other cloud providers.
- Setup VPC peering in your account
- Setup an AWS private link
- Interface Endpoint vs Gateway Endpoint
- What is the advantage of connecting to S3 through a Gateway Endpoint? Are there any disadvantages?
- What is a IGW, more detail than public communication with the internet?
- What is NAT? Network Address Translation.
- What is Network Address Translation?
- How do routing tables work?


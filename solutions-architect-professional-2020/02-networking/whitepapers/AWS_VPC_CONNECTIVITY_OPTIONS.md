# Amazon Virtual Private Cloud Connectivity Options

## User Network to Amazon VPC Connectivity Options

This allows seamless connection to resources hosted on AWS. It's best that you ensure that there are no overlapping IP addresses with your network when connecting to a VPC. Thus you should ensure that you have a non overlapping CIDR block.

### [AWS Managed VPN](https://docs.aws.amazon.com/whitepapers/latest/aws-vpc-connectivity-options/aws-managed-vpn-network-to-amazon.html)

Establishing a VPN connection from your network equipment on a remote network to AWS managed network equipment attached to your VPC.

AWS Managed IPsec VPN connection over the internet.

Advantages:
- Reuse existing VPN equipment
- Reuse existing internet connections
- AWS managed endpoint includes multi--data center redundancy and  auto failover
- Supports static routes or BGP peering and routing policies 

Limitations:

- Latency, variability and availablity dependent on internet conditions
- Customer managed endpoints are responsible for redundancy and failover
- Customer device must support single hop BGP (when using BGP for dynamic routing)

Use multiple user gateway connections (Customer Gateways?) so you can have redundancy on your side of the VPC connection.

The user gateway device must be able to terminate both IPSec and BGP connections.

Direct Connect is a globally available resource that can be created in any public region and accessed from all other public regions.

### AWS Direct Connect

Describes establishing a private logical connection from your remote network to Amazon VPC, leveraging AWS Direct Connection

Advantages:

- Predictable Network performance reduced bandwidth costs
- 1 or 10 Gbps provisioned connections
- Supports BGP peering and routing policies

Limitations:
- May require additional telecom and hosting provider relationships or new network circuits to be provisioned


### AWS Direct Connect + VPN

Describes establishing a private, encrypted connection from your remote network to Amazon VPC, leveraging AWS Direct Connect.

Same as direct connection, but with secure VPN connection and the VPN adds additional complexity

### AWS VPN CloudHub

A hub and spoke model for connecting remote branch offices

Advantages:
- Reuse existing internet connections and AWS VPN connections
- AWS managed VGW includes multi-data center redundancy and automated failover
- Supports BGP for exchanging routes and routing pritorties

Limitations
- Latency and availablity dependant on the internet
- User managed endpoints are responsible for redundancy and failover

### Software VPN

Establishing a VPN connection from your equipment on a remote network to a user managed software BPN appliance running in AWS

This supports more vendors and offers more flexibility at the responsibility of the customer for HA for all VPN endpoints

This option is for when you must manage both sides of the VPN connection. This can be done with any VPN technology i.e. Astaro, Check Point, OpenVPN, etc.

### Transit VPC 
This should be replaced with Transit Gateway.

Global transit network on AWS using software VPN with AWS managed VPN.

Same as previous section with the addition of VPN connections between hub and spoke VPCs.

## AWS VPC to AWS VPC connectivity options

### VPC Peering

The AWS recommended approach for connecting multiple VPCs within and across regions.

Advantages:
- AWS networking without a VPN instances
- No single point of failure
- No bandwidth bottleneck

Limitations:
- VPC peering does not support transitive peering relationships

### Software VPN 

User managed software appliance based VPN connections between AWS VPCs

Advantages:
- Leverage AWS equipment 
- Supports more VPN vendors, products
- Managed by you

Limitations:
- You are responsible for HA for all VPN endpoints
- VPN instances can be a bottleneck

### AWS Managed VPN

Connecting multiple VPCs leveraging multiple VPN connections between your remote network and each of your Amazon VPCs

It's best to ensure that between your AWS VPCs there are no overlapping IP ranges for each VPC connected.

Advantages:
- Reuse existing VPC VPN connections
- AWS managed endpoint includes redundancy and failover
- Supports static routes and dynamic BGP peering and routing policies

Limits:
- Network latency, variability and availability based on internet conditions
- The endpoint you manage is responsible for redundancy and failover

### Software to AWS Managed VPN

Connect multiple VPCs with a VPN connection established between a user managed software VPN appliance in one VPC and AWS managed network equipment attached to the other VPC

Advantages:
- Leverage AWS equipment 
- redundancy and failover

Limits:
- responsible for HA
- possible bottleneck

### AWS Direct Connection
Conenct multiple VPCs leveraging logical connections on customer managed Direct Connect routers

Advantages:
- Consistent Netowrking
- Reduced bandwidth costs
- 1 to 10 Gbps provisioned connections
- Supports static routes and BGP peering and routing polices

Limits:
- May need additional telecom and hosting provider relationships
### AWS PrivateLink

Connection multiple VPCs leveraging VPC interface endpoints and VPC endpoint services

## Internal User to Amazon VPC Connectivity Options

### Software Remote-Access VPN - A Remote access solution for end-user VPN access into an Amazon VPC

## Challenges

- Setup a [Transit Gateway](https://aws.amazon.com/transit-gateway/)
- Setup an VPN connection into AWS with OpenVPN

- Why would you want to multicast?
- What is the difference between customer managed endpoints and aws managed endpoints with an AWS managed VPN?
- What is an MPLS network?
- How do you setup a customer gateway?
# Networking Many VPCs: Transit and Shared Architectures

What is a VPC? A virtual network that closely resembles a traditional private network.


What are the differences between traditional and AWS VPC?

- Ease of creation
- Access Models, there is a single access credential to everything
- Diverse ownership

Larger VPCs or accounts - Policy and IAM focused

- Less accounts and netowrks to setup
- Tighter control within the account or VPC
    - IAM roles
    - strict sg and routing
    - Identifying resources with tags
- Billing and ownership complexity
- Larger account or VPC blast radius

Smaller VPCs or accounts - Infrastructure and Networking

- More accounts and infrastructure setup
- Tighter control of provisioning and standards
    - Automation of infrastructure
    - AWS Direct Connect and VPN standards
    - Subnet routing standards
- Simpler Billing
- Smaller blast radius for users and networks
    - Larger blast radius for shared infrastructure and services

## VPC Designs

### Transit VPC

- Centralization
- Higher scale
- Inter region connectivity
- Firewall insertion
- Encryption everywhere

The Transit VPC Hub starts with your VPC and subnet in each availability zone. In each subnet there will be an instance running VPN software.

Then create a VPN connection from each of the instances into the other VPC with BGP for dynamic routing. These connections will consist of two VPN tunnels.

You will have the option to propagate routes from the VPN instance into different subnets. On each route table you have the option to propagate routes from the VGW (Virutal Private Gateway). This means VPN the IP will not change making this highly available. The VGW will advertise the VPC CIDR to the VPN instance.

How does the above break?

- If a VPN tunnel goes down traffic will automatically route over to the second tunnel
- If an instance goes down that failure will automatically be picked up within 30 seconds tops.

#### Automation through Cloudformation

- Uses Cisco CSR
    - Available  for Hourly billing
    - Full featured IOS-XE device
- Deployed in two AZ
- Support for duplicate tunnel addresses

- Create an S3 Bucket for the VPN config
- Create a prefix list route in the route table to be able to get to the config
- Setup EC2 Auto Recovery, meaning if the internet connection is lost the EC2 will automatically reboot

#### The Automation of VPC setup

Once the VPC has been created tag it with transitvpc:spoke=true

This triggers a lambda which will write the VPN config to an S3 Bucket which will be KMS encrypted

Once this config has been added to the S3 Bucket this will trigger a lambda to add the config to instances of the transit VPC to connection to the new VPC.

Removing this is a simple as setting the transitvpc:spoke to false then have the lambda pick up on that and delete the config. 

This will trigger the second lambda to update the instances within the transit VPC.

#### Costs

- Per Spoke
    - VPN Hourly Charges
    - VPN Egress Charges
- Hub Traffic
    - Spoke Destination Egress
    - Egress Charages
- Transit Instances
    - EC2 Charges

#### Performance

- Use Security Groups within the VPC
- Each spoke can forward 1.25 gps per VPN tunnel
- 100 spokes aggregate routes
- Each VPN instance can forward 1 - 3 gbps
- Use VPC peering for better performance

Use pods of independent transit VPCs
Connect the pods with with tunnels



### On-Prem Connectivity

#### VPN over the internet

This provides more control and visibility, its similar to the the Transit gateway approach with the addition of on prem VPNs into the Transit VPC.

#### VPN over AWS direct connect

Figure out what direct connect location you want to be in, Add fiber etc., Add a device there (Customer Gateway), Create a Virtual Private Gateway, and finally create a private Virtual interface.

- Great for encrypting connections or inserting services
- Greater control over latency and quality of connectivity due to direct connection into AWS

- This is manually configured

#### Detached VGW with AWS direct connect

Create a Virtual Private Gateway and Don't attached it to anything. Then create a VIF and attach it to the detached VGW.

Then create VPN connections from your instances to the detached VGW.

- This will create a similar layout that you would have when dealing with spokes in AWS.
- Traffic can take multiple routes out of AWS

- The traffic is unencrypted after it leaves the Virtual Private Gateway


#### Transit VPC with firewalls

Why would you add a firewall?

- We need a firewall for all traffic between on-prem and AWS
- Compliance requirement for intrusion detection in the VPCs
- Security Organization requires application level inspection
- We would like to centralize security appliances for any internet traffic


## AWS Direct Connect

For each VPC create a VIF (BGP + VLAN), you can get up to 50 VIFs per port. Which translates to 50 VPCs. For high availablity use two direct connect locations.

With up to 4 ports in a LAG each with 50 VIFs which gets you to 200 VPCs.

### Direct Connect Gateway

Allows up to 10 VGW per Direct connection Gateway. You can get up to 500 VPCs on a single physical port. With multi-region connectivity.


## Shared Services VPC

Think...
- Authentication
- Logging
- DevOps Tools
- Security Resources
- Deployed in each region

Then VPC Peering, this can be a bit heavy handed if you only want to talk to a single service.

Scales up to 125 VPCs

### Introducing: PrivateLink

Create a network load balancer that connects to each of the API instances. The network load balancer will get a single IP address for each AZ. From here create the endpoint and when connecting within the VPC the IP address of the API's will be mapped into the space of the IP range. This means it's a local IP address.

The access is unidirectional so the VPC can make requests to the private link API but not the other way around.

You don't have to worry about overlapping IP addresses.

This scales out to thousands of VPCs.

**Start here**

- Unidirectional
- Granular access
- Scales beyond 125 spokes
- Overlapping addresses

## Multi-region options

Inter-Region VPC Peering 

## 1000s of VPCs

Use the internet, it's scalable

- Secure protocols
- Secure authentication
- Bastion hosts
- Use PrivateLink

## Tips and Tricks

- Networking changes fast
    - You're billed by the second try it out and see how it works for you
    - Don't try to make something that will get you through the cloud for the next 7 years
- Segment as needed
    - Transit VPC for one section
    - Private Link for another
- Experiment and test
- These are starting points so mix and match

## Resources

- [EC2 Auto Recovery](https://aws.amazon.com/blogs/aws/new-auto-recovery-for-amazon-ec2/)
- [Private Link](https://aws.amazon.com/privatelink/)

## Challenge

What is a WAN?
What is a VGW?
What is a VIF?
Why are there two VPN tunnels when connecting using Transit VPC, is it for availability?

Communicate with an EC2 via a lambda.
Setup a transit VPC and connect to two spokes and have them communicate with each other.


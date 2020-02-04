# Security Best Practices for Your VPC

https://docs.aws.amazon.com/vpc/latest/userguide/security.html

- Multiple AZ deployments for HA
- Security Groups and NACLs
- IAM polices to control access
- CloudWatch to monitor VPCs and VPN connections
- Number rules in the NACL list to have room for additions i.e. 100, 200, 300

## Recommended Network ACL rules for your VPC

[Ephemeral Ports](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html#nacl-ephemeral-ports)

- If the max transmission unit (MTU) between hosts in your subnets are different add the following rule for inbound and outbound. Thus preventing packet loss and ensures path MTU discovery to work properly.
    - Select Custom ICMP Rule and type **Destination Unreachable, fragmentation required, and DF flag set**
    - For the port range type 3, code 4
    - If you use traceroute, also add the following rule: select Custom ICMP Rule for the type and Time Exceeded, TTL expired transit for the port range (type 11, code 0)

### Single Subnet

Inbound IPv4/IPv6:

**IPv6 is the same but with higer rule numbers and source IP ::/0**

- Allow
    - Ports: 80, 443, your specific ephemeral ports.
        - Any Source IP
    - Ports: 22, 3389
        - Your network IP range 
- Deny: everything else

Outbound

- Allow
    - Ports: 80, 443, and your specific ephemeral ports.
- Deny: everything else

Check out the link above for the specifics of the rest. The idea is that you limit the ability to connect where possible.

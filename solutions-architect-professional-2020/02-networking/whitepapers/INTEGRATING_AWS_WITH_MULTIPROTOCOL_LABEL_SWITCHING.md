# Integrating AWS with Multiprotocol Label Switching (MPLS)

https://d1.awsstatic.com/whitepapers/Networking/integrating-aws-with-multiprotocol-label-switching.pdf

**Introductions to MPLS and Managed MPLS Services**

https://tools.ietf.org/html/rfc3031

MPLS is an encapsulation protocol that instead of relying on IP loops  to find the "next hop". MPLS predetermines the path and uses label swapping push, pop and swap method to direct traffic to it's destination. This means more flexibility and greater SLA through latency and jitter reduction.

Requirements to establish an MPLS connection to AWS:

- Create a VPC
- Request an AWS Direct Connect connection
- Once completed you will recieve an email containing a Letter of Authorization, which will describe the circuit information at the Direct Connect facility
- If the MPLS provider has access to the AWS Direct Connect facility, they can establish the required cross connection based on the LOA. If the MPLS provider can utilize a direct connect partner to contain facility access
- Establish IP data connection routing between AWS, the PE device, and the customer's network
- Create a virutal interface to start using your direct connection
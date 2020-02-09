# AWS Best Practices for DDoS Resiliency

https://d1.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf?did=wp_card&trk=wp_card

## Introduction to Denial of Service Attacks DDoS

A deliberate attempt to take down your website or application by making it unavailable to users. Such as flooding it with network traffic.

In the simplest form the attacker will use a single computer to take down a target. This is known as a DoS attack.

In a Distributed Denial of Service attack, the attack uses multiple sources such as distrbuted groups of malware infected computers, routers, IoT devices, and other endpoints To orchestrate the attack.


Most common against layers 3,4,6, and 7 of OSI.

Network and transport layer attacks are known as infrastructure attacks.

Application and presentation layer attacks refer to application layer attacks.

- Application/Data - Network process to application. Http floods, DNS query floods
- Presentation/Data - Data representation and encryption. TLS abuse.
- Session/Data - interhost communication. N/A
- Transport/Segments - e2e connections and reliability. SYN floods
- Network/Packets - Path determination and logical addressing. UDP reflection attacks.
- Data Link/Frames - Physical addressing. N/A
- Physical / Bits - Media, signal, and binary trasmission. N/A. 

## Infrastructure Layer Attacks

UDP reflection and SYN floods, are infrastructure layer attacks. These may be easy to spot you have to be able to scale up to absorb the attack thus allowing you to receive real traffic.

### UDP reflection attacks

This attack takes advantage of UDP's stateless protocol (there is no three way handshake). Attackers can craft a valid UDP request packet listing the attack target's IP as the UDP source IP address. Thus the server will send the response to the faked IP rather than the attackers.

### SYN Flood Attacks

#### Three Way Handshake

When a user connected to a TCP service (web server). The client sends a SYN (synchronization) packet. The server returns SYN-ACK packet in acknowledgement, and finally the client responds with an ACK packet.


#### The Attack

In this attack  the malicious client sends a large number of SYN packets, but never the final ack packet. Thus leaving the server waiting for responses on half-open TCP connections and eventually runs our of capacity to accept new TCP connections.

As a result valid clients cannot connect because there is no longer the capacity to accept new TCP connections.

## Application Layer Attacks

The attacker attempts to overload specific functions of an application to make it unavailable or extremely unresponsive to real users.

Examples include: HTTP floods, cache-busting attacks, and word press XML-rpc floods

**This can sometimes be achieved with a low level of volume so it can be harder to detect.**

### HTTP flood attack

Sending HTTP requests that appear to be from a real user of the application. Since these can appear real is can be hard to mitigate through things like request rate limiting.

### Cache-busting attacks
A type of HTTP flood that uses variations in query string parameters to prevent CDN caching. This means that the requests must reach the application server.

### WordPress XML-RPC flood attack

AKA WordPress pingback flood, the pingback feature allows a website hosted on WordPress  to notify a different WordPress site through at link create from one site to another. The second site will attempt to fetch the first site to verify existence of the link.

The attack usually has WordPress present in the User-Agent of the http request header.

### Other Attacks

- Scraper bots that steal content or record competitive information, like pricing
- Brute force and credential stuffing attacks
- DNS query flood - exhausting the DNS server through many well-formed DNS queries
- TLS overload by sending unintelligible data since this is an expensive process it can overload the server.

## Mitigation Techniques

AWS Shield Standard by default (free) protects against most common network and transport layer attacks.

Amazon CloudFront and Route53 protect against availability and all known infrastructure layer attacks.

Benefits include:
- sub-second mitigation due to integration with edge services 
- stateless SYN Flood mitigation techniques that proxy and verify incoming connections before passing them to the protected service.
- automatic traffic engineering system that disperse or isolate large DDoS attacks
- Application layer defense when combined with AWS WAF

**There is no charge for inbound data transfer on AWS and you do not pay for DDoS attack traffic that is mitigated by AWS Shield**


25 gigabit network interfaces and enhanced networking allow EC2 instances to absorb the traffic.

Use AutoScaling to handle DDoS attacks and TLS negotiation is handled at the distribution layer therefore will be taken care of by CloudFront or your Load Balancer. 

Depending on your region (if you are close to major internet exchanges) you could have a larger internet capacity to mitigate larger attacks.

### ELB

ELB allows balancing of excess traffic, reducing the risk of overloading the application.

Web Applications use the ALB to route traffic based on content and accept only well formed web requests. Thus many common DDoS attacks, like SYN floods or UDP reflection attacks will be blocked by the ALB. **The scaling will not affect your AWS Bill**.

TCP Application use the NLB to route traffic to targets. Any traffic that reaches the LB on a valid listener will be routed to your targets, not absorbed. AWS Shield Advanced allows configuration of DDoS protection for EIPs. By assigning an EIP per AZ to the NLB, AWS Shield Advanced will apply relevant DDoS protections

### Edge Services

#### CloudFront

CloudFront persistent connections and variable TTL settings offload traffic from origins even if you are not serving cacheable content. Thus reducing the number of TCP connections to origins protecting from HTTP floods.

CloudFront only accepts well-formed connections preventing many common DDoS attacks like SYN floods and UDP reflection attacks from reaching origins.

Use CloudFront with an OAI on S3 buckets to protect your buckets

#### Route 53

By default uses shuffle sharding (each name server corresponds to a set of edge locations) and anycast striping (server from the most optimal location) that can help users use your application even targeted through DDoS attacks.

Route 53 can detect anomalies in the source and volume of DNS queries, and prioritize requests from users that are known to be reliable.

### Application Layer Defense

You will need to implement architecture that allows you to specifically detect, scale to absorb, and block malicious requests. This is important because network based DDoS mitigation systems are ineffective against complex application layer attacks.

### Detect and filter malicious web requests

When you application runs on AWS, you can leverage both CloudFront and WAF to defend against application DDoS attacks.

CloudFront
- Allows caching static content and serve it from AWS edge locations, thus reducing load
- Also helps reduce server load by preventing non-web traffic from reaching your origin. 
- CloudFront can automatically close connections from slow reading/writigin attackers

WAF
- Control Web ACLs on CloudFront Distros or ALBs to filter and block request based on request signatures. Such as URI, query string, http method, or header
- Rate based rules returning 403 to offending clients
- Create rules using IP Match conditions or use managed rules of malicious IPs included in reputation lists
- You can block attack originating for geographic locations where you do not expect to serve users
- Identify malicios request by reviewing your web server logs or using AWS WAFs logging and sampled requests features.

## Attack Surface Reduction

Limit the opportunities an attacker has for targeting your application. For example if users should not directly interact with certain resource make sure those resources are not accessible from the internet. Ensure that if users for external applications should not communicated with your application on certain ports or protocols make sure that the traffic is not accepted.

Use shared secret headers with CloudFront to ensure that requests came from CloudFront and went to your origin. http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/forward-custom-headers.html

### Protecting endpoints

Amazon API Gateway has a regional API endpoint that allows you to supply your own CloudFront distribution that you are allowed to customize. There is also the ability to use WAF for application layer protection.

- Configure the cache behavior for distributions to forward all headers to API Gateway regional endpoint. This will make CloudFront treat content as dynamic and skip caching the content
- Protect your API Gateway against direct access by configuring the distribution to include the origin custom header, by setting the API key value in the API gateway
- Protect backend from excess traffic by configuring standard or burst rate limits for each method in the REST APIs

## Operational Techniques

### Visibility

Use CloudWatch to monitor applications running on AWS. Check for metrics that deviate substantially from the expected value.


## Challenges

- What is shuffle sharding (Route 53)?

## Resources

- [AWS Shield](https://aws.amazon.com/shield/)
- [AWS Shield Advanced Protections](https://docs.aws.amazon.com/waf/latest/developerguide/ddos-overview.html)
- [AWS WAF](https://aws.amazon.com/waf/)
- [Geo Restriction](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/georestrictions.html)
- [Protecting Dynamic Web Apps Against DDoS](https://aws.amazon.com/blogs/security/how-to-protect-dynamic-web-applications-against-ddos-attacks-by-using-amazon-cloudfront-and-amazon-route-53/)
- [Enhanced Networking EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/enhanced-networking-ena.html#test-enhanced-networking-ena)
- [Choosing an AWS Region](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)
- [Protecting TCP Applications](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/network-load-balancer-getting-started.html)
- [Slowloris Attack](https://en.wikipedia.org/wiki/Slowloris_(computer_security))
- [Automatically Update You SGs for CloudFront and WAF Using Lambda](https://aws.amazon.com/blogs/security/how-to-automatically-update-your-security-groups-for-amazon-cloudfront-and-aws-waf-by-using-aws-lambda/)
# Web Application Hosting in the AWS Cloud

## How AWS can solve common web app hosting issues

AWS allows you to dynamically provision servers as needed vs the need to manually provision and stand up servers.

This means that during peak usage you can schedule, dynamically auto scale, or manually scale instances.

As a result you get less wasted capacity.

### On-Demand Solution for non production environments

Create environment fleets as necessary and tear them down as you don't need them.

## An AWS Cloud Architecture for Web Hosting

1. Load Balancing with ELB/ALB - allows you to spread load across multipel AZs and Amazon EC2 Auto Scaling groups for redundancy and decoupling of services
2. Firewalls with SGs - Moves security to the instances to provide a stateful, host-level firewall for web and app servers.
3. Caching with Amazon ElastiCache - provides caching services with redis or memcached to remove load from the app and database, and lower latency
4. Managed DB with RDS - Creates HA Multi-AZ database architecture with 6 possible engines
5. DNS Services with Route 53 - Provides DNS services for domain management
6. Edge Caching with CloudFront - Edge caches high-volume content to decrease the latency to customer
7. Edge Security for CloudFront with AWS WAF - filters malicious traffic, including XSS and SQL injection through customer defined rules
8. DDoS protection with AWS Shield - Safeguards infrastructure against most common network and transport layer attacks automatically
9. Static storage and backups with Amazon S3 - object storage for backups and static assets

## Key Components of an AWS Web Hosting Architecture

### Network Management

Host-level security through Security Groups, network isolation through VPCs.

### Content Delivery

CloudFront to deliver both static and dynamic content as well as streaming.

### Managing Public DNS

Use Route 53 for highly scalable DNS web service. This automatically routes queries for your domain to the nearest DNS server.

### Host Security

Security Groups are applied to the host level and represent an inbound network firewall. You are allow to specify protocols, ports other SGs, and source IP ranges.

### Load Balancing Across Clusters

ELB/ALB allow distribution of traffic much like hardware load balancers.

### Finding other hosts and services

Generally IP addresses will be dynamically assigned to your ec2 instances if you need consistent IP addresses use Elastic IP addresses.

### Caching within the Web Application

Amazon ElastiCache makes it easy to deploy, operate, and scale an in memory cache in the cloud.

### Database Configuration, Backup, and Failover

Managed Database Service through RDS for relational database solutions. For NoSQL SimpleDB, DynamoDB and DocumentDB

Self-Managed - hosting the database of choice on EC2

RDS supplies

- automatic patching
- backups
- point-in-time recovery
- multi-az

With self-managed solutions consider availability of fault-tolerant and persistent storage (EBS). In addition ensure your logs are placed on the EBS volume.

### Storing and Backup of Data and Assets

S3 for storage and CloudFront for caching and streaming of assets.

EBS volumes are create for block storage and can be as large as 16 TB. The volumes can be striped for even larger volumes or increased I/O performance. I/O is improved through Provisioned IOPS volumes.

### Auto Scaling the Fleet

This is done through the Auto Scaling service.

### Additional Security Features

Avoiding issues with DDoS attacks

- AWS' large infrastructure
- ELB
- CloudFront
- Route 53
- AWS Shield
- AWS DDoS Response Team (For Advance AWS Shield)
- AWS WAF - protects against availablity and security attacks. Works inline with CloudFront or ALB

### Failover with AWS

Hosting across multiple AZ creates an HA application.

## Key considerations when using AWS for Web Hosting

- No physical appliances such as routers and firewalls. These are replaced with software solutions.
- Firewalls Everywhere - NACLs and security groups on EC2 instances allow the level of security you desire.
- Through AZs you have multiple data centers at your disposal
- Servers should be treated as ephemeral (short lived, cattle)
-  Consider Serverless for your web architecture.


## Challenges

- Setup 
    - [SimpleDB](https://aws.amazon.com/simpledb/)
    - [DocumentDB](https://aws.amazon.com/blogs/aws/new-amazon-documentdb-with-mongodb-compatibility-fast-scalable-and-highly-available/)

## Resources

- [EBS Features](https://aws.amazon.com/ebs/features/)
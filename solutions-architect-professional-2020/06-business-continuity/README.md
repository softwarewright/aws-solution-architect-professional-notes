# Business Continuity

## Concepts

*There is always a chance that something will fail in AWS, architect your solutions to account for these failures.*

- Business Continuity - Seeks to minimize business activity disruption when something unexpected happens.
- Disaster Recovery - Act of responding to an event that threatens business continuity.
- High Availability - Designing redundancies to reduce the change of impacting service levels
- Fault Tolerance - Designing in the ability to absorb problems without impacting service levels.
- Service Level Agreement (SLA) - An agreed goal or target for a given service on its performance or availability.
- Recovery Time Objective (RTO) - Time that it takes after a disruption to restore business processes to their service levels.
- Recovery Point Objective (RPO) - Acceptable amount of data loss measured in time

Start with a **Business Continuity Plan** that determines the RTO and RPO allowed. The RTO and RPO drives the Disaster Recovery Plan. The investment that you place into High Availability can mitigate the need for intense Disaster Recovery.

### Disaster Categories

- Hardware Failure - network switch fails
- Deployment Failure - deploying a patch breaks the environment
- Load Induced - DDoS
- Data Induced - Data conversions causing failures
- Credential Expiration - SSL/TLS cert expiration
- Dependency - S3 goes down and causes other services to fail
- Infrastructure - construction crew cuts a fiber optic data line
- Identifier Exhaustion - Insufficient capacity in an AZ
- Human Error

## Disaster Recovery Architectures

### Backup and Restore

Pros:
- Very Common Entry point
- Minimal Effort to configure

Cons:
- Least flexibility
- Analogous to off site backup storage

### Pilot Light

Pros:
- Cost effective way to maintain a "hot site" concept
- Suitable for a variety of landscapes and applications

Cons:
- Usually requires manual intervention for failover
- Spinning up cloud environments will take minutes or hours
- Must keep AMIs up-to-date with on-prem counterparts

### Warm Standby

Pros:
- All services are up and ready to accept a failover faster within minutes or seconds
- Can be used as a shadow environment for testing or production staging

Cons:
- Resources would need to be scaled production load
- Still requires some environment adjustments but could be scripted

### Multi-Site

Pros:
- Ready all the time to take full production load-effectively a mirrored data center
- Fails over seconds or less
- No or little intervention required to fail over

Cons:
- Most expensive DR option
- Can be perceived as wasteful as wasteful as wasteful as you have resources just standing around waiting for the primary to fail

You can use a route53 healthcheck failover.

## Storage HA Options

### EBS

- Annual Failure Rate of less than 0.2% compared to commodity hard drives at 4%. (Example: given 1000 EBS volumes 2 will fail per year).
- Availability target of 99.999%
- Replicated automatically within a single AZ
- Vulnerable to AZ failure
- Easy to snapshot, stored in S3 and multi-AZ durable
- You can copy snapshots to other regions as well
- Supports RAID configurations

#### Raid Levels

RAID0

- No redundancy
- High Reads and writes
- 100% Capacity

RAID1

- 1 drive can fail
- Slightly slower
- 50% Capacity

RAID5 (Three drives)

- 1 drive can fail
- Slower writes and fast reads
- (n-1)/n

RAID6

- 2 drive can fail
- Fast reads and slow writes
- (n-2)/n

**AWS does not recommend using RAID5 and RAID6 because managing the parity bit can cause you lose up to 30% performance.**

### S3 Storage

- Standard 99.99 AV
- Infrequent 99.9
- One-zone Infrequent 99.5
- 99.999999999% Durability
- Backing service for EBS snapshots and many other services

### EFS

- NFS implementation
- True file system
- File locking, strong consistency, concurrently accessible
- Each file object and metadata is stored across multiple AZs
- Can be accessed from all AZs concurrently
- Mount targets are HA

### Other options

- Amazon Storage Gateway
    - Good way to migrate on-prem data to AWS for offsite backup
    - Best for continuous sync needs
- Snowball
    - Various options for migrating data to AWS based on volume
    - Only for batch transfers of data
- Glacier
    - Safe offsite archive storage
    - Long term storage with rare retrieval needs

## Compute HA Options

- Up-to-date AMIs are critical for rapid fail over
- AMIs can be copied to other regions for safety or DR staging
- Horizontally scalable architectures are preferred because risk can be spread across multiple smaller machines versus one large machine
- Reserved instances is the only way to guaranteed resources will eb available
- Auto Scaling and Elastic Load Balancing work together to provide automated recovery by maintaining minimum instances
- Route53 health checks allow "self healing" fail over

## HA Database Options

- If possible chose DynamoDB over RDS because of inherent fault tolerance
- If DynamoDB can be used Aurora because of redundancy and automatic recovery features
- If Aurora can't be used, Multi-AZ RDS
- Frequency RDS snapshots can protect against data corruption or failure and they won't impact performance of multi-AZ deployment
- Regional replication is also an option, but will not be strongly consistent
- If Database is on EC2, design HA yourself

### HA notes for Redshift

- Currently, Redshift does not support multi-AZ deployments
- Best HA option is to use a multi-node cluster which support data replication and node recovery
- A single node Redshift cluster does not support data replication and you'll have to restore from a snapshot on S3 if a drive fails

### HA Notes for ElastiCache

Memcached
- Memcached does not support replication, therefore node loss == data loss
- Use multiple nodes in each shard to minimize data loss on node failure
- Launch multiple nodes across available AZs to minimize data loss on AZ failure

Redis
- Use multiple nodes in each shard and distribute the nodes across multiple AZs
- Enable multi-AZ on the replication group to permit automatic failover if the primary node fails
- Schedule regular backups of your redis cluster

## HA for networking

- By creating subnets in available AZs, you create multi-AZ presence for your VPC
- Best pratice to create two VPN tunnels into your Virtual Private Gateway
- Direct Connect **IS NOT** HA by default, so you need to establish a secondary connection via another Direct Connect (another provider) or use a VPN
- Route53 health checks provide basic level of directing DNS resolutions
- Elastic IPs allow flexibility to change out backing assets without impacting name resolution
- For multi-AZ redundancy of NAT Gateways, create gateways in each AZ with routes for private subnets to use the local Gateway

## Exam Tips

General

- Difference between 
    - Business Continuity, DR, and SLA
    - High Availability and Fault Tolerance
    - RTO RPO
- Understand inter-relationships between the terms above
- Four general types of DR architectures and trade-offs of each


Storage Options

- HA capabilities and limitations of AWS storage options
- When to use each storage option to achieve required level of recovery capability
- Understand RAID and potential benefits and limitations

Compute Options
- Understand why horizontal scaling is preferred from an HA perspective
- Know that compute resources are finite in an AZ and know how to guarantee their availability (reserved instances)
- Understand how Auto Scaling and ELB can contribute to HA

Databases
- Know HA attributes of various database services
- Understand the different HA approaches and risks for memcached and redis
- Know which RDS options require manual failover and which are automatic

Network Options
- Know which networking components are not redundant across AZs and how to architect for them to be redundant
- Understand the capabilities of Route 53 and Elastic IP in context of HA

## Pro Tips

### Failure Mode and Effects Analysis (FMEA)

A systematic process to examine

1. What could go wrong
2. What impact it might have
3. What is the likelihood of it occurring
4. What is our ability to detect and react

Severity * Probability * Detection = Risk Priority Number

#### Steps

1. Round up Possible Failures
2. Assign Scores
3. Prioritize Risk


## Whitepapers

- [Reliability Pillar](https://d1.awsstatic.com/whitepapers/architecture/AWS-Reliability-Pillar.pdf)
- [Amazon Aurora](https://d1.awsstatic.com/whitepapers/getting-started-with-amazon-aurora.pdf)

## Reinvent

- [Models of Availability](https://www.youtube.com/watch?v=xc_PZ5OPXcc)
- [How to Design a Multi-Region Active-Active Architecture](https://www.youtube.com/watch?v=RMrfzR4zyM4)
- [Disaster Recovery with AWS](https://www.youtube.com/watch?v=a7EMou07hRc)

## Challenges
- Backup and restore a DynamoDB Table https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/BackupRestore.html
- Promote a read replica in RDS to main
- What is a SAN?
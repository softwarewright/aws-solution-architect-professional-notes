# AWS RDS Deep Dive

Engine Support - Amazon Aurora, MySQL, MariaDB, PostgreSQL, Oracle, SQL Server

## Why RDS? 

- Automated provisioning, patching, scaling, replicas, backup/restore
- Easily scales to handle growth
- High availability
- Lower TCO and focus on differentiating yourself as a company
- Even small startups can leverage multiple data centers to design highly available apps 

## Configuration

- Pick an engine
- Choose an instance type (Shoot for the latest instance types)
    - t series for burstable 
        - variable workloads
        - eligible for free tier
        - make sure to monitor burstable credits 
    - m series general purpose
        - balance of memory and cpu
        - High performance networking
        - Great for CPU intensive workloads like wordpress
        - Good for production workloads
    - r series memory optimized
        - like m's but 2x memory
        - good for query intensive workloads and high connection counts
- Choose storage types
    - GP2 (Burstable Like t2)
        - affordable performance
        - Monitor credits on volumes less than 1 TB
        - 
    - Provisioned IOPS (IO1)
        - High performance and consistency
    - Magnetic
        - Supported for legacy databases
        - Not recommended
        - You'll pay more for less consistency
- Scaling
    - Compute/memory vertically up or down
        - higher load to grow over time
        - lower usage control costs
        - minimal downtime
            - same endpoint, different IP 
    - Scales up EBS storage (Up to 16 TB)
        - EBS engines now support Elastic Volumes for fast scaling
        - No downtime for storage scaling
        - Older volumes can take longer
        - You can only scale once every 6 hours
        - IOPS can be reprovisioned on the fly 
    - High Availability
        - Multi-AZ providers enterprise fault tolerance
        - Automatic fail over, could take up to 2 minutes 
        - Simulate with reboot with failover 
    - Read replicas
        - relieve pressure on the source database
        - Bring data close to your applications in different regions
        - Promote to master for faster recovery in the event of disaster
        - Upgrade the read replica to a new engine version for testing


### Multi-AZ Vs Read Replicas

Multi-AZ
- Synchronous replication
- only the primary instance is active at anytime
- backups can eb taken from the secondary
- Always in two AZ's within a region
- Database engine version upgrades happen on primary
- Automatic failover

Read Replicas 

- Async replication - highly scalable
- All replicas are active and can be used for read scaling
- No backups configured by default
- Can be within an AZ, cross AZ, cross region
- Database engine version upgrades independently from source
- can manually promote to a standalone database 

### Managing Backups

- Automated and Manual
- Snapshots through EBS stored in S3
- Transaction logs are stored every 5 mins to S3 to support point in time recovery
- No performance penalty for backups
- Snapshots can be copied across regions or shared with other accounts

#### Automated Backups Vs Manual Snapshots

Automated
- Retention window per instance
- Kept until outside of the window 35 day max
- Supports PITR
- Good for disaster recovery

Manual
- Manually craeted
- Kept until you delete
- Restores to saved snapshot
- Use for checkpoint before making large changes, non-production/test environment, final copy before deleting a database

#### Restoring a backup (This can take a while)

Creates an entirely new database instance
- Define the instance configuration

New volumes are hydrated from S3

Since this can take a while to stand up it can be helpful to create the database with High I/O just to get started and then scale down once the database is live.

## Security

Secure By Default

- VPC - Isolate from the public routes on the internet. In dev environments it's ok to make this public
- IAM for permissions - MFA for extra security and IAM roles for general control
- Encryption at rest - No performance penalty for encryption. Audited for access.
    - Best practices
        - Encryption cannot be removed from the DB
        - Add encryption to an unencrypted DB instance by encrypting a snapshot copy
        - if the source is encrypted, read replicas will be as well.

## Monitoring

Cloudwatch metrics

- CPU/Storage/Memory
- Swap usage
- I/O
- Latency
- Throughput
- Replica lag

Enhanced Monitoring
- Access to over 50 CPU, memory, file system, and disk metrics
- Low as 1 second intervals

Integration with third party monitoring tools

### Service Events

RDS uses SNS to receive notification when an event occurs. These events can tell you things like:

- backup deletion
- configuration changes
- instance changes
- so much more...

## Improving Performance

- RDS Performance Insights
- Measures DB Loads: Average Active Sessions
- Identifies database bottlenecks
- Enables problem discovery
- Adjustable time frame
    - Hour, day, week, and longer

## Maintenance

- Any maintenance causes downtime
- operating system or software patches are performed without restart
- Database engine upgrades require downtime
    - Minor - automatic or manually applied
    - Major - Manually applied
    - Version deprecation - 3 to six notification before scheduled
- View upcoming maintenance events in your AWS Personal Health Dashboard

## Cost

- Instance hours
    - region + instance type + engine + license (optional)
- Database storage GB/Month
    - Provisioned or Consumed
    - Provisioned IOPs
    - Database I/O requests for Aurora and magnetic storage types
- Backup storage GB/Month
    - Size of backups and snapshots
    - No charge for backup storage up to 100% of total database storage
- Data transfer
    - Regional data transfer pricing

Use RDS Reserved Instances to provide a discount over on demand prices

- offers size flexibility for open-source
- by default RIs are shared between all accounts in consolidated billing

### Stop Database

- Stop and start a running database instance from the console or AWS CLI.
- When sopted you only pay for storage
- Instances are restarted after 7 days
    - Pending maintenance operations are applied
    - Instances can be stopped again if desired


## Challenge

- Find a deep dive on aurora
- EBS Scalable Volumes?
- Can cloud custodian stop/start rds databases
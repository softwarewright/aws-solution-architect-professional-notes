# Data Stores

## Concepts

- Persistent Data Store - Data is durable and sticks around after reboots, restarts, or power cycles
    - RDS
    - DynamoDB
- Transient Data Store - Data is just temporarily stored and passed along to another process or persistent store
    - SNS
    - SQS
- Ephemeral Data Store - Data is lost when stopped
    - EC2 Instance Store
    - Memcached

### IOPS vs Throughput

IOPS - measure of how fast we can read and write to a device

Throughput - measure of how much data can be moved at a time

**Consistency is far better than rare moments of greatness** -Scott Ginsberg

### Consistency Models

ACID
- Atomic - Transactions are all or nothing
- Consistent - Transactions must be valid
- Isolated - Transactions can't mess with one another
- Durable - Completed transaction must stick around

BASE
- Basic Availability - values available even if stale
- Soft-state - might not be instantly consistent across stores
- Eventual Consistency - will achieve consistency at some point

## S3

- S3 is an object store
- Max object size 5 TB; largest object in a single PUT is 5GB
- Recommended to use multi-part uploads if larger than 100MB

The "file path" of S3 is a key. It leans closer to a database rather than a file store.

### S3 consistency

- read after write for PUTS of new objects, you can read the object right after you write it to S3
    - There is a caveat if you attempt to request (GET/HEAD) the object before it exists then create it after, that read will be eventually consistent this is withing a few seconds
- head and get requests of a key before an object exists will be eventually consistent. Until the object has been written you will not be able to read it, but once it has you will be able to.
- eventual consistency on PUTs and DELETEs. The update or delete will happen locally, then the change will be replicated out, but until that is completed the old files will be served.
- Updates to a single key are atomic. Only one update can be processed at time and will be processed in the order they were received.

### Security

- ACL
- IAM
- Multi-factor before delete

### Protection

- Versioning
- Enables "roll-back" and "un-delete" capabilities
- You will be billed for storing old versions
- Integrated with life cycle management

### Cross Region Replication

- Security
- Compliance
- Latency

### S3 Life cycle Management
- Optimize storage costs
- Adhere to data retention
- Keep S3 volumes well maintained

### S3 Analytics

- Data Lake Concept - Athena, Redshift Spectrum, Quicksight
- IoT Streaming Data Repository - Kinesis Firehose
- Machine Learning and AI Storage - Rekognition, Lex, MXNet
- Storage Class Analysis - S3 Management Analytics

### S3 Encryption at Rest

- SSE-S3 - Use S3's existing encryption key for AES-256
- SSE-C - Upload your own AES-256 encryption key which S3 will use when it writes the objects
- SSE-KMS - Use a key generated and managed by AWS Kwy Management Service
- Client-Side - Encrypt objects using your own encryption process before uploading to S3 (PGP, GPG, etc)

### Other Features

- Transfer Acceleration - Speed up data uploads through edge locations
- requester pays - the requester rather than the bucket owner pays for requests and data transfer 
- Tags - assign tags to objects for use in costing billing and security
- Events - triger notifications to lambda, sqs, sns 
- static website hosting - simple and scalable static website hosting
- BitTorrent - use the BitTorrent protocol to retieve any publicy available object by automatically generating a .torrent file


## Amazon Glacier

- Cheap, slow to respond, seldom accessed
- Faster retrieval speed options if you pay more
- Think archival 

### Rules

- Glacier Vault Lock
    - Different from the vault access policy
    - Enforce rules like no deletes or MFA
    - Immutable, you can replace or delete but not change
    - This is for the entire vault
    - 24 Hours to complete or abort the lock
- IAM Access
    - Control who accesses the vault
- Archive
    - Immutable
    - Up to 40 TB
    - file, zip, tar, etc

## EBS

- Virtual Hard Drives
- Tied to a single AZ
- Choices for IOPS, throughput, cost

### EBS vs Instance Store

Instances stores can be thought of as attached directly to the EC2 instance. Therefore will offer better performance, but the memory is temporary. Whereas EBS can be detached from the EB2 and you are allow to take snapshots of them. As a result EBS volumes are slower than instance stores since we need to access them over the network. 

- Cost effective and easy backup strategy
- Share between accounts
- Migrate a system to a new AZ or region
- Convert unencrypted volume to an encrypted volume

Later snapshots only contain small changesets.

### Snapshots

Deleting earlier snapshots moves the pointer of data forward

Deleting in the middle is allowed, but you will not be able to restore that point in time again

**Data Life Cycle Management** - used to manage snapshots and their deletion

## EFS (Network File System)

- Implementation of NFS file share
- Elastic storage capacity, and pay for only what you use
- Multi-AZ
- Configure mount points in one or many AZs
- Can be mounted from on prem. Only do it if you have a stable and fast connection. NFS protocol is not secure. 
- Alternatively use Amazon Data Sync. This provides secure transfer. 

- 3 times expensive as EBS
- 20 times as expensive as S3

## Storage Gateway

- VM that you an run on prem
- Providers local storage resource backed by AWS S3 and Glacier
- Used in disaster recovery preparedness to sync to AWs
- Useful for migrations

### Modes

- File Gateway (NFS, SMB) - Allow on-prem or EC2 instances to store objects via NFS or SMB mount point
- Volume Gateway Stored Mode / Gateway-stored Volumes (iSOSI) - Async replication of on-prem data to S3
- Volume Gateway Cached Mode / Gateway Cached Volumes (iSCSI) - Primary Data stored in S3 with frequently accessed data cached locally on-prem
- Tape Gateway / Gateway-Virtual Tape Library (iSCSI) - Virtual media changer and tape library for use with existing backup software

## WorkDocs (Think Google Docs)

- Secure fully managed file collaboration service
- Integrates with AD for SSO
- HIPAA, PCI, DSS, ISO compliance
- SDK available

## EC2 Databases

- Run any database on EC2
- Good if there is a DB not available on AWS managed

## RDS

- Managed database option for MySQL, Maria, PostgreSQL, MS SQL, oracle, Aurora
- Best for relational data store needs
- Automates backups and patching in customer defined maintenance windows

### Anti patterns

Lots of Blobs - S3
Automated scalability - DynamoDB
Name/Value Data structure - DynamoDB
Data not well structured or unpredictable - DynamoDB
Other platforms IBM DB2 or SAP HANA - EC2
Complete control over the database - EC2

### Failover

- Multi-AZ RDS
- Read replicas 

- If the master fails, a stand by data base will be promoted to master. If a region fails a read replica can be promoted to a master node. This read replica promotion can be automated, but caution should be taken when doing so.
- Read replicas can be slightly behind

## DynamoDB

- Managed, multi-AZ NoSQL data store with cross-region replication option
- Defaults to eventual consistency reads, but can request strongly consistent read via SDK parameters.
- Priced on throughput, rather than compute
- Provision read and write capacity in anticipation of need
- Autoscale capacity adjusts per configured min/max levels
- On-Demand Capacity for flexibile capacity at a small premium
- Achieve ACID compliance with DynamoDB Transactions

### Relational vs NoSQL

 - There must be a primary key to identify records, also known as the partition key
 - There can also be a composite primary key known as a partition key and sort key

### Secondary Indexes

- Global Secondary Index - partition key and sort key can be different from those on the table
    - When you want to quickly query attributes outside of the primary key without doing a table scan. "Give me salesforce orders by customer number"
- Local Secondary Index - Same partition key as the table, but different sort key
    - When you have the partition key and want to quickly query on some other attribute. "I have the sales order number, but I'd like to retrieve only those records with a certain material number"

## Redshift

- Fully managed, clused peta-byte data warehose

Data Lake 
- Query Raw data without extensive preprocessing.
- Lessen time from data collection to data value
- Identify correlations between disparate data sets

## Neptune - Graph Database

Fully Managed graph database
Supports open graph APIs

## Elasticache

- Fully managed implementation of two popular in-memory data stores - Redis and memcached
- push-button scalability for memory, writes and reads
- In memory key/value store not persistent in the traditional sense
- billed by node size and hours of use

### Uses

- Web session store - with load balanced web servers, they store web session information in redis so if a server is lost, the session info is not lost and another web server can pick-up
- Database caching - used in front of RDS to cache popular queries to offload work from RDS and return results faster to users
- Leaderboards - Use redis to provider a live leaderboard for millions of users of your moblie app
- Streaming Data Dashboards - provide a landing spot for streaming sensor data on the factory floor, providing real-time dashboard displays

### Types

Memcache

- Simple straight forward
- You need to scale in and out as demand changes
- You need to run multiple CPU cores and threads
- You need to cache objects

Redis

- You need encryption
- HIPAA compliance
- Support for clustering
- You need complex data types
- You need high availability
- pub/sub
- geospacial indexing
- backup and restore

## Other options

- Athena - SQL Engine overlaid on S3 based on presto
    - Query raw data objects as they sit in an S3 bucket
    - Convert to parquet format for performance
    - Similar to redshift but used when you don't have the need to perform joins

- Amazon Quantum Ledger - Great if you just need a journal
    - Based on blockchain concepts
    - Provides an immutable journal as a service
    - Centralized design allowing for higher performance
    - Append only concept where each record contributes to the integrity of the chain
- Amazon Managed Blockchain
    - Fully managed blockchain framework supporting open source frameworks
    - Designed to be distributed
    - Uses Amazon QLDB ordering service to maintain complete history of all transactions
- Amazon TimeStream database
    - Fully managed database service specifically for storing and analyzing time-series data
    - Alternative to DynamoDB or redshift and includes built in analytics line interpolation and smoothing
    Good for
        - Industrial Machinery
        - Sensor Networks
        - Equipment Telemetry
- Amazon DocumentDB
    - mongoDB compatibility
    - acts like mongodb
- Amazon ElasticSearch (ES)
    - Mostly a search engine, but also a document store (JSON)
    - Amazon elastic search service components are sometimes referred to as an elk stack

## Comparison

Database on EC2
- Ultimate control
- DB not supported on RDS

Amazon RDS
- Need traditional relational database for OLTP
- Your data is well formed and structured

Amazon DynamoDB
- Name/Value pair data or unpredictable data structure
- In-memory performance with persistence

Amazon Redshift
- Massive amounts of data
- Primarily OLAP workloads

Amazon Neptune
- Relationships between objects a major portion of data value

Amazon Elasicache
- Fast temporary storage for small amounts of data
- Highly volatile data

## Exam Tips

### Whitepapers

- [AWS Storage Options](https://d1.awsstatic.com/whitepapers/Storage/AWS%20Storage%20Services%20Whitepaper-v9.pdf)
- [SaaS Storage Options](https://d1.awsstatic.com/whitepapers/Multi_Tenant_SaaS_Storage_Strategies.pdf)
- [Performance at Scale with Amazon ElastiCache](https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf)

### re:Invent Videos

- [Amazon S3 & Amazon Glacier Storage Management](https://www.youtube.com/watch?v=SUWqDOnXeDw)
- [Deep Dive on Amazon Relational Database Service](https://www.youtube.com/watch?v=TJxC-B9Q9tQ)
- [ElastiCache Deep Dive: Best Practices and Usage Patterns](https://www.youtube.com/watch?v=_YYBdsuUq2M)
- [Deep Dive: Using Hybrid Storage with AWS Storage Gateway](https://www.youtube.com/watch?v=9wgaV70FeaM)

#### Bonus Videos
- [Which Database to Use When](https://www.youtube.com/watch?v=KWOSGVtHWqA)

## Pro Tips

- Archiving and Backup often a great "pilot" to build AWS business case
- Make use of S3 endpoints withing your VPC
- Learn how to properly secure your S3 Buckets
- Encrypt, Encrypt, Encrypt

- Consider Aurora for production needs
- Consider NoSQL if you don't need relational database features
- Database on EC2 costs less on the surface than RDS, but remember the costs of management (backups, patching, OS-level hardening)
- There can be a performance hit when RDS does back ups and there RDS is single AZ

## Additional Reading

- [EFS Access](https://docs.aws.amazon.com/efs/latest/ug/accessing-fs.html)
- [Mounting EFS For High Availability](https://docs.aws.amazon.com/efs/latest/ug/accessing-fs.html)

Quiz Notes

If you make a HEAD or GET request for the S3 key name before creating the object, S3 provides eventual consistency for read-after-write. As a result, we will get a 404 Not Found error until the upload is fully replicated. However, this replication usually only takes a few seconds and we might get the metadata after all. Further information: https://docs.aws.amazon.com/AmazonS3/latest/dev/Introduction.html

## Challenge

1. Identify an open source data repository for air quality readings
2. Setup a way to query that data via AWS services and tools
3. What City had the highest average ozone (O3) reading on October 9 2018
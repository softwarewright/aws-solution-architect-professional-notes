# Architecting to Scale

## Architectural Patterns


### Loosely Coupled Architecture

- Layers of abstraction
- Permits more flexibility

Shoot for horizontal scaling, meaning adding more instances on demand. This means no downtime, it's automatic with autoscaling groups.

Know that sometimes a single large instance can cost the same as 4 smaller instances.

## Auto-Scaling 

### Amazon EC2 Auto Scaling

Automatically provides horizontal scaling 

Triggered by an event or scaling action to either launch or terminate instances.

Availability, Cost and System metrics can all factor into scaling

Four scaling options

- Maintain - keep a specific or minimum number of instances running 
- Manual - Use maximum, minimum, or specific number of instances
- Schedule - increases based on schedule
- Dynamic - Scale based on real-time metrics

#### Launch Configs

Attach to an ELB
Define a health check grace period.

### Application Auto Scaling - API used to control scaling for things like DynamoDB ECS, EMR

### AWS Auto Scaling - Centralized means of Auto Scaling

## Scaling Types

- Maintain - I need 3 instances always 
- Manual - Manually change the desired capacity via console or CLI, my needs change so rarely that I can just manually add and remove
- Scheduled - Adjust instances based on times. 
- Dynamic - scaling in response to behavior

### EC2 Auto Scaling Policies

- Target Tracking - when CPU is greater than 70% scale up
- Simple Scaling Policy - Wait until health check and cool down period expires before evaluating new need.
- Step Scaling Policy - Responds to scaling needs with more sophistication and logic 

### Cooldown Concept

Gives your scaling a chance to start up and absorb load.

Default cool down period is 300 seconds.

Applies to dynamic scaling.

You could also do scaling based on SQS queues size.

### AWS Auto Scaling

A wholistic scaling view on scaling. There is the option of predictive scaling.

## Kinesis

- Collection of services for processing streams of data
- Data is processed in "shards" - with each shard able to ingest 1000 records per second
- There is a defaultl of 500 shards, but you can request an increase of unlimited shards
- Record consists of partition key, sequence number and data blob (up to 1MB)
- Transient Data Store - Default retention of 24 hours, but can be configured up to 7 days.

Kinesis allows you to perform analytics on data as it comes through.

### How does it work?

The shards that you have can feed into either consumer apps or kinesis firehose.

Each shard is given a partition key (128 bit md5 hash). Each piece of data is given a sequence number.

## DynamoDB

Throughput - Read/Write Capacity Units
Size - Max item size is 400 KB

Storage is limitless 

Partition - Phyiscal space where DynamoDB data is stored. (10GB)
Partition Key - A unique identifier for each record; sometimes called hash key.
Sort Key - In combination with a partition key the optional second part of a composite key that defines storage order. Sometimes called a range key

### Partitions

By Capacity - RCU / 3000 + WCY / 1000
By Size - 10GB

Take the max and round up.

You want a partition key with variability.

### Auto Scaling for DynamoDB

- Using Target Tracking method to try to stay close to target utilization
- There is no scale down if the consumption drops 
- Workaround 1: Send request to the table until it scales down
- Workaround 2: Manually reduce the max capacity
- Also supports Global Secondary Indexes - Like a copy of the table

### On-Demand Scaling for DynamoDB

- Alternative to Auto-Scaling
- Useful if you can handle scaling lag
- Instantly allocates capacity as needed with no concept of provisioned capacity
- Costs more than traditional provisioning and auto-scaling

### DAX

An in-memory cache that sits in front of DynamoDB. When you need nano second response instead of micro second response.

Good Uses of DAX
- require the fastest possible reads such as live auctions or securities trading
- Read-intense scenarios where you want to offload the reads from DynamoDB
- Repeated reads against a large set of DynamoDB data

Bad Uses of DAX
- Write intensive applications that don't have many reads
- Application where you use client caching methods

## CloudFront

- Delivers content faster by caching static and dynamic content at edge locations.
- Dynamic content delivery is achieved using HTTP cookies forwarded from your origin
- Supports adobe flash media server's RTMP protocol but you have to choose RTMP delivery method.
- Web distros also support media streaming and live streaming but use HTTP or HTTPS
- Origins can be S3, EC2, ELB, or other web servers
- Multiple origins can be configured
- Use behaviors to configure serving up origin content based on URL paths

### Invalidation Requests

1. Delete the file from the origin and wait for the TTL to expire
2. Use the AWS Console to request invalidation for all content or a specific path
3. Use CloudFront API to submit an invalidation request
4. Use third-party tools to perform CloudFront invalidation.

### Features 

- Zone Apex Support
- Geo-Restrictions - When you are not allowed to distribute content to certain locations.

## SNS

- Pub/Sub design pattern
- Topics = A channel for publising a notification
- Subscription = configuring an endpoint to recieve a message
- Endpoint protocols include HTTP(s), Email, SMS, SQS, Amazon Device Messaging (push notifications) and Lambda.

### SQS

- Reliable, highly scalable, hosted message queuing service
- Avaiable integration with KMS for encrypted messaging
- Transient storage - default 4 days, up to 14 days
- Optionally support FIFO queue ordering
- Maximum message size of 256 KB but with a Java SQS SDK you can have messages as large as 2 GB (it uses S3).

Queues create loosely coupled architecture.

## Amazon MQ

- Managed implementation of Apache ActiveMQ
- Fully managed and highly available within a region
- ActiveMQ API and support
- Use SQS if you are creating a new app from scratch
- Use MQ if you want low hassle free migration from on prem queues


## Challenges


- Know the difference between the following
    - [EC2 Auto Scaling](https://aws.amazon.com/ec2/autoscaling/)
    - [Application Auto Scaling](https://docs.aws.amazon.com/autoscaling/application/userguide/what-is-application-auto-scaling.html)
    - [AWS Auto Scaling](https://aws.amazon.com/autoscaling/)

- Kinesis
    - Video Streaming
    - Data Streams
    - Firehose
- Implement Fan Out Architecture with SNS
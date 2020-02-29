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

## Serverless + Lambda

### Lambda

- Allows on demand code without infrastructure
- Supports Node.js Python, Java, Go, and C#
- Code is stateless and executed on event basis (SNS, SQS, S3, DynamoDB Streams)
- No limits to scaling a function since AWS dynamically allocates capacity

### Lambda at Scale

Commonly used for fan-out architecture. 

### AWS Serverless Application Model

- Open source framework for building serverless apps on AWS
- Uses YAML as the configuration language
- Includes AWS CLI-like functionality
- Enables local testing and debugging of apps using lambda-like emulator via docker
- Extension of CloudFormation so you can use everything CloudFormation can provide by way of resources and functions.

### Amazon EventBridge

- Designed to link variety of AWS and 3rd party apps to rules logic  for launching other event-based actions 

An example would be if someone creates a JIRA ticket you can trigger a lambda function to execute logic.

## Simple Workflow Service (AWS SWF)

A managed status tracker.

- Create distributed async systems as workflows
- Supports both sequential and parallel processing
- Tracks the state of your workflow which you interact and update via API
- Best suited for human-enabled workflows like an order fulfillment or procedural requests.
- AWS recommends new applications - look at step functions over SWF.

### The Two pieces to SWF

Activity Worker - program that interacts wih the AWS SWF service to get tasks, process tasks, and return results.

Decider - A program that controls coordination of tasks such as their ordering, concurrency and scheduling.

The activity workers use long polling to see if there is any work that they need to do.

## AWS Step Functions

- Managed workflow and orchestration platform
- Scalable and highly available
- Define your app as a state machine
- Create tasks, sequential steps, parallel steps, branching paths or timers.
- This is written in Amazon State Language declarative JSON
- Apps can interactve and update the stream via Step function API
- Visual interface describes flow and realtime status
- Detailed logs of each step execution

Think of them in terms of a finite state machine.

## AWS Batch

Management tool for creating, managing and executing batch oriented tasks using EC2 instances

1. Cerate Compute Environment: Managed/Unmanaged, Spot/On-Demand, and CPU
2. Create a Job Queue with priority and assigned to a compute environment
3. Creation Job Definition: Script or JSON, environment variables, mount points, IAM role, container image, etc.
4. Schedule the Job


## Comparisons

Step functions - out of the box coordination of AWS service components

Simple Workflow Service - Need to support external processes or specialized execution logic. Good for manual intervention. Lean towards Step functions.

Simple Queue Service - Messaging Queue; Store and word patterns. Image resize process

AWS Batch Scheduled or reoccurring tasks that do not require heavy logic. Rotate logic daily.


## ElasticMap Reduce

Hadoop map reduce + Hadoop HDFS

- Managed Hadoop framework for processing huge amounts of data
- Also supports Apache Spark, HBase, Presto and Flink
- Most commonly used for log analysis, financial analysis or extract, translate and loading (ETL) activities
- A Step is a programmatic task for performing some process on the data (i.e. count words)
- A Cluster is a collection of EC2 instances provisioned by EMR to run your Steps.

An example of when to use this is if you wanted to collect all of your logs, and scan them for anomalies.

## Tips

Auto Scaling Groups:
- Know the different scaling options and policies
- Understand the difference and limitations between horizontal and vertical scaling
- Know what a cool down period is and how it might impact your responsiveness to demand. How is it different from a health check grace period.

Kinesis:

- Exam is likely to be just Data Stream use cases such as Data Streams and Firehose
- Understand shard concept and how partition keys and sequences enabled shards to manage data flow.

DynamoDB auto scaling:

- Know the new and old terminology and concept of a partition, partition key and sort key in context of DynamoDB
- Understand how DynamoDB calculates total partitions and allocates RCU and WCU across available partitions.
- Conceptually know how data is stored across partitions.

CloudFront
- Both static and dynamic content is supported
- Understand possible origins and how multiple origins can be used together with behaviors
- Know invalidation methods, zone apex and geo-restriction options

SNS:
- understand a loosely coupled architecture and benefits it brings
- Know different types of subscription endpoints supported.

SQS:

- Know the difference between Standard and FIFO queues
- Know the difference between Pub/Sub (SNS) and Message queueing (SQS) architecture

Lambda:
- Know what "Serverless" is in concept and how lambda can facilitate such an architecture
- Know the languages supported by lambda

SWF:
- Understand the difference and functions of a worker and a decider
- Best suited for human-enabled workflows like order fulfillment or procedural requests

Elastic MapReduce
- Understand the parts of a Hadoop landscape at a high level
- Know what a cluster is and what Steps are
- Understand the roles of a master node, core node, and task nodes

Step Functions
- Managed workflow and orchestration platform considered the preferred for modern development over AWS simple workflow service
- Supports tasks sequential steps, parallel steps, branching, timers

AWS Batch
- Ideal for use cases where a routine activity must be performed at a specific interval or time of day
- Behind the scenes, EC2 instances are provisioned as workers to perform the batch activities then terminate when done

Serverless Application Model
- Open source framework created to make serverless application development and development more efficient
- SAM CLI translates specific SAM YAML into CloudFormation scripts which is then used to deploy on AWS 
- Know that AWS SAM and the **Serverless Framework** are different things

Auto Scaling
- Know the different purposes of 
    - EC2 Auto Scaling
    - Application Auto Scaling
    - AWS Auto Scaling
- Know the different scaling options and policies
- Understand the difference and limitations between horizontal vs vertical scaling
- Know what a cooldown period is and how it affects responsiveness to demand.

## Whitepapers

- [Web Application Hosting in the
AWS Cloud](https://d1.awsstatic.com/whitepapers/aws-web-hosting-best-practices.pdf)
- [Introduction to Scalable
Gaming Patterns on AWS](https://d0.awsstatic.com/whitepapers/aws-scalable-gaming-patterns.pdf)
- [Performance at Scale with
Amazon ElastiCache
](https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf)
- [Automating Elasticity](https://d1.awsstatic.com/whitepapers/cost-optimization-automating-elasticity.pdf)
- [AWS Well-Architected Framework](https://d0.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf)
- [Implementing Microservices
on AWS](https://d1.awsstatic.com/whitepapers/microservices-on-aws.pdf)

## Reinvent

- [Scaling Up to Your First 10 Million Users](https://www.youtube.com/watch?v=w95murBkYmU)
- [Learn to Build a Cloud-Scale WordPress Site That Can Keep Up](https://www.youtube.com/watch?v=dPdac4LL884)
- [Elastic Load Balancing Deep Dive and Best Practices](https://www.youtube.com/watch?v=9TwkMMogojY)

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
- [Amazon Event Bridge](https://aws.amazon.com/eventbridge/)
- [AWS SWF](https://aws.amazon.com/swf/)
- [AWS Step Functions](https://aws.amazon.com/step-functions/)
- [AWS Batch](https://aws.amazon.com/batch/)
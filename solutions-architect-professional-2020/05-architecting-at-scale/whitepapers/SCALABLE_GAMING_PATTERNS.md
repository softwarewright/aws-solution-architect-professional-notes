# Introduction to Scalable Gaming Patterns on AWS

https://d0.awsstatic.com/whitepapers/aws-scalable-gaming-patterns.pdf

## Game Design Decisions

Many games tend to have the following features:

- Leaderboards and rankings
- Free to play
- Analytics around the game
- Content updates
- Async gameplay - real-time only multiplayer and competing against friends
- Push notifications
- Unpredictable clients since there is a variety of platforms

## Game Client Considerations

- All network calls are async and non-blocking
- Use JSON to transport data, it's compact and cross platform. The exception being UDP packets in  multiplayer.
- Use HTTP/1.1 with keepalives and reuse http connections between requests, as each connection can take 50 ms for the TCP handshake
- Use SSL for all requests
- Never store security critical data on the client device, if you need the client to access AWS use Amazon Cognito
- Never trust what a game client sends as it's an untrusted source

### Handling the Backend

Services that work well include:

- Elasticbeanstalk - for deploying the server, auto scaling, and allowing [Blue/Green Deployments](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.CNAMESwap.html). Don't manage RDS with your Elasticbeanstalk instance in production.
- RDS - for the backend database
- S3 - hosting static content and binary game data. Be sure the bucket is in the same region as your servers.

### HA, Scalability, and Security

Tips:

- Two AZs for a balance of HA and cost in production
- Single AZ for dev and test environments. Unless you require HA in non-prod
- Use a Load Balancer

### Binary Game Data with Amazon S3

Since S3 Buckets are private by default for users to download content you have two options. Make the bucket public, or the better way, create pre-signed urls using the AWS SDK. Creating the presigned url is a completely offline operation.

Use CloudFront as the game grows to provider better performance.

### Expanding Beyond AWS Elastic Beanstalk

You have the ability to use other AWS services in tandem with Elastic Beanstalk.

- ElastiCache - for caching
- DynamoDB, RDS, EC2 hosted DB - for data management

### ELB

Tips:

- Configure to balance between at least 2 AZs
- Enable SSL
- Use the DNS CNAME since IP address will change when the ELB scales
- ELB scale up roughly 50% every 5mins which usually works, but if you need more pre-warm the ELB through an AWS support request

### ALB

- Support for Amazon ECS
- HTTP/2
- IPv6 support
- WebSockets support

### Custom LB

If interested in HAProxy as a managed service consider AWS OpsWorks

### Auto Scaling

Tips:

- CPUUtilization is a good metric for auto scaling, if paired with NetworkIn or NetworkOut you can get more granularity
- Benchmark servers to determine good scaling values. Apache Bench or HTTPerf to measure response times. Take note of when the application degrades

Consider OpsWorks for bootstraping EC2 instances when you are not using Elastic Beanstalk.

## Game Servers

With game server's the approach is opposite of the RESTful approach. Clients will establish a stateful connection to the game server, this can be done through UDP, TCP, or WebSockets. If the network connection is interrupted then there needs to be client side reconnection logic.

Historically games used stateful connections and long-running server processes for all game functionality. GameLift is a managed service that helps with deploying, operating, and scaling dedicated game servers for session based multiplayer games.

The recommendation is to use http as much as possible using stateful sockets for things that actually need it like multiplayer. The hybrid approach of using the REST API to get information like friend lists, and altering inventory and connecting to the server for stateful information means more performant socket servers.

Network and database latency are generally the limiting factor in game servers.

### Push Messages with SNS

Messages meant to be sent between server can be sent to an SNS Topic which can then distribute the message to the correct server. Other options can be queues like RabbitMQ or Apache ActiveMQ.

With SNS you also have the ability to send mobile push notifications to mobile clients.

## Relational vs. NoSQL Databases

NoSQL databases can be beneficial to games due to their eventually consistent nature, as a result they can be significantly faster than their SQL counter parts.

Databases most used with games are:

- MySQL
- Aurora
- Redis
- MongoDB
- DynamoDB

### MySQL

ACID compliant and has the following benefits:

- Transactions - Grouping multiple changes into a single atomic transaction that must be committed or rolled back.
- Advanced Querying - Support for complex queries that evolve over time.
- Single Source of Truth - Guarantees data consistency over eventual consistency
- Extensive Tools - due to how long MySQL has been around it has an enormous toolset around it

MySQL really shines where data consistence is paramount. Even when you do use NoSQL offerings you want to use the right tool for the job and understand that when there are transactional needs MySQL/Aurora are an excellent choice.

#### Tips

- Micro instance in dev and test environments and medium to large for production environments.
- Multi-AZ deployment - no for dev/test environments and yes in production environments. Ensure that the DB instance is separate from other RDS dev/test instances
- Auto minor upgrades
- Allocate Storage: 5GB for dev/test and 100GB for production environments to enable PIOPS
- Use PIOPS for production environments.
- Enabled RDS backup snapshots and upgrades
- Enable MySQL slow query log
    - `slow_query_log=1`, these will be written mysql.slow_log
    - `long_query_time` should be updated to lower values. Consider 5, 3, or even 1 second.
    - Periodically rotate the slow query logs

As load increases in you need to increase database size, Multi-AZ comes in handy because you can rely on the failover to switch over to the larger instance.

### Amazon Aurora

MySQL compatible database that has speed and availability of high-end commercial databases.

- High performance - Designed to provider up to 5x throughput of standard MySQL running on the same hardware. On the largest instances it's possible to provide up to 500,000 reads and 100,000 writes per second. With 10 millisecond latency between read replicas

- Data durability - 10GB chunks of data replicated six ways across 3 AZs. Meaning you can lose two copies of data without affecting database write availability. Three copies of without affecting read availability. Backups happen automatically to S3 with retension up to 35 days.

- Scalability - Storage scaling up to 64 TB, storage is automatically provisioned. You can deploy up to 15 read replicas in any combination of AZs, including cross region. Allowing seamless failover in case of an instance failure.

#### Tips

- t2.small for dev/test r3.large for production
- deploy read replicas in at least one additional AZ for failover and read operation offloading
- Schedule backup snapshots and upgrades during low player count times
- If the database grows to large consider performance evaluation, tuning parameters and sharding
- Consider NoSQL options to offload workloads

## Redis

Atomic data structure server. The data types of Redis such as lists, counts, and stats make it a great choice for gaming.

Redis requires a large amount of memory because the dataset is all in-memory. The r* instance family is great for redis, or feel free to use ElastiCache for redis to handle clustering, replication, backups, and other redis maintenance.

Redis is generally used in conjunction with MySQL as the main data store.

## MongoDB

NoSQL solution that has JSON like data structures for storage. Allowing SQL like queries on the data.

MongoDB is generally used as the primary data store for games and is fronted with Redis for better system performance. Transient game data in redis and then progress is saved to MongoDB at logical points such as the end of the game/level. 


## DynamoDB

Full managed NoSQL solution.

Features:

- synchronous replication
- IO provisioning 
- automatic scaling
- managed caching

DynamoDB generally is used for the following

- KV store for user data, items, friends, and history
- Range key store for leader boards, scores, and date ordered data
- Atomic counters for game status, user counts, and matchmaking

### Table Structure and Queries

- Partition key - a unique key for records. Think username, game id, or uuid.
- Partition key + sort key - A composite primary key is a key composed of two attributes. These two values will have an unordered hash backing them to make querying elements faster.

To achieve the best querying performance maintain each DynamoDB table at a manageable size. For example have a table for each game mode, rather than a large single table.

### Provisioned Throughput

A read capacity unit represents 1 strongly consistent read / 2 eventually consistent reads for up to 4 KB.

One write capacity unity is one write per second  up to 1 KB.

**You cannot decrease the read or write capacity more than four decreases in one day.**

To get the best performance from DynamoDB, ensure reads and writes are spread as evenly as possible across keys. A hex string like a hash key or checksum is an easy way to inject randomness.

DynamoDB scales best for changes over time i.e. growth from 1000 to 10000, but if you have sudden growth then Redis or DAX will help.

### Amazon DynamoDB Accelerator (DAX)

Fully managed in-memory cache for DynamoDB to speed up the responsiveness of DynamoDB tables from milliseconds to microseconds.

## Caching

Memcached is a high-speed memory based key-value store paired with ElastiCache managing a cache is as simple as RDS. You can also used redis with ElastiCache.

Common approach to caching is lazy population where you check the cache and if the value is not there, then you get a cache miss. After the data has been retrieved then we write it to the cache to be read again later. The downside is that the response time is slower because the records needs to be queried from the database and populated into the cache.

For data that you know will be accessed frequently we can populate the record when the records are saved to prevent unnecessary cache misses. You have to be careful about how much you write to the cache because this can quickly fill up your cache if you are not careful.

Finally you have a timed refresh where you execute a background job that queries the database and refreshes the cache every few minutes.

### Scaling ElastiCache

Monitor the following metrics 

- CPUUtilization
- Evictions - should be 0 100% of the time, if it's nonzero scale up the nodes
- CacheHits/CacheMisses - if too low revisit the cache implementation
- CurrConnections

Configure the cache node cluster across multiple AZs to increase HA.

With ElastiCache for Redis you can do Shareded Clusters where you can create clusters with up to 15 shards expanding the data store up to 3.5 TiB and Each shard can have up to 5 read replicas. Meaning you can handle 20 mil reads and 4.5 mil writes per second.

## Binary Game Content with S3

S3 is great for the following use cases in  gaming use cases:

- Content downloads - Patches, betas, assets
- User generated files - Photos, avatars 
- Analytics - Storing metrics, device logs, and usage patterns
- Cloud saves - Game save data and syncing between devices

The advantages of storing on S3 vs in a database is the following:

- Storing binary data in a DB is memory/disk intensive consuming valuable query resources
- Clients can directly download the content from S3 using HTTP/S get
- Designed for 99.999999999% durability and 99.99% availability of objects over a year.
- S3 supports ETag (allows caches to be more efficient), authentication and signed urls
- S3 plugs into CloudFront for distributing content to large numbers of clients

With CloudFront you also have the ability to Geo Target or restrict access to your content. Amazon CloudFront detects the country where your customer are located and forwards the country code to the origin servers. Thus allowing your server to determine the type of content that will be returned.

### ETag

An HTTP header that allows you to send a request for S3 content, and include the MD5 checksum of the version that you currently have. If you already have the file then you will get a 304 not modified, if not then you get a 200 with the file data

### Uploading Content to S3

The recommendation to to upload directly to S3 rather than uploading through REST API endpoint.

User generated content is a great use case for uploading data to S3. USG generally has two parts the file and metadata about the file that can be store in a database. Which is treated as a master index of available content that others can download.

The flow for uploading content generally looks like the following:

1. PUT multi-part file data to S3
2. HTTP 200 OK from S3
3. POST meta-data to REST API tier
4. Insert data + S3 key to database

### Collecting Analytics

Once you know what you are looking to have answered about the user use the following steps:

1. Collect metrics in a local csv file on the device with a unique filename. e.g.  241-game_name-user_idYYYYMMDDHHMMSS.csv
2. Periodically persist the data by uploading to S3
3. For each file put you will need to indicate that there is a file to process. S3 Event Notifications works well, you can place the event information on an SQS queue then have a worker process the queue.
4. Amazon EMR can be used to process the data in the background
5. The data can also be feed into Redshift for additional data warehousing an analytics

### Athena

Athena helps with analytics pipeline by providing a means of querying data stored in S3 using standard SQL

### S3 Performance Considerations

S3 scales out to tens of thousands of PUTs and GETs per second.

You essentially want s3 to partition your files separately to achieve better performance. You can prefix the S3 key with a random 3 characters.

You can file an AWS support ticket to have your buckets validated for well architecture.

## Loosely Coupled architecture

Consider the features of your application to understand what can and cannot afford to be eventually consistent. For example cosmetic changes can be loosely coupled and eventually consistent, whereas match making needs to be as up to date as possible.

Another example is a user updating their stats needs to be immediately updated, where as the leaderboard can afford to be a few seconds off.

### SQS

Fully managed queue solution.

- Create the queue in the same region as the API for fast writes.
- SQS scales horizontally, it can process 50 requests a second. The more client processes that you add the more messages that you can process
- Consider Spot Instances for job workers to save money
- Message visibility will allow you to specify the amount of time before SQS will attempt to redeliver the message (defaults to 30 secs). Increase the time if you have longer running jobs
- SQS also has delete letter queues for messages that could not be processed

Caveats:

- Messages are not guaranteed to arrived in order. If you need order use FIFO queues.
- Messages can be delayed by a few minutes, but tend to arrive quickly.
- Messages can be duplicated, the client should ensure de-duplication

Essentially ensure  that your jobs are idempotent and resilient to delays. For example resizing and replacing an avatar yields the same results if applied twice.

#### FIFO Queues

These queues provide the ability to process messages in order and exactly once

## Challenges

- S3
    - Generate a Presigned Url
    - Request a file using ETags and get back a 304
    - [Perform a browser upload to S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingHTTPPOST.html) - be sure to include an MD5 checksum of the file so that S3 can ensure that the file was not corrupted during the upload.
    - Use S3 Events to post to an SQS queue and enable a worker to process the queue.
- [AWS OpsWorks](https://aws.amazon.com/opsworks/)
- SNS
    - Send mobile push notifications
    - Send messages through SNS to multiple endpoints
- RDS Aurora
    - Deploy across regions

## Resources

- [Common DBA Tasks for MySQL DB instances](http://docs.amazonwebservices.com/AmazonRDS/latest/UserGuide/Appendix.MySQL.CommonDBATasks.html)
- [SQS Throughput](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/throughput.html)
- [DynamoDB Best practices](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/BestPractices.html)
- [Optimizing Provisioned Throughput in DynamoDB](http://aws.typepad.com/aws/2012/09/optimizing-provisioned-throughput-in-amazon-dynamodb.html)
- [Apache Bench](http://httpd.apache.org/docs/2.2/programs/ab.html)
- [loadtest](https://www.npmjs.com/package/loadtest)
- [Artillery](https://artillery.io/)
# Performance at Scale with Amazon ElastiCache

https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf

Effective scaling is the single biggest factor in creating apps that perform well at scale.

Benefits of a cache

- Considerably cheaper to add a cache layer than to scale up a database
- It's easier to scale out horizontally than it would be for a database
- Provides a request buffer in the event of a sudden spike in usage (Which would make your database unhappy if there was not a cache)


## ElastiCache

The caching layer that you choose does not communicate directly with the database, rather your service (EC2, Lambda, Container) will communicate with the caching layer to retrieve and set keys. Meaning your application has full control over what data is cached and at what point. This means if there are expensive calculations that must be done after a query or etc it's up to you to store the result afterward.

When launching elasticache to get the best performance it is best to use the same AZs as your application servers, just be sure to specify the preferred zones when creating the clusters. Also be sure to select **spread nodes across zones** 

A popular combination for mobile and game companies is DyanmoDB with ElastiCache. DyanamoDB allows for higher write throughput at a lower cost than traditional relational databases. This paired with the similar key value access it simplifies the development process.

### Alternatives

- RDS Read Replicas - While this is useful, this is limited to providing data in a duplicate format of the primary database. You cannot cache calculation, aggregates, or arbitray custom keys in a replica. Read replicas are also not as fast as in-memory caches.

### Choosing the right cache node size

Choose a node from the M5 or R5, as the families increase performance generally see great improvements.

If you are not sure how much you need start with cache.m5.large node, then use metics in cloudwatch to monitor the following:

- Memory Usage
- CPU utilization
- Cache hit rate

For production large workloads R5 nodes provider the best performance and memory cost value.

To get an approximate estimate of the amount of cache memory you'll need multiply the size of items you want to cache by the numner of items you want to keep cached at once. Just to be safe it's good to overestimate a little and guess 1-2 KB per cached object.

Due to ElastiCahe being pay as you go, make a best guess at node instance size, and then adjust after getting more real world data.

### Security Groups and VPC

ElastiCache supports security groups, use  groups to define rules that limit access to your instances based on IP address and port. You also have the ability to add SG's from EC2s.

Launch the cluster in a private subnet (no public connectivity) for the best security. Only allow access to ElastiCache from the application tier, don't allow connection from the database tier as your database will not directly interact with ElastiCache.

### Caching Design Patterns

- Is it safe to use a cached value? For example you don't want a cached value when checking out, but it might be ok on other pages for a price to be a few minutes out of date.
- Is caching effective for that data? If you have to constantly keep the cache up for an every changing dataset may not be worth it.
- Is the data structured well for caching?

**Ensure that consistent hashing is enabled with the library that you are using.**

#### Be Lazy

Populate the cache only when the object is actually requested. 

1. You receive a query
2. Check if the object is in the cache
3. If so there is a cache hit, call flow ends
4. If not cache miss cache is populated and the object is returned 

Advantages of the lazy approach

- Cache size is limited to objects that the application would request. As a result the cache size is more manageable.
- Cache expiration is easily handled by deleting the cached object
- Lazy caching is widely understood and many web and app frameworks include support out of the box

Apply this pattern to locations in the code where data is read often,  but written infrequently.

#### Write Through

This means updating the cache as the database is updated.

Advantages
- Avoids cache misses
- Shifts any application delay to the user updating data which maps better to user expectations
- Simplifies cache expiration as the cache is always up to date

Disadvantages
- Cache filled with unnecessary objets that aren't actually being accessed
- Can result in a lot of cache churn if records are updated repeatedly
- When cache nodes fail, objects will no longer exist in the cache. Meaning you will need the means to replace the objects e.g. lazy population


Lazy and Write Through can be used together and in fact compliment each other. They can help each others short comings.

#### Expiration Date

Tips
- Always apply TTL to all cache keys, except write through caching
- For rapidly changing data use short TTLs of a few seconds
- Russian doll caching
- When in doubt delete a cache key

#### The Thundering Herd

Many application processes simultaneously request a cache key, get a miss, and then each hits the same database query in parallel. TTLs can make this worse.

In both cases prewarming the cache can be helpful

1. Write a script that performs the same requests that your application will
2. These cache misses will result in the cache being populated
3. Run the script on new nodes before attaching them to prewarm them
4. If you will be adding new nodes on a regular basis automate the prewarming process through reconfiguration events SNS

One last issue to be aware of is your cache expiring at the same time. To get around this add randomness to your TTL.

```
ttl = 3600 + (rand() * 120) /* +/- 2 minutes */
```

This is something that only large scale websites generally need to be concerned with though.

### Memcached vs Redis

- Memcached - historically the gold standard of web caching
- Redis - supports advanced data structures. It can be used for long lived data and supports replication which can be used to achieve Multi-AZ redundancy (Like RDS)

When deciding consider the following

- Is object caching your primary goal? Memcached
- Interested in as simple caching as possible? Memcached
- Planning on running large cache nodes with multithreaded performance? Memcached
- Ability to scale horizonally? Memcached
- Need to atomically increment or decrement counters? Redis or memcached
- Need advanced data types? Redis
- Need sorting and ranking datasets in memeory e.g. leaderboards? Redis
- Pub/sub? Redis
- Persistence? Redis
- Multi-AZ failover? Redis
- Geospatial Support? Redis
- Encrpytion and compliance to standards e.g. HIPAA? Redis


Differences

- Redis data structures cannot be horizontally shared. Meaning clusters are always a single node, rather than multiple with memcached
- Redis supports replication like RDS
- Redis supports fail over from the primary node to a replica in the event of a failure
- Redis supports persistence

### ElastiCache for Redis

While Redis does support persistence, DyanmoDB and RDS are better fits for most use cases of long term data storage.

Be careful about the number of replicas that you create as they will affect the performance of the primary node.

Redis cannot be horizontally scaled easily, it can only be scaled up to larger node sizes

#### Distributing Reads and Writes

When using read nodes to retrieve data be aware that the data can be out of data with the primary since Redis updates the read replicas asynchronously.

How to decide if this is ok for your application

- Is the value for display purposes only? Then it's ok
- Is the value a cached value like a page fragment? Then it's ok
- Is the value used on screen where the user might have edited it? This could look like a bug
- Is the value being used for application logic? This could be risky
- Are multiple processes using the value simultaneously? If so the value needs to be up to date

You application will need two separate connection handles in your application. One for the primary node the other for the read replica(s).

#### Multi-AZ with Auto Failover

When the failover occurs this could take up to a few minutes to perform the switch. Ensure that your application can handle this since you will be unable to write to the cluster in that time.

In generall you should set up Redis to have Multi-AZ failover, and realize that in the event of a failover some data can be lost.


### Monitoring and Tuning

Watch CPU usage
- one or two nodes use CPUUtilization
- four or more EngineCPUUtilization

Other metrics to watch
- Evictions
- CacheMisses
- BytesUsedForCacheItems
- SwapUsage
- Currconnections 

#### Hot Spots

These are less common unless you are a particularly large website. 

Essentially they are keys that are accessed significantly more than others, sometimes to a point that they could overload a node.

This can be fixed through:

- Smaller caches in front of the main node (latency)
- Remapping the keys
- Creating cloudwatch notficiation on suspected hot spots.

#### Backup and Restore

ElastiCache can automatically take snapshots of Redis clusters and save them to S3. 

In production it is best to use backup and restore as this can be used to prewarm the node, or on the chance that a production bug corrupts the data.

You also have the ability to use a snapshot to scale up to a larger instance type.

- Suspend writes, you can continue to allow reads
- Take a snapshot
- Create a new redis cluster and specify a cluster
- Once the cluster is online reconfigure the application to begin writing to the new cluster

If your writes cannot be suspended write them to SQS, and then run a script to pull them from SQS and write them to redis.

## Resources

- [Getting started with Redis] (https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/GettingStarted.html)
- [Getting started with memcached](Performance at Scale with
Amazon ElastiCache)
- [Creating a cluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Clusters.Create.html)
- [Choosing your node size](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/nodes-select-size.html)
- [Understanding ElastiCache and VPCs](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/VPCs.EC.html)
- VPCs
    - http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html
    - https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html
- Consistent Hashing
    - http://berb.github.io/diploma-thesis/
    - http://www.tom-e-white.com/2007/11/consistent-hashing.html

- [node-memcached](https://www.npmjs.com/package/memcached)
- [Auto Discovery of Memcached Nodes](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/AutoDiscovery.html)
- [Express Best Practices](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Russian doll caching](Performance at Scale with
Amazon ElastiCache)
- [Redis Backup and Restore](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/backups.html)
- [Redis Memory Optimization](https://redis.io/topics/memory-optimization)
- [How Auto Discovery Works](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/AutoDiscovery.HowAutoDiscoveryWorks.html)
- Monitoring
    - https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/CacheMetrics.html
    - https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/CacheMetrics.html
    - https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/CacheMetrics.WhichShouldIMonitor.html
    - https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/CacheMetrics.WhichShouldIMonitor.html

## Challenges

- DAX vs ElastiCache
- Implement the following in redis
    - Game Leader Board
    - Recommendation engine
    - Chat and messaging
    - Queues
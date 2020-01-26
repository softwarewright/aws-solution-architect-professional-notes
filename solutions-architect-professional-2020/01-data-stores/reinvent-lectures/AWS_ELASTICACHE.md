# Elasticache

https://www.youtube.com/watch?v=_YYBdsuUq2M

Respond in nano seconds, not milliseconds.

## Redis

### Cluster Mode Enabled

Enabled
- Failover 10-30 (Non DNS)
- More expensive, smaller nodes but more of them

Disabled
- Failover 1.5mins
- Less expensive, but larger nodes


#### Zero-downtime online re-sharding 

Creates more nodes without taking down your redis cluster. There will be a performance hit.

This means that you can, based on memory usage, increase the number of nodes that you are using through cloudwatch notifications and lambda functions.

It does take time so be conservative with your estimations.

### Security

VPC + AZs -> Define cache subnet group (Private subnet groups) -> Security Group -> Then create the group -> Enable traffic from your services sg in the public subnet to your redis sg



- You can encrypt snapshots 
- You can have encryption in transit, between the cluster and your service
- Encryption at rest
- Redis auth




## Common Usage

- Session management
- IOT
- APIs
- Database Caching
- Pub/sub
    - Chat
- Leaderboards
    - Easy with redis sorted sets
- Social media (sentiment analysis)
- Streaming data analytics
- Rate limiting
    - Use redis counters

### Caching

Why would I want to cache a NoSQL database? Because you have have extremely fast turn around on reads on the database. Lowering the overall latency.

### Geospacial Location

Stream information from DynamoDB into a ElastiCache to keep the data fresh


## Best Practices

### Cluster sizing 

Storage clusters should have adequte memory
- Recommended: memory 20% reserved memory + some room for growth 10%
- Optimize using evicition policies, think about how often the data is written
- Scale up or out before reaching max-memory using cloudwatch alarms
- Use memory optimized nodes for cost effectiveness

Performance
- Benchmark operations using redis benchmark tool
    - For more READIOPS - add replicas
    - For more WRITEIOPS - add shards (scale out)
    - For more network IO - using network optimized instances and scale out
- Use pipelining for bulk reads/writes
- Consider Big(O) time complexity for data structure commands
- Cluster isolation - choose a strategy that works for your workload

**Use the redis benchmark tool**


### Metics

- CPUUtilization
    - Redis - divide by the cores (90 / 4 = 22.5)
- Swap Usage - low
- CacheMisses/CacheHits ratio low/stable
- Evictions - near zero, if you have these it means that you are overloading the cluster
    - Exception: Russian-doll caching
- CurrConnections stable
- Ensure alarms are set

Parameters

- Maxclients: 65000 (unchangable)
    - Use connection pooling
    - timeout closes a connection after it has been idle for a given interval
    - tcp keep alive
- Databases 


### Caching tips

- Understand the frequency of change of underlying data
    - What is the cost of serving stale data to the users
- Set appropriate TTL to match
- Choose appropriate eviction policies that are aligned with application requirements 
- Isolate your cluster by purpose
- Maintain cache freshness with write-throughs
- Performance test and size 
- Monitor Cache HIT/MISS ratio and alarm on poor performance
- Use failover API to test application resiliency

## Challenge

- Experiment with Redis locally
- How do you implement a cache in a backend service. Look for how to integrate redis in a coherent manner
- Create a cache in AWS
- How fast is Redis vs (RDS, DynamoDB, etc)
    - https://redis.io/topics/benchmarks
- DynamoDB Streams?
- Amazon DynamoDB Accelerator (DAX)
- Investigate redis eviction policies
- What is russian doll caching? 
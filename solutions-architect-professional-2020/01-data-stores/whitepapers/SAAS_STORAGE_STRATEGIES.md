# SaaS Storage Strategies

https://d1.awsstatic.com/whitepapers/Multi_Tenant_SaaS_Storage_Strategies.pdf

## Partitioning Models

- Silo - Separate database for each tenant
- Bridge - Single database with separate schemas for each tenant
- Pool - All tenants share all of the systems storage constructs. This fits well with CD and agility goals essentials to SaaS providers 

## Silo Model

Pros

- Appealing for regulatory and security constraints
- Cross-tenant impacts can be limited
- Contained Availability, minimizing tenant exposer to outage

Cons

- Provisioning and management is more complex, thus creating a layer of complexity and possible point of failure
- Ability to view and react to tenant activity is complicated  by needing system wide health of all tenants
- Limits cost optimization

## Pool Model

The unified approach to handling tenants allowing you to streamline tenant storage provisioning, migration, management, and monitoring.

Pros

- Agility - updating and managing tenants is all centralize
- Storage monitoring and management is simpler
- Clearer opportunities for optimization
- Pool improves deployment automation and operational agility 

Cons

- Higher agility means higher bar for managing scale and availability. This means a heavy investment into automation and testing of environments
- Increases the challenge of data distribution
- Some customers will demand a silo model for their regulatory and data requirements

## Hybrid: The Business Compromise

The tenants will have an influence over how you approach your storage strategy. This strategy might involve more than one of the models.

One possible model is to build a pooled solution as a foundation and then carve out separate databases for tenants that demand a siloed storage solution.

Though complicated it presents the means of tiered offerings to customers.

## Data Migration

Understand how the storage solution that you have chosen will handle change as your business grows.

Think about what migration means for the silo model vs the pool. What the advantages vs disadvantages are.

As a rule of thumb favor backwards compatible changes.

## Security

- Encryption at rest
- IAM works well with silo and bridge model, but with Pools you will need service security
- Isolation is fundamental for some orgs, consider what this will look like in your storage solution

## Management and Monitoring

You need metrics and dashboard to give you an aggregated view of your tenants. With silo this is a bit simpler as they each have their own specific dashboards. Whereas with pool you will need filtering mechanisms.

Alarms should allow you to surface and response to the changes in the health of your service.

## Multitenancy on DynamoDB

With DyanmoDB there are  only global tables, therefore it complicates the silo model a abit. 

One solution is to create unique named tenant tables isolated by IAM roles.

Another is to use linked accounts, but there is a limit to the number of linked accounts that you can achieve.

With the pool model sharding mechanisms can help with performance issues caused due to larger tenants. This does come with complexity overhead, but does allow you to add more shards to larger customers giving you the ability to scale effectively.

Questions to think of
- how to detect when tenants require new shards?
- how do you automate the above process?

## Multitenancy on RDS

### Silo Model

Just create separate instance  per tenant, there is no need to create the separate accounts.

### Bridge

Create an shared instance that has a table per tenant. This gives the ability to create schema modifications per tenant.

Another option is within a single instance create a database per tenant. This prevents you from having to create oddly named tables.

Something to note with SQL Server there is a limit to the number of databases that you can have within a single instance. The the time of this writing (100) and you can have 10 instances with the license included model.

### Pool

This is what you would expect, all tenants share common tables. We rely on indexing by the tenants identifier.

With this model as you gain more tenants you will begin to reach limits of each of the databases (this is in the TBs). You will need to introduce some form of sharding across multiple instances to ensure that none hit their limits. 


## Multitenancy on Amazon Redshift

Limits
- 60 databases per cluster
- 9900+ schemas per database
- 500 concurrent connections per database
- 50 concurrent queries
- Access to a cluster enables access to all databases in the cluster

### Silo

Create a cluster per tenant and take advantage of IAM roles etc to provide security of access.

### Bridge

This previously did not have a natural mapping due to the pervious 256 schema limit. Now there is a limit of 9900 schema limit which can make this option a bit more viable.

The issue then is with security between tables. You will need to move this to the SAAS application rather than apart of AWS security schemes.

### Pool

Pool models closely to what you would expect from RDS. The difference being connection limits, you will have to ensure that though these connection limits work well for your tenants 

These connection limits can be addressed through things like client based caching.

You will have to watch your environment to ensure that one tenant does not overload performance for others.

## Conclusion

No matter what solution you choose remember that agility and ability adapt are the things that define a SaaS solution

## Resources

- [DynamoDB Sharding strategies](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-partition-key-sharding.html)
- [Dynamic DynamoDB (DynamoDB Autoscaling)](https://dynamic-dynamodb.readthedocs.io/en/latest/index.html)
- [Redshift Concurrency Scaling](https://aws.amazon.com/blogs/aws/new-concurrency-scaling-for-amazon-redshift-peak-performance-at-all-times/)

## Challenges

- Learn how to shard a dynamodb table 
- Can you autoscale dynamodb through cloudwatch and lambdas instead of dynamic dynamodb
- How do you manage separate databases within a single instance
- What does sharding look like in general.
- How do you shard across RDS instances
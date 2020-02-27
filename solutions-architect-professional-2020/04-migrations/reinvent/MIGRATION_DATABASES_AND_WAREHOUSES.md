# Migrating Databases and Data Warehouses to the Cloud

https://www.youtube.com/watch?v=Y33TviLMBFY

## Cloud Benefits

- Automated provisioning, patching, scaling, backup/restore, failover
- HA with RDS Multi-AZ
    - 99.95 SLA for Multi-AZ deploymenys

- Lower TCO because of management
- Built in high availability and cross region replication
- Anyone can have 99.95% availability

## Journey

Migration use to involve cost + complexity + time.

Commercial migration

## What are DMS and SCT

DMS - easily and securely migrate databases and data warehouses to AWS

SCT - convert commercial database and data warehouse schemas to open source engines or AWS native services such as Aurora or Redshift

If you're not switching engines and can take downtime

- SQL Server: bak file import
- MySQL: read replicas
- Oracle SQL Developer, Data Pump, Export/Import
- PostgreSQL: pg_dump

## When to use DMS and SCT

- Modernize database tier
- Migrate business critical applications
- Replicate - create cross region replicas

DMS is great to migrate major versions of your database if you cannot afford downtime.

You can migrate between database i.e. Aurora -> MySQL and back again. It really allows you to modernize applications.

## Why use DMS and SCT

- Remove Barriers to entry
- Near zero downtime
- Secure
- Easy to use but sophisticated
- Allow DB freedom
- Cost Effective

Moving to AWS native solutions is free.

Recommended that you only move 15 TB over the internet a most, generally stick to 5 TB.

## Migration process

- Step one SCT - convert the schemas and can work with applications. It will also run a migration assessment
- Step two DMS - Connects to source and target database. Select tables and schemas that you would like to move. Then let AWS move the data, switch application to the target at your convenience. 

Works table by table. Migrates 8 Tables at a time.

Change data capture from the native logs. You need to enable this on the databases

You can consolidate multiple sources into a single target. 

For example multiple shards in MySQL into a single aurora instance.

Or you can take a single monolith and convert it into microservice databases

Or choose what you want from the database

You can also connect to S3.

You can create DMS tasks from the SCT tool.

With DMS you can do AOT assessment to proves that data types are going to work when switching between databases.

Database conversion is a project so it will never just be click button.


## Challenge

- Run the SCT locally 
- Figure out why Redshift cannot be a source
- Find swing bench alternatives
- DMS supported targets
- What DMS targets are free to migrate to?

## Resources

[DBeaver](https://dbeaver.io/)
[AWS SCT](https://aws.amazon.com/dms/schema-conversion-tool/)

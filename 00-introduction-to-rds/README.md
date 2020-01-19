# RDS Relational Database Service


## What is RDS

**Managed relational databases in AWS**

- Easier to set up, operate and scale your database
- Leveraging the flexibility of the cloud

The goal of the service is to lower the TCO (Total Cost of Ownership)

This means offloading the following:

- Provisioning
- Patching
- Backup
- Recovery
- Failure Detection
- Repair

Just Focus on your core business

## Terminology

- Database Instance - basic building block of amazon rds. An isolated database environment. Can contain multiple user crated databases
- Database Engine Type - MySQL, PostGresSql ,etc.
- Database Instance Type - determines the CPU and memory capacity i.e. db.m4.large
- Multi-AZ - hosted in two AZ's for high availability
- Read replicas - a node which is part of a database instance
- Primary host - the host within a database instance which handles the traffic from the client
- Secondary Host (Standby) - the host within a database instance which is not actively handling write traffic for your application. Redundancy
- Aurora - MySql and PostgreSQL compatible relational database, natively built for AWS. It has a significant performance improvement

## Setup

You will need a VPC with 2 public subnets, and two private subnets.

You will need to create 2 rds subnets with the 2 public and private subnets respectively.

## Provisioning a Database Instance

- Use Case
    - Production - Enables Multi - AZ by default
    - Dev / Test - Single AZ, 20 GB

- License Model 
    - BYO License or per hour licensing

- Database instance class
    - t series - burst
    - m series - gpu
    - r series - memory optimized

- Multi-AZ deployment
    - Achieve high availability in case of failure
    - Primary and Standby in different AZs

Storage Types and Allocation
- Aurora uses proprietary storage system
- all others use EBS
    - general purpose (gp2) - use to get started and upgrade to iops if necessary
    - provisioned iops (io1) - great for production work loads
    - magnetic - supported for legacy dbs


Costs
- Database engine and Version
- License Model
- Database instance class
- multi-az deployment
- Storage type and allocation

Database identifier
- identifies the database instance (cli, etc.)
- unique across the region

Credentials
- Master user account (varies across DB engines)
- Don't use these in your application, create a specific user

Backups
    - backups retention period
        - Range 0-35 days (default 7)
Maintenance
- Auto minor version upgrade
    - Automatic upgrades minor versions
- Maintenance window 
    - Aimed at patching and updates
    - Once a week time range when maintenance occurs

Database Engine upgrades
- Database engine upgrades requires downtime
- minor version upgrades - automatically or manually applied
- major version upgrades - manually applied
- Version deprecation - 3 to 6 six month notification before scheduled upgrades
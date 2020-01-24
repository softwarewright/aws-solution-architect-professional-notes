# AWS Storage Services Overview

https://d1.awsstatic.com/whitepapers/Storage/AWS%20Storage%20Services%20Whitepaper-v9.pdf


## Amazon S3 - **Scalable and highly durable object storage in the cloud**

Store objects from 0 - 5 TB of data.

### Storage Classes

- Standard - general purpose storage of frequently accessed data
- Standard-Infrequent Access (Standard-IA) - for long lived, but less frequently accessed data
- Glacier - for low-cost archival data

## Glacier

Archival Data Purposes, retrieval times generally complete in 3 to 5 hours.

There are SNS notifications for when retrievals are complete.

When considering usage think things that need to be stored, but are not immediately needed. 

When compared to S3 it's roughly 7x cheaper.

Life cycle polices simplify moving between the two.

For a single archive unit it can be up to 40 TB.

You can retrieve a range of bytes for the archive.

### Security

- IAM roles to control access permissions
- Vault Locks allow
    - undeletable records
    - time based data retention

### Logging

Much like S3 api access can be logged to cloud watch

## EFS (Networked Drive)

- NFS v 4

Used to give multiple EC2's access to the same file system.

Meant to meet the needs of big data and analytics, media processing, content management, web serving, and home directories.

### General Purpose vs Max I/O

When there will be less than 7000 file operation per seconds General Purpose works fine. When workloads exceeds with with up to thousands of EC2 accessing the same data opt for Max I/O (Provisioned Throughput).

### Security

- IAM
- Mount Points
- Security Groups for EC2s

## EBS

Block level storage for EC2 instances, these are network attached storage. 

They persist independent from the life of a single EC2.

An EBS can only be attached to a single EC2.

Snapshots of EBS volumes can be copied across regions

Types

- General Purpose SSD (gp2) - cost effective general purpose storage
- Provisioned IOPS SSD (io1) - databases
- Throughput Optimized HDD (st1) - big data
- Cold HDD (sc1)

EBS Snapshots can be copied across regions, used to create volumes within an AZ, shared with other accounts.

You are charged for allocated space, not used space

## EC2 Instance Store

Temporary storage that lasts only as long as the EC2 Instance is up and running. 

If the instance:

- Stop
- Restarts
- Fails
- Terminates

[Lifecycle](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html)

The data saved to the instance store will be terminated as well.

The storage capacity is fixed based on the instance type.

### Security

For encryption use your own encryption tools, or use third party encryption tools.

### Tips

The first write operation to any location on an instance store volume is slower than subsequent writes. If you need high performance prewarm the drives by writing once to every drive location before production use.

i2, r3, and hi1 don't require prewarming

## Storage Gateway

Secure storage integration between on-prem and cloud based storage.

Once installed and associated the Gateway can be used to create:

- Gateway Cached Volumes - primary data stored in AWS with frequently accessed data stored in AWS S3
- Gateway Stored Volumes - Store primary data locally while providing async back ups to AWS.  
- Gateway Virtual Tape Library - data archival

All of the above can be mounted as an iSCSI device by on prem devices

### Security

IAM for access management

All data is encrypted in transit to and from AWS using SSL

## Snowball

Accelerates moving large amounts of data in and out of AWS. Consider if loading data over the internet would take a week or more.

### Security

Encrypted using KMS on the appliance. In addition HIPPA compliant.


## Amazon Cloudfront

CDN webservice through edge locations

### Logging

CloudFront can create log files that contain information about every request recieved. In addition it intergrates with cloudwatch metics to monitor the website or application
# Amazon S3 & Amazon Glacier Storage Management

## S3

### Tagging

Object level tagging allows the following 

Tags can be added with the Put request

- Tag based permissions through IAM roles
``` json
{
    "version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::Project-bucket/*",
            "Condition": {"StringEquals": {"s3:RequestObjectTag/Project": "X"}} 
        }

    ]
}
```

### Monitoring

#### S3 Inventory

- Audit and report on replication and encryption of objects 
- Schedule alternative to listing objects
    - Outputs a csv of your objects and their metadata
    - Report can be encrypyted with SSE-S3 or KMS
- SNS or Lambda triggers on completion
- Costs 1/2 of what the sync list cost to execute
- Query with Athena, Redshift spectrum


Inventory is a part of the S3 console now

You can visualize the data through quicksight

#### Cloudwatch Storage Metrics

Free daily bucket level metrics
- Object count
- Bytes

Monitor performance and operation
- 1 minute metrics
- alert and alarm based on them

Types that you can monitor

- DELETE, HEAD, GET, PUT, requests
- 500 erros
- List requests
- etc

#### Storage Class Analysis

https://docs.aws.amazon.com/AmazonS3/latest/dev/analytics-storage-class.html

- Monitors access to understand the current storage usage and recommends when to move objects 

This can be visualized in QuickSight

#### CloudTrail

API logging with AWS CloudTrail, this is great for IT auditing and compliance needs to take action. These can spawn cloudwatch events.

Logging can occur at the object level (S3 Data Event)

Bucket level operations (Management)

Delivered within seconds

This can be configured under Properties on the bucket

#### Security Inspection

AWS Trusted Advisor - Can be used to check if a bucket is public or not

Bucket Permission Check, S3 Console. This can be viewed when looking under the bucket.


### Act

#### Lifecycle Policies

Automatically Transition or Expire your storage. Rules take action based on object age. This can be done to a bucket, prefix, or tag

####  Cross Region Replication

Replication accross regions

Use cases
- compliance 
- moves bucket closer to user

Benefits
- All uploads are replicated
- Secure Transfer
- Lifecycle policies are not replicated
- Supports KMS Encrypted objects
- Bidrectional replication is allowed

Under management tab of bucket under replication

#### Cross Region Replication Accounts

Why? Additional protection from malicious deletes

#### Automated with trigger based workflow

- Notification with Put, Post, Copy, multipart upload
- SNS, SQS, and lambda functions

#### Default Encryption

Automatically encrypt every object written to the S3 Bucket

#### Amazon Macie

Automatically discover, classify, and protect sensitive data in AWS.

## Challenges

- Setup S3 Inventory
    - Enable Amazon Athena querying
- Investigate quicksight
- Use S3 Events with SQS
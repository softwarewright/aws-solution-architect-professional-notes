# Migrations

## Strategies

Re-Host - "Lift and Shift" move assets with no change. For example move on-prem MySQL database to EC2 instance.

Low Effort, lower opportunity to optimize.

Re-Platform - "Lift and Reshape" Move assets but change underlying platform. Like moving on prem MySQL to RDS

Higher effort, with medium opportunity for optimization.

Re-purchase - "Drop and Shop"; Abandon existing and purchase new. Migrate from legacy on-prem CRM to salesforce.

Medium Effort and lower opportunity for optimization

Rearchitect - Redesign application in cloud native manner. Create a serverless version of the legacy application

Highest effort but highest opportunity to optimize.

Retire - get rid of application that aren't needed. End of life the app because it's simply not used.
Retain - "Do nothing option"; DEcide to reevaluate at a future date.

Little effort

## TOGAF

- Enterprise Architeture is not TOGAF
- But often fills a vacuum
- All practitioners are great practitioners
- It's a framework meant to be adopted it's not a cookbook

Architects should be boundary spanners

### What is a framework

- IS
    - information to get your head around a problem
    - open for localization and interpretation
    - something to adapt to organizational culture
- IS NOT
    - a perfect step-by-step recipe to success
    - something to hide behind with big works

### Cloud Adoption Framework

There is more to Cloud Adoption than technology, a holistic approach must be taken.

- Business - create strong business case for cloud adoption, business goals are congruent with cloud objectives, and ability to measure benefits TCO and ROI.
- People - Evaluate organizational roles and structures, new skills and process needs and gaps. Incentives and Career Management aligned with evolving roles. Training options appropriate for learning styles. Extremely important to making or breaking your migration. You need to look at existing roles and envision what they look like in a cloud future.
- Governance - Portfolio management geared for determining cloud eligibility and priority. Program and Project management for more agile projects. Align KPI's with newly enabled business capabilities.

- Platform - Resource provisioning can happen with standardization. Architecture patterns adjusted to leverage cloud-native. New application development skills and processes enable more agility.
- Security - Identity and Access Management modes change, Logging and audit capabilities will evolve. Shared responsibility removes and adds facets.
- Operations - Service monitoring can be automated, performance management can scale as needed, and business continuity and disaster recovery takes on new methods in the cloud.

## Hybrid Architecture

CLoud resources + on prem, a very common first step. Infrastructure can augment or be extensions of on-prem platforms. e.g. VMWare.

Ideally integrations are loosly coupled - meaning each end can exist without extensive knowledge of the other side.

### Example

- Storage gateway - creates a bridge between on-prem and AWS
- Seamless to end-users

- Middleware is often a great way to leverage cloud services. Loosely coupled.

- VMware vCenter Plugin allows transparent migration of VMs to and from AWS. Essentially scaling into the cloud.

## Migration Tools

- Storage Migration
    - AWS Snowball
    - Storage Gateway
- Server Migration Service
    - Automates migration of on-premise VMware vSphere to AWS
    - Replicates VMs to AWS, syncing volumes and creating periodic AMIs. Great for disaster recovery.
    - minimizes cutover downtime
    - Supports windows and linux VMs only just like AWS
    - The service migration connector is download as a virtal appliance on on-prem vSphere or Hyper-V setup
- Database Migration Service - works with the Schema Conversion tool, helps customers migrate databases to AWS RDS or EC2-based DBs. Copys database schemas between the same and different databases. Used for smaller simpler conversions and supports MongoDB and DynamoDB. SCT is used for larger, more complex datasets like data warehouses. DMS has a replication function for on-prem to AWS or to Snowball or S3.
- Application Discovery Service - Gathers information about on-prem data centers to help with planning. Helps learn the full landscape of services. Collects config, usage and behavior data from servers to help estimate TCO of running on AWS. Can run as agent-less.
- AWS Migration Hub - All of the above can be viewed here.

## Network Migration

- Ensure IPaddress will not overlap with VPC
- VPCs support IPv4 netmasks range from /16 to /28
- Remember - 5 IPs are reserved in every VPC subnet that will take up addresses

- Most orgs start with VPN
- As they grow switch to Direct Connect with VPN as backup
- Transition can be seamless with BGP
- Once Direct Connect is set-up configure both the VPN and Direct connect within the same BGP prefix
- From the AWS side, the direct connect path is always preferred, but you need to ensure the same on your network.

## Snow Family

- Evolution of the Import/Export process
- Data transfer is fast or slow as you are willing to pay 


- AWS Snowball - copy up to 80TB of data then ship it to AWS
- Snowball Edge - Snowball with Lambda and Clustering
- AWS Snowmobile - Up to 100PB of data 


## Exam Tips

Migration Strategies
- Understand possible strategies and trade offs 

Cloud Adoption Framework
- Know what a "framework" is and realistic expectations around it
- Understand high-level components of Cloud Adoption Framework
- Know that cloud adoption is only partially a tech effort

Hybrid Architectures
- Be able to speak to some typical hybrid architectures using both on-prem and cloud assets.
- Know that VMware has some tools for abstracting workloads across on-prem and cloud

Migrations
- Understand different services and tools available for migrating servers, storage, and databases
- Storage Gateway

Network Migration and Cutover
- Know various hybrid networking achitectures 
- Understand smooth transitions

## Pro Tips

- Tech is often a minor part of cloud migration
- Project management discipline is a must
- Adapt the Cloud Adoption Framework for your own organizations culture
- Leverage the CAF to get buy-in by acknowledge the enterprise nature of cloud migrations
- Be a boundary spanner!

## Whitepapers

- https://d1.awsstatic.com/whitepapers/Migration/aws-migration-whitepaper.pdf
- https://d1.awsstatic.com/whitepapers/aws_cloud_adoption_framework.pdf
- https://d1.awsstatic.com/whitepapers/Migration/migrating-applications-to-aws.pdf
- https://d1.awsstatic.com/whitepapers/AWS-Cloud-Transformation-Maturity-Model.pdf

## Re:invent

- [Assessing Migration Readiness](https://www.youtube.com/watch?v=id-PY0GBHXA)
- [Migrating Databases and Data Warehouses to the Cloud](https://www.youtube.com/watch?v=Y33TviLMBFY)

## Challenges

- Use the Server Migration Service
- Use the Database Migration Service
- Investigate the AWS Migration Hub
- Convert a MongoDB Database to DynamoDB using AWS DMS - https://medium.com/faun/database-migration-from-mongodb-to-amazon-dynamodb-with-aws-dms-25e36ea1500b


## Resources 
- https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html
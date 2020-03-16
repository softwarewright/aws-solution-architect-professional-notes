# Security

Principle of Least Privilege - Give users (or services) nothing more than those privileges required to perform their intended function. (and only when they need them).

## Concepts

### Security Facets

| Facet | Description | AWS Example|
|-------|-------------|------------|
| Identity | Who are you? | Root account user, IAM user, temp security credentials |
| Authentication | Prove that you're who you say you are | MFA, Client-side SSL Cert |
| Authorization | Are you allowed to do this? | IAM Policies |
| Trust | Do other entities that I  trust say they trust you | Cross Account Access SAML-based Federation, Web Identity Federation |

Components

- Identites - people, computers, etc
- Identity store
- Identity Broker
- Federation


### SAML vs OAuth vs OpenID

#### SAML 2.0

- Can handle both authorization and authenication
- XML based protocol
- Can contain user, group membership and other useful information
- Assertions in the XML for authentication, attributes or authorization
- Best suited for Single Sign-on for enterprise users

#### OAuth 2.0

- Allow sharing of protected assets without having to send login creds
- Handles authorization only no authentication
- Issues tokens to the client
- Application validates token with Authorization Server
- Delegate access, allowing the client applications to access information on behalf of user
- Best suited for API auth between apps

#### OpenID connection

- Identity layer built on top of OAuth 2.0, adding authentication
- Uses REST/JSON message flows
- Supports web clients, mobile, clients, JavaScript clients
- Extensible
- Best suited for single sign on for consumer

### Compliance

AWS has implemented processes, practices, and standards, as prescribed


## Multi-Account Management

When should you use multiple accounts?

- Do you need administartive isolation between workloads
- Do you require limited visibility and discoverability of workloads?
- Do you require isolation to minimize blast radius
- Do you require strong isolation of recovery and/or auditing data 

### Tools For Account Management

- AWS Organizations
- Service Control Policies - polices that can be applied to sub accounts to restrict access
- Tagging
- Resource Groups
- Consolidated Billing

### Identity Account Structure

- Manage all user accounts in one location
- Users trust relationship from IAM roles in sub-accounts to identity account to grant temporary access (Cross Account Roles)
- Variations include by business unit deployment environment, geography

### Logging Account Structure

- Centralized Logging repository
- Can be secured so as to be immutable
- Can use Service control polices to prevent sub accounts from changing logging settings

### Publishing Account Structure

- Common repository for AMI's, Containers, and Code
- Permits sub-accounts to use pre-approved standardized services or assets

### Information Security Account Structure

- Hybrid of consolidated security and logging
- Allows one point of control and audit
- Logs cannot be tampered with by sub-account users

### Central IT Account Structure

- IT can manage IAM users and groups while assigning to sub-account roles
- IT can provide shared services and standardized assets (AMI's, databases, EBS, etc) that adhere to corporate policy

### Consolidated Security

Consolidated Billing - Target of all accounts to invoice into a single account to get greater benefits.

Consolidated security - This will be where AD will be setup, or you can setup all of your IAM users that will be able to communicate across all accounts.

Service Control Polices - Uses these to limit access to AWS services in lower accounts for example you can block the ability to modify cloudtrail logs etc.

## Network Controls

### Security Controls

- Firewalls for assets (EC2, RDS, AWS Workspaces)
- Controls inbound and outbound traffic for TCP, UDP, ICMP or custom protocols
- Port or port ranges
- Inbound rules are by source ip, subnet, or other security group
- Outbound rules are by destination IP, subnet, or other security group

### Network Access Control Lists

- Additional layer of security for VPC that acts as a firewall
- Apply to entire subnets rather than individual assets
- Default NACL allows all inbound/outbound traffic
- NACLs are stateless - meaning outbound traffic simply obeys outbound rules - no connection is maintained
- Can duplicate or further restrict access along with security groups
- remember ephemeral ports for outbound traffic if you need them

### Why use SG's and NACL's

- NACLs provide a backup method of security if you accidentally change your SG to be too permissive
- Covers the entire subnet so users to create new instances and fail to assign proper SG are still protected
- Part of multi-later least privilege concept to explicitly allow and deny

## AWS Directory Services

| Directory Service Option | Description | Best for...|
|--------------------------|-------------|------------|
| AWS Cloud Directory | Cloud-native directory to share and control access to hierarchal data between applications | Cloud applications that need hierachical data with complex relationships |
| Amazon Cognito | Sign-up and sign-in functionality that scales to millions of users and federated to public social media services | Develop consumer apps or SaaS |
| AWS Directory Service for Microsoft Active Directory | AWS-managed full microsoft AD running on windows server 2012 R2 | Enterprises that want hosted microsoft AD or you need LDAP for linux apps |
| AD connector | Allows on-premises users to log into AWS services with their existing AD credentials. Also allows EC2 instances to join AD domain. | Single sign-on for on-prem employees and for adding EC2 instances to the domain |
| Simple AD | Low Scale, low cost AD implementation based on Samba | Simple user directory, or you need LDAP compatibility |


### AD Connector vs Simple AD

AD Connector
- Must have existing AD
- Existing AD users can assets via IAM roles
- Supports MFA via existing RADIUS based MFA infrastructure

Simple AD
- Stand-alone AD based on Samba
- Supports user accounts, groups, group policies and domains
- Kerberos-based SSO
- MFA not support
- No Trust Relationships

## Credentials and Access Management

- Know what IAM is and components
- Users, Groups, Roles, Polices
- Resource-based policies vs Identity Based Policies
- Know how to read and write policies in JSON
- Services -> Actions -> Resources

### AWS STS

Temporary access to AWS credentials

### Cognito

Works great for web and mobile application 

### Token Vending Machine Concept

- Common way to issue temporary credentials for mobile app development
- Anonymous TVM - Used as a way to provider access to AWS services, does not store identity
- Identity TVM - used for registration and login and authorizations
- AWS recommends cognito and the related SDK

### Secrets Manager

- Store passwords, encryption keys, API keys, SSH Keys, PGP Keys etc
- Alternative to storing passwords or keys in a "vault"
- Can access secrets via API with fine-grained access controls provided by IAM
- Automatically rotate RDS data creds for MySQL, PostgreSQL, and Aurora
- Better than hard-coding creds in scripts or application

## Encryption

Encryption at rest - encrypted where it is stored
Encryption in transit - as data flows through networs (ssl, https)

### Key Management Service (KMS)

- Key storage, management and auditing
- Tightly integrated into MANY AWS services. Lambda, S3, EBS, EFS, DynamoDB, SQS, etc
- You can import your own keys or have KMS generate them
- Control who manages and accesses keys via IAM users and roles
- Audit use of keys via CloudTrail
- Differs from Secret Manager as it's purpose is for encryption key management
-  Validated by many compliance schemes

### CloudHSM (When you need more than KMS)

- dedicated hardware device, single tenanted
- must be within a VPC and can access via VPC peering
- does not natively inetegrate with many AWS services like KMS, but rather requires custom application scripting
- Offload SSL from web servers, act as an issuing CA, enable TDE for oracle dbs

### CloudHSM vs AWS KMS

CloudHSM

- Single-Tenant HSM
- Customer-managed durability and available
- Customer managed root of trust
- Level 3 FIPS 140-2
- Broad 3rd Party Support

AWS KMS

- Multi-Tenant AWS Service
- HA and durable key storage management
- AWS managed root of trust
- Level 2 / Level 3 in some areas
- Broad AWS Service Support

### AWS Certificate Manager

- Managed for provisioning, managing, and deploying public or private SSL/TLS certs
- Directly integrated into many AWS services (Cloudfront, ELB, API Gateway)
- free public certs to use with AWS services, but you can import 3rd party certificates
- Supports wildcard domains to cover all subdomains
- Managed certificate renewal
- Can create a managed private certificate authority as well as for internal or proprietary apps services or devices

## Distributed Denial of Service Attack

- Overwhelming with traffic
- Amplification/Reflection Attacks
- Application Attacks - flood with get requests

### Mitigating

- Minimize attack surface - NACLs, SGs, VPC Design
- Scale to absorb attack - Auto-Scaling Groups, AWS CloudFront, S3
- Safeguard exposed resources -  Route53, AWS WAF, AWS Shield
- Learn normal behavior - AWS GuardDuty, CloudWatch
- Have a plan - what will you do, what steps will you do

### Intruder Prevention and Detection (IPS/IDS)

- Intruder Detection System - watches the network and systems for suspicious activity that might indicate someone trying to compromise a system

- Intruder Prevention System - tries to prevent exploits by sitting behind firewalls and scanning and analyzing suspicious content for threats

- Normally comprised of a collection / monitoring system and monitoring agents on each system

- Logs collected or analyzed in CloudWatch, S3 or third party tools (Splunk, SumoLogic) sometimes called Security Information and Event Management System

### CloudWatch vs Cloud Trail

CloudWatch

- Log events across AWS services, operations
- Higer level monitoring eventing
- Log from multiple accounts
- Logs stored indefinitely
- Alarms history 14 days

CloudTrail

- Log API activity across AWS services, activities
- More low level
- Log from multiple accounts
- Logs stored in S3 or CloudWatch indefinitely
- No native alarming, can use cloudwatch alarms

## AWS Service Catalog

- Framework for allowing admins to create predefined products
- Granular control over which users have access to which offerings
- Makes use of adopted IAM roles so users don't need underlying service access
- Allows end users to be self sufficient while upholding enterprise standards for products
- Based on CloudFormation templates
- Administrators can version and remove products. Existing running product versions will not be shutdown.

### AWS Service Catalog Constraints

Launch Constraints - IAM role that service catalog assumes when an end user launches a product. Without this the end-user must have all permissions needed within their own IAM creds.

Notification Constraint - Sepcifies the AWS SNS topic to recieve notifications about stack events. Can get notifications when products are launched or have problems.

Template Constraint - One or more rules that narrow allowable values an end-user can select. Adjust product attributes based on choices a user makes. e.g. only allow centain instances types for DEV environment. A wizard like experience.

Works well with multi-account scenarios, this is because the catalogs can be shared across accounts.


## Exam Tips

Multi-Account Management

- Know the different models and best practices for cross account management of security
- Know how roles and trusts are used to create cross-account relationships and authorizations

Network Controls and Security Groups

- Know differences and capabilities of NACLs and SGs
- NACLs are stateless
- Get some hands-on with NACLs and SGs to reinforce your knowledge
- Remember ephemerals

AWS Directory Services

- Understand the types of Directory Services offer by AWS - especially AD connector and Simple AD
- Understand use-cases for each type of Directory Service
- Be familiar with how on-prem AD implentation might connect to AWS and what functions that might enable

Credential and Access Management

- Know IAM and its components
- Know how to read and write IAM policies in JSON
- Understand Identity Brokers, Federation, and SSO
- Know options and steps for temp authorization

Encryption

- Know differences between AWS KMS and CloudHSM
- Need two CloudHSM for HA
- Test likely is restricted to the "classic" CloudHSM
- Understand AWS certificate manager and how it integrates with 
other AWS services

DDoS Attacks

- Understand what they are and some best practices to limit your exposure
- Know some options to mitigate them using AWS services

IDS/IPS

- Understand the difference between IDS and IPS
- Know that AWS Service can help with each
- Understand the differences between CloudWatch and CloudTrail

Service Catalog

- Know that it allows users to deploy assets through inheriting rights
- Understand how service catalog can work in a multi-account scenario

## Protips

- Know that security will be front of mind for every client evaluating the cloud but rarely are there sound processes in place

- Acknowledge concerns and be ready with a process (Cloud Adoption Framework is a good start)

- Leverage assessments and checklists as illustrators of care and best-pratice.

- Migrating to the cloud is often more secure than on-prem due to increated transparency and visibility

- Speack in terms of risk as a continuum rather than an absolute

- Consider AWS Cerified Security - Specialty or other security minded cert (CISSP)

## Whitepapers

- https://d1.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf
- https://d1.awsstatic.com/aws-answers/AWS_Multi_Account_Security_Strategy.pdf
- https://d1.awsstatic.com/whitepapers/Security/Intro_to_AWS_Security.pdf?did=wp_card&trk=wp_card

### Additional Whitepapers
- [Security](https://aws.amazon.com/whitepapers/?whitepapers-main.sort-by=item.additionalFields.sortDate&whitepapers-main.sort-order=desc&awsf.whitepapers-content-type=content-type%23whitepaper&awsf.whitepapers-category=categories%23security-identity-compliance#security)
- [AWS Governance at Scale](https://d1.awsstatic.com/whitepapers/Security/AWS_Governance_at_Scale.pdf?did=wp_card&trk=wp_card)
- [AWS Cloud Security](https://aws.amazon.com/security/?ref=wellarchitected-wp)
- [AWS Security Pillar](https://d0.awsstatic.com/whitepapers/architecture/AWS-Security-Pillar.pdf?ref=wellarchitected-wp)
- [AWS Compliance](https://aws.amazon.com/compliance/?ref=wellarchitected-wp)
- [AWS Security Whitepaper](https://d0.awsstatic.com/whitepapers/Security/AWS%20Security%20Whitepaper.pdf?ref=wellarchitected-wp)
- [AWS Security Best Practices](https://d1.awsstatic.com/whitepapers/Security/AWS_Security_Best_Practices.pdf)
- [AWS Risk and Compliance](https://d0.awsstatic.com/whitepapers/compliance/AWS_Risk_and_Compliance_Whitepaper.pdf?ref=wellarchitected-wp)

## Reinvent

- [Best Practices for Managing Security Operations on AWS](https://www.youtube.com/watch?v=gjrcoK8T3To)
- [IAM Policy Mastery in 60 mins or less](https://www.youtube.com/watch?v=YQsK4MtsELU)
- [Security Anti-Patterns: Mistakes to avoid](https://www.youtube.com/watch?v=tzJmE_Jlas0)
- [Architecting Security and Governance Across a Multi-Account Strategy](https://www.youtube.com/watch?v=71fD8Oenwxc)
- [Managing Multi-Account AWS Environments Using AWS Organizations](https://www.youtube.com/watch?v=fxo67UeeN1A)

### Additional Videos

- [Using Analytics to set Access Controls](https://www.youtube.com/watch?v=6EIsRI4CD10)

## Challenge

Setup a few of the AWS Directory offerings
    - Simple AD
    - AD Connector
    - Cloud Directory
    - Cognito

Setup rotating RDS credentials for MySQL
Setup a WAF infront of your services

Setup another AWS account and integrate the accounts

Search the Cloud Adoption Framework

What is Samba?


## Resource

 - https://web-identity-federation-playground.s3.amazonaws.com/index.html

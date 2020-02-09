# AWS Multiple Account Security Strategy

## General Best Practices

Do not over-engineer the initial account structure in an attempt to create a perfect, immutable set of account. Instead figure out the different ways that your company will use AWS accounts and take an iterative approach to structuring and securing them.

- Clearly define an AWS account-creation process.
    - Who is creating accounts
    - What will they be used for
    - Centralization of account creation is not required as long as the process is well defined and it allows a business to maintain an account inventory
- Define a company-wide AWS usage policy
    - Implement well defined AWS usage policies include minimal security baseline requirements such as encryption and approved services
    - Vet policies with company stakeholders
- Create a security account structure for managing multiple accounts
    - Centralize security monitoring and management
    - Manage identity and access
    - Provider audit and compliance monitoring services

## Implementation Considerations

### When to Create Multiple Accounts

Answering Yes to any of the following questions is a good indication to consider creating additional AWS accounts.

- Does the business require administrative isolation between work loads?
- Does the business require limited visibility and discoverability of workloads
- Does the business require isolation to minimize blast radius?
- Does the business require strong isolation of recovery and/or auditing data?

### When to create a Security Account Structure

- Do you want to managing AWS user identities in one account and federate access to other accounts?
- Do you want to centrally store, secure, analyze, and report on AWS generated log data from services?
- Do you want to empower security and compliance organizations to apply security baselines and monitor security compliance across multiple AWS accounts?
- Do you want to centrally manage approved EC2 AMIs, or AWS Service Catalog portfolios and products?

### AWS Security Account Structures

Use organizational units with AWS organizations to create hierarchical and logical account groupings. When creating a new AWS account in an organization you can use Service Control Polices to filter and restrict the services and actions its users and roles can access.

#### Identity Account Structure

When you want to create and manage users in a single account and enable user and group access to resources in other accounts. The IAM roles give temporary access to other accounts.

You have the ability to use existing Active Directory AD credentials and and automatically mangage IAM roles and permissions needed to give federated users and groups access to multiple AWS accounts.

100000000
#### Logging Account Structure

When you want to centrally store, secure, process AWS log and configuration data. In the parent account make SCP to prevent lower accounts from changing configuration settings. These settings will be stored into S3 in the parent account which should have MFA delete projection or Glacier Vault Lock enabled.

In addition you'll want to forward log data from child accounts to the parent, thus simplifying global logging.

#### Publishing Account Structure

When you want to manage preapproved server images and AWS CloudFormation templates across a company. The model leverages cross account resource sharing to are AMIs and AWS Service Catalog portfolios created in a parent account with one or more associated accounts.


### Hybrid AWS Security Account Structures

#### Info Sec Account

These accounts assume the responsibility for collecting and analyzing security-related data. running compliance scripts, configuring security services (CloudTrail and AWS Config), or manage IAM access across other accounts.

This is generally owned by the Info Security deparatment that monitors and enforces security compliance through cross-account role access within the multi-account structure.

#### Central IT Account

Generally used to host identity repositories (IAM, AWS Directory Service, etc.), federate user access, centrally manage shared AMIs, EBS snapshots, or service catalog portfolios, and provide centralized DNS, logging, configuration management, or software development services.

## Challenge

- Setup centralized logging through Elasticsearch and just CloudWatch through to a central account

- [ELK Stack](https://aws.amazon.com/elasticsearch-service/the-elk-stack/)
# Managing Multi-Account AWS Environments Using AWS Organizations

https://www.youtube.com/watch?v=fxo67UeeN1A

## What is an AWS account

- Compartment for AWS resources
    - S3 buckets
    - EC2 instances
    - DynamoDB databaess
- Access to resources that are controlled using AWS IAM principles (users and roles)

### Single account vs Multiple Accounts

Multiple accounts - Categorized resources into their own containers

## Why use multiple accounts?

- Group resources for categorization and discovery
- Improve your security posture with logical boundary
- Limit blast radius in case of unauthorized access
- More easily manage user access to different environments

If Team A cannot support Team B's app when paged at 2 AM, the applications probably shouldn't be in the same account.

## What is AWS Organizations?

Central governance and management across AWS accounts.

Features

- Manage and define your org accounts
- Control access and permissions
- Audit monitor and secure your environment for compliance.
- Share resources across accounts
- Centrally manage costs and billing

### Difference from control tower

[AWS Control Tower](https://aws.amazon.com/controltower/pricing/)

- Provides automated setup of a secure, well architected environment
- Abstracts multiple AWS Services
- Best suited if you want automated deployment of a multi-account environment with AWS best practices
- Must be configured on a new environment that does not already have AWS organizations deployed

AWS Organizations

- Provides central management and governance for multiple AWS Accounts
- Best suited if you want to define your own custom multi-account environment with advanced governance and management capabilities.

### Migrating from consolidated billing orgs

- If you receive a single bill monthly for multiple accounts you're using AWS Orgs
- This is legacy 
- No changes occur to your organization when you migrate

## Account strategy with AWS Orgs

Terms
- Master account 
    - used to create the orgs (payer account)
    - central management and governance hub
- Organizational Unit (OU)
    - Set of AWS accounts logically grouped within an org
- Policy
    - Document describing controls to be applied to a selected set of accounts

### AWS accounts and OUs

- Segment your applications and workloads into AWS accounts
- Group accounts into OUs to simplify management
- Create multiple OUs within a single org
- Nest OUs inside other OUs to form hierarchical structure

### Organizing service accounts into OUs

- How many business units will you grow to?
- What are the commonalities among account guardrails?
- How much can you automate


**OUs can only be 5 levels deep currently**

### Capabilities of AWS Organizations

- Create new AWS accounts programmatically
- Group accounts into OUs for managment
- Centrally provision accounts
- Tag AWS accounts 
- Manage service quotas for new accounts, service quotas organization template

#### Control access and permissions

- Deploy console and CLI access to accounts (AWS Single Sign-On)
    - Enable AWS SSO
    - Connect your corporate identities
    - Grant SSO access to your accounts and applications
    - Manage use permissions centrally
- Define permissions based on membership in an organization
- Author fine grained permission guardrails across accounts
    - SCPs which allow resource and conditional elements
        - Define max permissions for IAM entities in an account
        - Do not grant permission
        - ATtach SCPs to organization root, OUs, and individual accounts
        - SCPs attached to the root and OUs apply to all OUs and accounts inside them
        - Great for ensuring users are only using HIPPA compliant resources


#### Shared resources across accounts

- AWS Service Catalog
- AWS Resource Access Manager
    - VPC transit gateway and subnets
    - AWS license manager configurations
    - Amazon Route53 resolver rules

#### Centrally manage costs and billing

- Consolidate usage across all accounts into a single bill
- Manage your tax settings across accounts from a central Tax console
- Gain insights and manage spending across your organization (AWS Budgets and AWS Cost Explorer).

#### Examples

- Deny access to AWS based on the requested region
- Prevent IAM principals from making changes to a common administrative IAM role
- Prevent modification of AWS CloudTrail Logs


## Resources

- [IAM NotAction](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_notaction.html)
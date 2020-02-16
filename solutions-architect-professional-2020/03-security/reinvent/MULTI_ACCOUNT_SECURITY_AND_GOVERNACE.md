# Architecting Security and Governance Across a Multi-Account Strategy

https://www.youtube.com/watch?v=71fD8Oenwxc

## Account Models

One Account vs 1000s of Accounts

Most people of between these. 

### Why one account isn't enough

Many Teams, Isolation, Security Controls, Business Processes, and Billing Isolation.

## Case Study Thomson Reuters' Multi Account Strategy

Connecting from data centers to AWS

- Direct Connect
- 

AWS Regions
- Data residency
- New growth opportunities
- Latency Requirements

AWS accounts
- Project isolation
- AWS API limits
- Billing separation

Hard stops

- Private VIF - 50
- VPN connections
- VPC Peering

### Account Creation

Use organization API to create accounts

- Create new account
- Assume created cross account role
- Bootstrap with foundational things - VPCs, networking topology, etc
    - Vault root credentials
    - Create service management records
    - Federated with corportate identity provider
        - Create intial operations roles
    - VPC and network setup
        - AWS Direct Connect/VPC Peering
    - Security Controls and logging setup
        - Enable logging
        - create security and custodian IAM roles
- Move the account into an Organizational Unit
- Delete Organizations Role - This has a lot of permissions

The above means
- self-service account creation
- Enables use of service control policies
    - Enforce that no one can delete CloudTrail logs

### Logging Account

Store

- CloudTrail Logs
- S3 Logs


- Single source of truth
- One place to secure
- Very limited access
- Multiple Amazon S3
- Add read-only access when needed

### Custodian account

- Trust but verify
- Need a single pane of class into all accounts 
- Notify as opposed to enforce
- Select tooling to enforce this tooling 

### Security Account

- Process logs
- Host tooling for threat dection
- Perform incident management 
- Conduct Security Audit

### Shared Services Account

- Direct Connect
- DNS servers
- Bastion hosts
- Network monitors 
- Building AMIs

Consider separating business critical services

- Direct connect
- DNS
- Shared Services

This means you can give more limited access and reduce blast radius.

### Sandbox Accounts (Per Business Unit)

- Team innovation

- No DC connectivity
- Multi-tenant
- Restrictive permissions
- Full account inflation (similar to higher level accounts)


There is the accounts per developers as well

- Learn and experiment
- No DC connectivity
- Single tenant
- Full permissions 
- Minimal account inflation

Things to consider with per developer

- Monthly spending
- Onboarding and Offboarding

### SDLC Accounts

- Dev
- Staging
- Prod
- Disaster Recovery


### Multi-tenant Resource Isolation Using IAM

- IAM can give you resource isolation
- Setting tag conditions and resource names in IAM policies 
- Comes with overhead
- Not 100% coverage
- Policy templates and automation

### CI/CD Account

- Host CI/CD pipelines
- Perform chaos engineering
- One per business unit
- Cross account roles

Steps
1. Build artifact
2. For each account (env)
    1. Assume Role
    2. Deploy AWS CloudFormation
    3. Deploy Application

This means deploying your applications to other accounts becomes simple since the CI/CD account just needs permissions to another account to deploy to.

## Multiple Accounts

Pros
- Complete security and resources isolation
- Smaller blast radius
- Simplified billing per account

Cons
- Aggregation and Distribution
- Setup and operation overhead
- More complex security policies across accounts

### Goals

- Automated
- Scalable - as you create accounts 
- Self-service 
- Guardrails NOT Blockers
- Auditable
- Flexible - as new services are added you need to change with them

### Account Security - Day 1

- AWS Account Credentials Management - lock the root account away
- AWS CloudTrail
- Map Enterprise Roles 
- Federation
    - Joining the org means that you have permissions
    - Leaving means that you lose them
- InfoSec's cross account roles 
- Actions and Conditions - Default encryption on S3 and etc

### What Accounts Should I Create

- Org - No connection to DC, Service control policies, Consolidate Billing, Volume discount, minimal resources and limited access. **Delete Orgs role when you are done with it.** You can stop IGW from being attached
    - Logging - Versioned S3 Bucket. Restricted MFA delete. CloudTrail Logs, security logs, it's a single source of truth. Limited access
    - Security - Security and Auditing tools. Cross Account read/write. Limited Access.
    - Billing - Reduces access to master organizations account. Billing reports and usage metrics and reporting. Usage optimizations and RI management
    - Shared Services - Connected to DNS, AMIs, pipeline. Scanning infrastructure for inactive instances, improper tags, and snapshot lifecycle. Monitoring
    - Direct Connect - Limited access, send logs to logging account, managed by the networking team.
    - Internal Audit - Regulatory compliance, read-only access to the logs
    - Developer Sandbox - Innovation space with fixed spending limit. Autonomous and experimentation.
    - SDLC - Match your development lifecycle
        - Dev
        - Stage - Connect to DC, production-like. 
        - Prod - Promoted from pre-prod, limited access. Be as automated as possible.
    - BU Sandbox - Work together to figure out solutions for the Business Unit
    - Other
        - Be flexible
        - Regulatory/compliance
        - Additional isolation/security controls (PII)
        - Complex platform / product

## Next Steps

- Define tagging strategy
- Define automation strategy
- Creation accounts
    - Org
    - Logging
    - Security
    - Shared Services
    - Billing Tooling account
    - Developer Sandbox Account(s)

### Common Checklist

- Secure Root Credentials
    - MFA
    - Complex Password
        - Establish rotation policy
- Link to organizations master account
- Enable CloudTrail in all regoins, send to logging account
- Enable AWS Config, send to logging account
- Create read-only cross-account security role
- Create read/write cross-account security role
- Create VPC
- Enable federation into account
- Define roles and access polices
- Use group email/phone as contact info
- peer VPC with shared services
- Add a policy around prefix naming conditions to every account

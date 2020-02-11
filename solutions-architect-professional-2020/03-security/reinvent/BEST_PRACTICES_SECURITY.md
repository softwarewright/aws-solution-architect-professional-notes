# Best Practices for Managing Security Operations on AWS

## Understand the boundary

Could be trying to minimize impact, blast radius, segregation.

In-VPC
- SGs NACLs
- AWS IAM Resource level constraints

VPC as a boundary (single account)
- Equivalent to separate networks
- Peering, routing
- AWS IAM similar to previous

AWS Account as the boundary
- Highest degree as the boundary
    - By data classification
    - Business unit
    - Workload
    - Functional

You need to know the boundaries to know how to separate


Regardless of boundaries consider:

- How to aggregate logging
- SecOps Dedicated account

## Consistent Controls

AWS Organizations with policy based management for the multiple accounts (SCP).

## Test Often, fail early

Use Code Pipeline to validate that the code is in a valid state.

Stop things like:

- creating security groups that allow ssh from everywhere

You can also trigger vulnerability scans.

## Closed-loop mechanisms

Control -> Monitor -> Fix

Consider making alerts for things that you don't expect. For example you would not want to create an alert for someone creating a instance, but you would want to create an alert for someone creating an instance in a region that you don't expect.

### Examples

- Example 1:
    - Control - API calls (Cloud Trail) are logged
    - Monitor - StopTrail/Change
    - Fix - Turn back on
- Example 2:
    - Control: SSH only from bastion subnet
    - Monitor: Create/Change sgs validate source if port == 2
    - Fix - Change SG via Lambda
- Example 3:
    - Control: All instances patch up to date
    - Monitor: AWS EC2 Systems Manager + AWS Config rules
    - Fix: Patch via systems manager
- Example 4:
    - Control: No root access
    - Monitor: CloudWatch Logs + Syslog
    - Fix: Isolate + Investigate
- Example 5:
    - Control: No public objects in S#
    - Monitor: Object level logging in CloudTrail
    - Fix: Make object private

## Full stack

## Practice

S.I.R.S - Security Incident Response Service

## Visibility

## Netflix SecOps

Tools - CloudTrail, CloudWatch, SDKs

### CloudTrail

24/7 monitoring of AWS API actions, and identification of users making calls. These CloudTrail events allow you to ensure when something is stood up we know who did it.

CloudTrail can help you figure out how to downsize permissions of your roles.

### CloudWatch Events

Realtime Event changes in the account.

Using cloudwatch event buses you can centralize events.

As AWS releases new things think about if you should change things.



- Look into AWS Config
- Look into EC2 Systems Manager
- What is Amazon WorkMail
- Use AWS Config, this replaces Security monkey by netflix
- Investigate Awwwdit
- Investigate Historical 
- https://github.com/netflix/chaosmonkey
- https://www.spinnaker.io/
- https://github.com/cloud-custodian/cloud-custodian
# Introduction to AWS Security

## Security of the AWS Infrastructure

AWS operates under a shared security responsibility model, where AWS is responsible for securing underlying cloud infrastructure and you are responsible for securing workloads you deploy to AWS.

AWS is responsible for the security of the cloud (storage, database, regions, azs) and you are responsible for security in the cloud (server-size encryption, client side data encryption, IAM, customer data).

## Security Products and Features 

### Infrastructure Security

- Network firewalls built into Amazon VPC allowing private networks and control access. You can also control encryption in transit.
- Optional private, or dedicated connections from office/on-prem
- DDoS mitigation applied at layers 3,4, and 7
-  Auto encryption of all traffic on AWS global and regional networks between AWS facilities

### Inventory and Config Management

- Deployment tools to manage creation/destruction of AWS resources at organizational levels
- Inventory and configuration managements tools to identity AWS resources and then track and manage changes to those resources
- Template definition and management tools to create standard, preconfigured, hardened virutal machines 

### Data Encryption

- Data at rest encryption capabilities in most AWS services (S3, EBS, RDS, ...)
- Flexible key management options
- Dedicated, hardware based crypto key storage using CloudHSM
- Encrypted message queues (SQS)

### Identity and Access Control

AWS offers you capabilities to define, enforce, and manage user access policies across AWS services. These include:

- AWS IAM - define individual user accounts with permissions across AWS resources AWS Multi-Factor Authentication for privileged accounts. Along with the ability for federated access to AWS management console and AWS service APIs using existing identity systems.
- AWS Directory Service - allows you to integrate and federate with corporate directories
- AWS SSO - allows you to manage SSO access and user permissions to all of your accounts in AWS Organizations

### Monitoring and Logging

- CloudTrail - monitor all AWS API calls in/to your account. Identify which users and accounts called the AWS APIs for services
- CloudWatch - providers reliable, scalable, and flexible monitoring solution
- GuardDuty threat detection service that continuously monitors for malicious activity and unauthorized behavior to protect your AWS accounts and workloads.

## Security Guidance

- AWS Trusted Advisor - helps configure resources with best practices, it inspects your AWS environment to help close security gaps and find opportunities 
- AWS account teams provides first point of contact, guiding you through deployment and implementation, and pointing toward the right resources to resolve security issues you may encounter.
- AWS Enterprise Support - Provides 15 min response time and is available 24x7
- AWS Partner Network - hundreds of industry leading products that match on-premises environment controls.
- AWS professional services - Security, risk, and compliance specialty practice to help develop confidence and technical capability when migrating.
- AWS marketplace - thousands of software listings from independent software vendors to run on AWS.
- AWS Security Bulletins - bulletins around current vulnerabilities and threats, and enables customers to address concerns
- [AWS Security Docs](https://docs.aws.amazon.com/security/) - shows how to configure AWS services to meet security and compliance objectives
- AWS Well Architecture Framework - helps build secure, high-performing, resilient, and efficient infrastructure for apps
- AWS Well Architected Tool - helps review the state of workloads and compares them to the latest AWS architectural best practices. After answer questions it will provider a plan on how to architect for the cloud using best practices.

## Compliance

[AWS Compliance Programs](https://aws.amazon.com/compliance/programs/)

By operating in an accredited environment customers reduce the scope and cost of audits.

You also have the ability to take advantage of tools like AWS Security Hub, AWS Config, and AWS CloudTrail to validate compliance.

These tools reduce the effort needed to perform audits since these tasks become routine and automated.

## Resources

- [AWS Security Documentation](https://docs.aws.amazon.com/security/)
- [AWS Well-Architected Framework](https://d1.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf)
- [AWS Well Architected Tool](https://aws.amazon.com/well-architected-tool/)
- [AWS SSO](https://aws.amazon.com/single-sign-on/)
- [AWS Cloud Adoption Framework](https://d1.awsstatic.com/whitepapers/aws_cloud_adoption_framework.pdf)
- [AWS Security Best Practices](https://d1.awsstatic.com/whitepapers/Security/AWS_Security_Best_Practices.pdf)
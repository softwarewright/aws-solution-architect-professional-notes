# AWS Well-Architected Framework

https://d0.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf

**The AWS Well Architected Framework is a tool used to generate constructive conversations around architectural decisions, it is not an audit mechanism.**

**When architecting workloads you make trade-offs between pillars based on your business context. Security and operational excellence should not be traded-off against the other pillars.**

**Creating a software system is a lot like constructing a building, if the foundation is not solid structural problems can undermine the integrity and function of the building.**

## Introduction

### The pillars of the AWS Well Architected Framework

- Operational Excellence - Ability to run an monitor systems to deliver business value and continually improve supporting processes and procedures
- Security - The ability to protect information, systems, and assets while deliver business value through risk assessments and mitigation strategies.
- Reliability - The ability of a system to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfigurations or transient network issues.
- Performance Efficiency - The ability to use computing resources efficiently to meet system requirements, and to maintain that efficiency as demand changes and technologies evolve.
- Cost Optimization - The ability to run systems to deliver business value at the lowest price point

### Terms

- Component - The code, configuration, and AWS Resources that together deliver against a requirement. A unit of technical ownership that is decoupled from other components.
- Workload - A set of components that together deliver business value, the level of detail that technology leaders communicate about
- Milestones - these mark key changes in your architecture as it evolves throughout the product lifecycle (design, testing, go live, and in production).
- Architecture - how components work together in a workload. How the components communicate and interact is often the focus of the architecture diagrams.
- Technology Portfolio - The collection of workloads that are required for the business to operate

### On Architecture

Prefer to distribute capabilities into teams rather than centralized teams. There are risks with this level of autonomy, but can be mitigated through best practices and experts who ensure that teams raise the bar on standards. In addition implement automated mechanisms to ensure standards are being met.

To help ensure best practices a community of principal engineers who can review and give lunch and learns to focus on best practices. These should be recorded and used for onboarding.

### General Design Principles

- Stop guessing capacity needs - Don't allow your infrastructure to  sit on expensive idle resources, take advantage of the dynamic nature of the cloud to use as much or as little capacity as you need.
- Test Systems at production scale - Use the cloud to create production scale environments on demand, complete testing, and then decommission the resources.
- Automate to make architectural experimentation easier - Use automation to create and replication systems at low cost and avoid the expense of manual effort.
- Allow for evolutionary architectures - As business and context change initial decisions might hinder the systems ability to deliver changing business requirements. Allow systems to evolve over time so the business can take advantage of innovations as a standard practice.
- Drive architectures using data - Collect data on how your architectural choices affect the behavior of your workload. Thus allowing you to make fact-based decisions on how to improve your workload. When your cloud infrastructure is code you cab use that data to inform architectural choices and improvements over time.
- Improve through game days - Test how your architecture and processes perform by regularly scheduled game days to simulate events in production. This will help understand where improvements can be made and help develop organizational experience in dealing with events.

## The Five Pillars of the Framework

### Operational Excellence

The ability to monitor systems to deliver business value and continually improve supporting processes and procedures.

#### Design Principles

- Perform operations as code - Apply the same engineering discipline that you use for application code to the entire environment. Create your entire workload as code and update it with code. Implement operations procedures as code and automate execution by triggering them in response to events. Limit the human error and maintain consistent responses to events.

- Annotate documentation - Automate the creation of annotated documentation after every build.

- Make frequent, small, reversible changes - Design to allow components to be updated regularly. Make changes in small increments that can be reversed if they fail.

- Refine operations procedures frequently - Look for opportunities to improve operation procedures. Evolve your workload as your evolve your procedures, set up regular game days to review and validate procedures are effective and teams are familiar with them.

- Anticipate failure - Perform "pre-mortem" exercises to identify potential sources of failure so that they can be removed or mitigated. Test failure scenarios and validate your understanding of their impact. Test response procedures to ensure they are effective, and teams are familiar with their execution

- Learn form all operational failures - Drive improvement through lessons learned from all operational events and failures. Share what is learned across teams and through the entire organization.

#### Best Practices

Three best practice areas for operational excellence in the cloud

*Operations teams need to understand business and customer needs to effectively support business outcomes*

**Prepare**

Effective preparation is required to drive operational excellence. Business success is enabled by shared goals and understanding across business, development, and operations.

**Design to monitor and gain insights into application, platform, and infrastructure components, as well as customer experience and behavior.**

Take advantage of CloudFormation for templatized infrastructure and CloudWatch or other various logging systems to monitor your infrastructure at every level. There is also CloudTrail as well as VPC Flow Logs.

Questions to focus on to deliver operational excellence:

1. **How do you determine what your priorities are?** Everyone needs to understand their part in enabling business success. Have shared goals in order to set priorities for resources. This will maximize benefits of your efforts.

2. **How do you design your workload so that you can understand its state?** Design the workload so that it providers the information necessary for you to understand its internal state across all components. Think metrics, logs, and traces. This allows you to provide effective responses when appropriate.

3. **How do you reduce defects, ease remediation, and improve flow into production?** Adopt approaches that improve flow of changes into production, allow refactoring, and fast feedback on quality, and bug fixing. 

4. **How do you mitigate deployment risks?** Adopt approaches that provider fast feedback on quality and enable rapid recovery from changes that do not have desired outcomes.

5. **How do you know that you are ready to support a workload?** Evaluate the operational readiness of your workload, processes, and procedures, and personnel to understand the operational risks related to your workload.

*Your goal is the minimum number of architectural standards for your workloads, balance the cost to implement the standard vs the benefit that your workload will receive. Implement operations activites as code, thus maximizing productivity and reducing error rates.*

**Operate**

*Operational success is determined by the achievement of business and customer outcomes.*

The goal is to define your expected outcomes, and determine how success will be measured, and identify the workload and operations metrics that will be used in the calculation to determine if operations are successful.

Your metrics should inform you of if you are satisfying customer and business needs, and help identify areas for improvement.

Communicate operational status of workloads through dashboards and notification tailored to the target audiences customer, business, developers, operation. Thus enabling them to take appropriate action, and their expectations are managed appropriately.

When there are unplanned events ensure that root cause analysis has been performed and communicated with affected parties.

Focus on the following questions regarding operational excellence.

6. **How do you understand the health of your workload?** Define capture and analyze workload metrics to gain visibility to workload events so that you can take appropriate action.

7. **How do you understand the health of your operations?** Define, capture, and analyze operations metrics to gain visibility to operations events so that you can take appropriate action.

8. **How do you manage workload and operations events?** Prepare and validate procedures for responding to events to minimize their disruption to your workload.

*Automate routine operations and responses to unplanned events. Avoid manual processes for deployments, release management, changes, and rollbacks. Manual processes lead to greater disruption to operations during unplanned events.*

**Evolve**

*Evolution is required for sustained operational excellence.*

Improve workloads through dedicated time and feedback loops to rapidly identify and execute changes on areas of improvement. Share lessons learned across teams to benefit the organization.

Focus on the following question:

9. **How do you evolve operations?** Dedicate time and resources for continuous incremental improvement to evolve the effectiveness and efficiency of your operations.

#### Key AWS Services

- AWS CloudFormation allow you to create templates based on best practices. Another option is terraform for creating IAC tools.

- Prepare - AWS Config and AWS Config rules can be used to create standards for workloads.

- Operate - CloudWatch allows monitoring of the operational health of your workload

- Evolve - Elasticsearch Service allows log analysis to gain actionable insights quickly and securely.

### Security

*The ability to protect information, systems, and assets while delivering business value through risk assessments and mitigation strategies.*

#### Design Principles

- Implement a strong identity foundation - Implement the principle of least privilege and enforce separation of duties with appropriate authorization for each interaction with AWS resources. Centralize privilege management and reduce or eliminate reliance on long term credentials.
- Enable traceability - Monitor, alert, and audit actions and changes to you environment in real time.
- Apply security at all layers - Protect more than just your outer layer, take advantage of all security controls. VPC, subnet, LB, instances, OS, and application.

- Automate security best practices - Automated software-based security mechanisms improve your ability to securely scale more rapidly and cost effectively. Create secure architectures, including the implementation of controls that are defined and managed as code in templates

- Protect data in transit and at rest - Classify your data into sensitivity levels and use tools like encryption, tokenization, and access control

- Keep people away from data - Create tools to reduce or eliminate the need for direct access or manual processing data. Thus reducing the risk  of loss or modification and human error when handling sensitive data.

- Prepare for security events - Prepare for incidents by having incident management process that aligns to organizational requirements.

#### Best Practices

**Identity and Access Management**

*Only authorized and authenticated users are able to access your resources, and only in the manner that you intent.*

IAM is the tool of choice in AWS that will allow you to dictate permissions and access. In addition you have the ability to force MFA authentication, and password level complexity.

Security questions to focus on:

1. **How do you manage credentials and authentication?** Protect credentials with appropriate mechanisms to help reduce risk of accidental or malicious use.

2. **How do you control human access?** Control human access by implementing controls inline with defined business requirements.

3. **How do you control programmatic access?** Control programmatic or automated access with appropriately defined limited and segregated access to help reduce the risk of unauthorized access.

**Detective Controls**

*Identify potentials security threat or incident, essential to supporting governance*

Detective controls can be implemented in AWS through the following tools:

- CloudTrail
- CloudWatch
- AWS Config
- Amazon GuardDuty

Security Questions to focus on:

4. **How do you detect and investigate security events?** Capture, analyze, and take action on security events and potential threats.

5. **How do you defend against emerging security threats?** Staying up to date with AWS and industry best practices and threat intelligence helps you stay aware of new risks. This allows you to create a threat model to identify, prioritize, and implement appropriate controls to help protect workloads.

**Infrastructure Protection**

AWS allows you to implement stateful and stateless packet inspection through AWS native technologies or through partner products and services.

6. **How do you protect your networks?** Public and private networks require multiple layers of defense to help protect from external and internal network-based threats.

7. **How do you protect your compute resources?** Compute resources in your workload require multiple layers of defense to help protect from external and internal threats. These include: EC2 instances, containers, lambda functions, etc.

**Data Protection**

*Before architecting a system, foundational practices that influence security should be in place.*

The following practices facilitate the protection of data:

- As an AWS customer you maintain full control over your data
- AWS makes it easier for you to encrypt data and manage keys, including regular key rotation
- Detailed logging that contains important content such as file access and changes
- AWS has storage systems for exceptional resiliency. Think S3
- Versioning
- AWS never initiates movement of data between regions.

Consider the following security questions:

8. **How do you classify your data?** Classification provides a way to categorize data, based on levels of sensitivity, to help determine appropriate protective retention controls.

9. **How do you protect your data at rest?** Protect your data at rest by defining your requirements and implementing controls, including encrpytion, to reduce the risk of unauthorized access or loss.

10. **How do your protect your data in transit?** Define requirements and implementing controls: encryption and reduce risk of unauthorized exposure.

**Incident Response**

*Even with the best preventive detective controls, there should still be processes in place to respond and mitigate to security incidents.*

The following practices facilitate effective incident response:

- Detailed logging is available that contains important content, such as file access and changes.
- Events can be automatically processed and trigger tools that automate responses through the use of AWS APIs.
- You can pre-provision tooling and a "clean room" using CloudFormation. Allowing your to carry out forensics in a safe isolated environment.

Think about the following security questions:

11. **How do you respond to an incident?** Preparation is critical to timely investigation and response to security incidents

#### Key AWS Services

- Identity and Access Management - IAM to control access to AWS services and resources.
- Detective Controls - CloudTrail for AWS API calls, AWS Config for detailed inventory of AWS resources and configuration. GuardDuty for managed threat detection and CloudWatch for triggered events.
- Infrastructure Protection - AWS VPC for private cloud network. CloudFront for secure content delivery with AWS Shield integration for DDoS mitigation. AWS WAF that protects CloudFront and Application LB to protect against common web exploits.
- Data Protection - Take advantage of the encryption capabilties that AWS natively supports for its services: EBS, S3, RDS, Amazon Macie, KMS
- Incident Response - IAM to grant appropriate authorization to incident response teams and tools. CloudFormation to create trusted environments. CloudWatch Events allows you to create rules that trigger automated responses.

### Reliability

*The ability of a system to recover from infrastructure of service disruptions, scale to demand, and mitigate disruptions such as misconfigurations or transient network issues.*

#### Design Principles

- Test recovery procedures - Take advantage of the cloud to validate recovery strategies, through automation you have the ability to simulate different failures or recreate scenarios.
- Automatically recover from failure - Monitor system's KPIs and use them to trigger automation to inform or automatically remediate issues.
- Scale horizontally to increase aggregate system availability - replace one large resource with multiple small resources to reduce the impact of a single failure.
- Stop guessing capacity - Through the cloud you can monitor demand and scale as necessary. Thus satisfying demand without over or under provisioning.
- Manage change in automation - Changes to infrastructure should be done through automation, meaning the only managed changes are to the automation.

#### Best Practices

**Foundations**

Before architecting a system there are foundational requirements that should be in place that ensure the reliability of the system.

- Sufficient network bandwidth
- Compute capacity

Consider the following reliability questions:

1. **How do you manage service limits?** These limits are often to prevent abuse/accidental provisioning of resources. Understand these limitations as you develop in AWS.

2. **How do ou manage your network topology?** How do your on-prem data center, public infrastructure, public cloud infrastructure, and private cloud infrastructure all communicate.

**Change Management**

*Be aware of how changes affect your system to plan proactively. Monitoring will allow you to quickly identify trends that cloud lead to capacity issues or SLA breaches.*

Reliability questions:

3. **How does your system adapt to changes in demand?** - Ensure that your system is elastic and conforms to the demand at hand.

4. **How do you monitor your resources?** Logs and metrics are a powerful tool to gain insight into the health of your workloads.

5. **How do you implement change?** Controlled changes to resources are necessary to ensure workloads are running in a manner than can be replaced in a predictable manner.

**Failure Management**

*In any system of reasonable complexity it is expected that failures will occur. Become aware of these failures, respond to them, and prevent them from happening again.*

AWS offers the opportunity of full automation in the face of an event. If a server goes down automatically stand up a new one, a region is down automatically provision resources in a new region and perform analysis on the failed resource.

6. **How do you back up data?** Back up data, applications, and operating environments to meet requirements for mean time to recovery and recovery point objectives.

7. **How does your system withstand component failures?** Architect your workloads for resilience and distribute your workloads to withstand outages.

8. **How do your test resilience?** Test the reslience of your workload to help find latent bugs that only surface in a production environment. Exercise these tests regularly.

9. **How do you plan for disaster recovery?** Disaster recovery (DR) is critical should restoration of data be required from backup methods. Your choices must align with RTO (recovery time objective) and RPO (recovery point objective) objectives.

Tracking KPIs will help you identify and mitigate single points of failure.

#### Key AWS Services

CloudWatch is an essential service to monitoring runtime metrics.

- Foundations
    - IAM to secure access to AWS services
    - VPC to provision private isolation within the cloud
    - AWS Trusted Advisor for service limits 
    - AWS Shield for managed DDoS protection
- Change Management
    - CloudTrail for monitoring AWS API calls
    - AWS Config for detailed inventory  and configuration of resources
    - Auto Scaling to automated demand management for deployed workload
    - CloudWatch alerting on metrics and logging
- Failure Management
    - CloudFormation for the creation of AWS resources
    - S3 highly durable file storage for backups
    - Glacier for durable archives
    - KMS for reliable key management

### Performance Efficiency

*The ability to use computing resources efficiently to meet system requirements, and maintain that efficiency as demand changes and technologies evolve.*

#### Design Principles

- Democratize advanced technologies - Take advantage of cloud service technologies such as RDS, CloudWatch, and EKS. Instead of managing them yourself, thus allowing your team to focus on product development rather than resource provisioning and management.
- Go global in minutes - Deploy your system in multiple regions around the world in just a few clicks.
- Use serverless architectures - These remove the need to maintain servers to carry out traditional compute activities. Thus removing the operational burden, and possibly lowering the transactional costs since the services operate at cloud scale.
- Experiment more often - With virtual and automatable resources you can quickly carry out comparative testing using different types of instances, storage, or configurations
- Mechanical sympathy - Use the technology approach that aligns best to what you are trying to achieve.

#### Best Practices

*Take a data-driven approach to selecting a high-performance architecture. Review choices on a cyclical basis. Monitor for deviance in performance and take action. Make tradeoffs in architecture to increase performance, such as compression, caching, or switching to eventual consistency.*

**Selection**

*The optimal solution for a particular system varies based on the workload.*


Consider the following question:

1. **How do you select the best performing architecture?** - Consider multiple solutions when creating your architecture and find the options that match your needs.

*There are four main resource types to consider: compute, storage, database, and network.*

Compute

Compute in AWS is available in three different forms:
- Instances - virtual servers that come in different families in sizes that can be configured to your choosing.
- Containers - operating system virtualization allowing you to run the application in resource isolated processes.
- Functions - Abstract execution environment from code that you want to execute. Thus allowing you to execute code without consideration of the underlying servers.

Consider the following questions:

2. **How do you select your compute solution?** Take advantage of the elasticity mechanisms to ensure you have sufficient capacity to sustain performance as demand changes.

Storage

Storage Questions:

3. **How do you select your storage solution?** The optimal storage solution for a system will vary based on the access method (block, file, object), patterns of access (random or sequential), required throughput, frequency of access (online, offline, archival), frequency of update (WORM, dynamic)

Database

Generally systems will choose different database solutions for it subsystems, as selecting the wrong database leads to lower performance efficiency.

Database Question(s):

4. **How do you select your database solution?** Optimal database solution of a system depends on the following requirements: availability, consistency, partition tolerance, latency, durability, scalability, and query capability.

Network

*Optimal network solutions for a particular system varies based on latency and throughput requirements.* 

AWS offers the following networking products and features to match your needs: Enhanced Networking, EBS optimized instances, S3 transfer acceleration, CloudFront, Route 53 Latency routing, VPC endpoints, Direct Connect.

Consider the following questions:

5. **How do you configure your networking solution?** Consider location and place resources close to where they will be used to reduce distance. Take advantage of regions, placement groups, and edge locations.

**Review**

Take advantage of new solutions as they arise within AWS.

6. **How do you evolve your workload to take advantage of new releases?** As new technologies become available in AWS take advantage of them to improve the performance of your workload.

Understand where your architecture is performance constrained to allow you to look out for releases that can alleviate that constraint.

**Monitoring**

*After implementing your architecture monitor its performance to remediate any issues before your customers are aware.*

The monitoring metrics you create should raise alarms and trigger automation around any performance issues.

Consider the following:

7. **How do you monitor your resources to ensure they are performing as expected?** Systems performance can degrade over time, monitor system performance to identify this degradation and remediate internal or external factors.

*The key to an effective monitoring solution is limiting false positives and ensuring that you are not overwhelmed with data.*

**Tradeoffs**

*When architecting consider tradeoffs to select an optimal approach. Trading consistency for latency to deliver for higher performance for example.*

AWS Offers Solutions such as the following: Amazon ElastiCache, CloudFront, DynamoDB + DAX.

Consider the following:

8. **How do you use tradeoffs toe improve performance?** Actively consider tradeoffs to enable optimal selection of solutions. Tradeoffs can increase the complexity of the architecture and require load testing to ensure that a measurable benefit is obtained. 

#### Key AWS Services

CloudWatch is essential to monitoring resources and systems, providing visibility into overall performance and operational health.

- Selection
    - Compute - Auto Scaling
    - Storage - EBS and its storage options, S3 for serverless content delivery
    - Database - RDS and DynamoDB
    - Network - Route 53 latency based routing, VPC endpoints, and Direct Connect
- Review - AWS Blog and what's new section of AWS website
- Monitoring - CloudWatch for metrics, alarms, and notification then based on event trigger things like lambdas.
- Tradeoffs - Amazon ElastiCache, CloudFront, Snowball, and read replicas to improve performance.

### Cost Optimization

*Ability to run systems to deliver business value at the lowest price point.*

#### Design Principles

- Adopt a consumption model - Pay only for the computing resource that you require and increase or decrease usage depending on business requirements. Stop resources that are not in use during business hours to increase potential cost savings of 75% (40 hours vs 168)
- Measure overall efficiency - measure the business output of the workload and costs associated with delivering it.
- Stop spending money on data center operations - Allow AWS to do the heavy lifting of racking, stacking, and powering servers to focus on customers and organization projects.
- Analyze and attribute expenditure - The cloud makes it easier to accurately identify usage and costs of systems, thus you can measure the ROI and give workload owners opportunity to optimized their resources and reduce costs.
- Use managed and application services to reduce the TCO -  Managed services of AWS allow you to lower the cost per transaction of managing the service your self.

#### Best Practices

**Expenditure Awareness**

*The ease of use and virtually unlimited on-demand capacity requires a new way of thinking about expenditures*

The ability to attribute resource costs to individual organization or product owners drive efficient usage behavior and help reduce helps reduce waste. The AWS Cost Explorer can help understand teh spend within an account to gain insights.

Consider the following:

1. **How do you govern usage?** Establish policies and mechanisms to ensure that appropriate costs are achieved.

2. **How do you monitor usage and cost?** Establish policies and procedures to monitor and appropriately allocate your costs. This allows you to measure and improve the cost efficiency of this workload.

3. **How do you decommission resources?** Implement change control and resource management from project inception to EOL. This ensures you shut down or terminate unused resources to reduce waste.

Take advantage of tags to determine AWS usage and costs per resources.

**Cost-Effective Resources**

*Using the appropriate instances and resources for your workload is key to cost savings.*

Through AWS using managed services can end up being cheaper then managing the server yourself for example email.

EC2 Offers Reserved instance pricing to obtain up to 75% off on-demand pricing, and with spot instances you leverage unused EC2 capacity getting up to 90% off on-demand pricing.

Consider the following:

4. **How do you evaluate cost when you select services?** When deciding on a solution consider the operational overhead associated with maintaining the resource. Choosing between EC2 managed databases and RDS for example.

5. **How do you meet cost targets when you select resource type and size?** Choose the appropriate resource size for the task, choosing the correct type and size minimizes waste.

6. **How do you use pricing models to reduce cost?** Use the pricing model that makes sense for your resources to minimize expense.

7. **How do you plan for data transfer charges?**  Ensure that you plan and monitor data transfer charges to make decisions to minimize costs.

**Matching supply and demand**

You can automatically provision resources to match demand, Auto Scaling allows you to add resources as you need them.

Consider:

8. **How do you match supply of resources with demand?** Ensure that everything you pay for is used and avoid significantly underutilized instances.

**Optimizing Over Time**

*Review your architecture decisions to ensure they continue to be the most cost-effective as requirements change.*

Managed services significantly optimize workloads and can be cheaper than running your own versions of similar services.

Consider the following:

9. **How do you evaluate new services?** As new services/features are released review existing architecture to ensure they are the most cost effective.

#### Key AWS Services

Cost Explorer helps gain visibility in insights into usages.

- Expenditure Awareness - AWS Cost Explorer to view and track usage

- Cost-Effective Resources - Reserved Instances, CloudWatch + Trusted Advisor to help right size instances. RDS to remove database licensing costs. Direct Connect and CloudFront for optimized data transfer.

- Matching supply and demand - Auto Scaling allows you to add or remove resources to match demand without overspending

- Optimizing over time - AWS Blog and Whats new section on AWS website.

## Challenge

- Use CodeDeploy to perform a rolling deploy
- Implement failover to a new region with RDS


## Resources
- Pillars
    - [AWS Operational Excellence Pillar](https://d0.awsstatic.com/whitepapers/architecture/AWS-Operational-Excellence-Pillar.pdf?ref=wellarchitected-wp)
    - [Security Pillar](https://d0.awsstatic.com/whitepapers/architecture/AWS-Security-Pillar.pdf?ref=wellarchitected-wp)
    - [Reliability Pillar](https://d0.awsstatic.com/whitepapers/architecture/AWS-Reliability-Pillar.pdf?ref=wellarchitected-wp)
    - [Performance Efficiency Pillar](https://d0.awsstatic.com/whitepapers/architecture/AWS-Performance-Efficiency-Pillar.pdf?ref=wellarchitected-wp)
    - [Cost Optimization Pillar](https://d0.awsstatic.com/whitepapers/architecture/AWS-Cost-Optimization-Pillar.pdf?ref=wellarchitected-wp)

- [AWS Well Architected Home Page](https://aws.amazon.com/architecture/well-architected)
- [AWS Well Architected Tool](https://aws.amazon.com/well-architected-tool/)
- [AWS Well Architected Labs](https://wellarchitectedlabs.com/?ref=wellarchitected-wp)
- [TOGAF Enterprise Architecture framework] (https://pubs.opengroup.org/architecture/togaf9-doc/arch/?ref=wellarchitected-wp)
- [Zachman Framework Enterprise Architecture](https://www.zachman.com/about-the-zachman-framework?ref=wellarchitected-wp)
- [Amazon Leadership Principles](https://www.aboutamazon.com/working-at-amazon/our-leadership-principles)
- [AWS DevOps](https://aws.amazon.com/devops)
- [DevOps at Amazon](https://www.youtube.com/watch?v=esEFaY0FDKc&ref=wellarchitected-wp)
- [Backup Archive and Resource AWS](http://d0.awsstatic.com/whitepapers/Backup_Archive_and_Restore_Approaches_Using_AWS.pdf?ref=wellarchitected-wp)
- [Embracing Failure: Fault-Injection and Service Reliability](https://www.youtube.com/watch?v=wrY7XoOnysg&ref=wellarchitected-wp)
- [AWS Disaster Recovery](http://media.amazonwebservices.com/AWS_Disaster_Recovery.pdf?ref=wellarchitected-wp)

- [EC2 Deep Dive](https://www.youtube.com/watch?v=mZy6E2I5Rek&ref=wellarchitected-wp)
- [Cost Optimization on AWS](https://www.youtube.com/watch?v=XQFweGjK_-o&ref=wellarchitected-wp)
- [TCO Calculator](http://aws.amazon.com/tco-calculator?ref=wellarchitected-wp)
- [Analyzing Costs with Cost Explorer](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-explorer-what-is.html?ref=wellarchitected-wp)
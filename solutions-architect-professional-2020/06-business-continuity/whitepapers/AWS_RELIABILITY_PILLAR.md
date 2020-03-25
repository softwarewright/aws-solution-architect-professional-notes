# Reliability Pillar

*System Reliability, the ability of a system to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, mitigate disruptions such as misconfigurations or transient network issues.*

## Introduction

### Design Principles

- Test recovery procedures - Test how your system fails to validate recovery works as expected, before a real failure scenario.
- Automatically recover from failure - Monitor system KPIs to trigger automated recovery processes
- Scale horizontally to increase aggregate system availability - replace large systems with multiple small systems to reduce single failure impact.
- Stop guessing capacity - Monitor systems and automate the addition or removal of resources
- Manage change in automation - changes to infrastructure should be automated. Change that need to be managed are changes to the automation.

### Definition

*Services availability is defined as the percentage of time that the application is operating normally.*

Common application availability design goals:

- 99% / 3 days 15 Hours / Batch processing, data extration, transfer, and load jobs
- 99.9% / 8 hours 45 minutes / Interal tools like KB, people tracking
- 99.95 / 4 Hours 22 minutes / Online commerce, point of sale
- 99.99% / 52 minutes / Video Delivery, broadcast systems
- 99.999% / 5 minutes / ATM transactions, telecommunications systems

*Hard dependencies* - dependencies where an interruption in the service translates to an interruption in the invoking system. 

*Redundant components* - independent components that are redundant and improve the resiliency of the system. For example Availability Zones.

*Costs for availability* - higher availability comes with higher costs therefore ensure that you identify true availability needs.

## Foundation - Limit Management

Understand the limitations of the system that you operate in: physical (rate that you can push bits down a optic cable), services limits, rate limiting and hard limits.

*In AWS limits are per-region per-account so plan accordingly*

Tools to understand limits:
- AWS Trusted Advisor
- Service Limits Documentation
- Service Quotas

The Tracking of limits should be an automated process.

### Key Services

- AWS CloudWatch/Logs - Set alarms and search and extract patterns in a log event.

## Foundation - Networking

AWS VPC considerations

- Allow IP address space for more than one VPC per Region
- Consider cross account connections, separate accounts should be able to connect back to shared services
- Within a VPC allow space for multiple subnets that span multiple AZs
- Always leave unused CIDR block space within a VPC

Next consider...

- How are your going to be resilient to failures in your topology
- What happens if you misconfigure something and remove connectivity?
- Will you be able to handle unexpected increase in traffic/use of your services
- Will you be able to absorb a DoS attack?

When operating in AWS consider...
- How many VPCs do you plan to use?
- Will you use VPC peering?
- Will you connect VPNs to the VPC?
- Will you use Direct Connect or the internet

General Things to Remember

- Ensure there are no overlapping IP addresses in the range you select.
- VPC CIDR blocks lean toward to large rather than too small. (AWS recommends /16, but you can get up to /24)

### Key Services

- AWS Direct Connect
- EC2
- Route 53
- Global Accelerator - Network layer service used to create accelerators to direct traffic to optimal endpoints over the AWS global network
- Elastic Load Balancing 
- AWS Shield - Automatic protection against DDoS attacks at no extra cost


## Application Design for HA

Consider your use case before deciding the availability that you would like to maintain. While 5 nines (99.999%) sounds like amazing availability to maintain for a service, it can be overkill depending on the business needs since it only allows for 5 minutes of downtime per year. 

When building an application in AWS consider the following questions to avoid considerable cost:

- What problem are you trying to solve?
- What specific aspects of the application require specific levels of availability?
- What amount of cumulative downtime can this workload realistically accumulate in a year?
- In essence, what is the real impact of the system being unavailable?

Customer considerations:

- Do you have customers at all hours that will not come back to conduct business another time if you have an interruption
- Can you use a lower level of availability with a fallback mechanism to handle failures. For example if the thermostat cannot connect to the internet there is still a physical override.

*At higher levels of availability ensure the detection and response to interruption are fully automated.*

Common Sources of Failures

- Hardware - Failure in hosts, storage, network, etc
- Deployment 
- Load induced
- Data induced - input accepted by the system that it cannot process ("poison pill")
- Credential expiration - certificate or credential expiration
- Dependency
- Infrastructure - Power supply or environment condition failure has an impact on hardware availability
- Identifier  exhaustion - Throttling limit, id ran out, resource vended to customers is no longer available.

*If you plan to achieve 99.999% availability means master all of the sources of interruption listed here and automating all human intervention out of operational processes.*

Mastery of:

- Deploying and maintaining canaries
- Frequently testing automated fail-over testing 
- unit and workflow/transaction monitoring of both success and failure 
- Alarms and log analysis
- Auto-rollback and automatic system recovery capabilities including every dependent service, network connection and piece of infrastructure between you and your customer.

After considering the above refine the definition and prioritization of requirements

- What are the most valuable transactions to your customers and your business?
- Can you predict when certain geographies have the greatest demand?
- What times of day, week, month, quarter, or year are your peak times? 

### Understanding Availability Needs

*Applications generally have different availability requirements depending on the components within the application. For example there are applications that prioritize storing data ahead of retrieving data, others prioritize high availability during certain hours of the day, but tolerate longer periods of disruption outside of these hours.*

### Application Design for Availability 

*Five common practices to apply to improve availability:*

- Fault Isolation Zones
- Redundant components
- Micro-service architecture
- Recovery oriented computing
- Distributed systems best practices

#### Fault Isolated Zones

An example is using multiple AZs. Since AZs are separate physical locations that miles apart they avoid environment hazards, but are close enough to allow data replication without undue impact on application latency.

Regions are meant to be dedicated copies of services deployed in each geographical region. The Regional AWS services use multiple AZ internally. With AWS any cross region services maintain the autonomy of the region, exceptions being CloudFront and Route53.

#### Redundant Components

*One of the principles for service design in AWS is the avoidance of single points of failure in underlying physical infrastructure. Hence availability zones and systems that avoid the failure of a single node causing complete system failure.*

#### Microservice Architecture

*Micro-services are smaller and simpler components that allow you to differentiate availability required of different services.* Consider Amazon.com, there are literally hundreds of microservices used to build the page, yet there are only a few needed to be available for a user to be able to make purchase. If the additional services are not available then that section of the page simply does not render.

Each microservice establishes a contract with calling applications:

- Concise solution to a business problem to be served and a small team that owns the business problem. Thus allowing better organizational scaling.
- The team can deploy at any time as long as they meet their API and other "contract" requirements
- The team can use any tech stack as long as they meet their API and other "contract" requirements

#### Recovery-Oriented Computing

*Recovery-Oriented Computing is the term applied to systematic approaches to improving recovery.* This directly impacts the availability of a system as impact duration is a primary input to calculating availability.

Characteristics of systems that enhance recovery are:

- Isolation and redundancy
- System wide ability to rollback changes
- Ability to monitor and determine health
- Ability to provide diagnostics
- Automated recovery
- Modular design
- Ability to restart

There are many types of failures that occur in systems, rather than construct novel mechanisms to trap, identify, and correct each failure focus on having the right mechanisms to detect failures (ELB or Route53). After the failure apply one of a small number of well-tested recovery paths.

In ROC many of the different categories of failures are mapped to the same recovery strategy. Retrying requests + Exponential Back off for network or dependency failure, Auto Scaling Groups for hardware failures rather than building customer remediation for each treat issues like an instance failure.


*The best practice is to have a small number of recovery paths to ensure that at all times they are well tested frequently to avoid unexpected fail over issues.*

#### Distributed Systems Best Practices

There are a few best practices to ensure that distributed systems continue to operate normally in the presence of "normal issues":

- Throttling - Responds to unexpected increase in demand on a web service. For the requests that are not honored they will be returned with a message indicating they have been throttled. This carries the expectation they will try again at a slower rate. Ensure that load testing has been performed to ensure the nodes in your service can handle the request load. API Gateway provides throttling methods built into the service.

- Retry with exponential fallback - The invoking piece of the throttling pattern, AWS SDKs implement this by default and this can be configured. The pattern pauses and then reties at a later time, each time pausing for longer durations.

- Fail fast - Return an error as soon as possible to release resources associated with requests. This is better than creating queues to handle request that can impede your ability to quickly recover.

- Use of idempotency tokens - Completing an action once in a distributed system is easy, but ensuring that the action has been only performed once is complicated. Idempotency tokens in APIs are used whenever the request is repeated to figure out what work has already been completed.

- Constant work - Since systems can fail under rapid changes to load you can add "filler" work to ensure that the capacity hovers around the necessary capacity keeping the system healthy.

- Circuit Breaker - There are situations where a service needs to make request to another service, but does not want to make a hard dependency. In these cases the circuit breaker pattern is helpful, when the external system starts to exhibit high latency/issues the circuit breaker "opens" and returns locally cached data. Periodically the system will attempt to call the dependency to determine if it has recovered, and will close the circuit breaker when that occurs.

- Bi-modal behavior and static stability - Cascading failures across a system can cause failures across the entire distributed system. Build systems that are statically stable and operate in only one mode. They maintain enough internal state to continue operating as they were before the failure without adding additional load to the system.

### Operational Considerations for Availability

When designing software systems plan for automation and human processes as a part of the application lifecycle. Deployment of new versions, operation of the service, infrastructure refreshes, and replacing failure infrastructure.

Testing is important to ensuring the quality of your system, more than unit and functional tests:

- Load Testing
- Fault Injection Testing
- SLA auditing 

### Automated Deployments to Eliminate Impact

Different deployments that can mitigate the risk of deploying to a production environment:

- Canary deployment - directing a small amount of traffic to the new functionality monitoring the entire session, if there is an issue the feature is removed from the canary release.
- Blue-Green Deployment - A full fleet of the application is deployed and you alternate between two stacks. you can send traffic to the new version and fall back if you see problems with the deployment.
- Feature toggles - Allow you to deploy features turned off, so that customers cannot see them. Then you turn the feature on when you are ready. AWS rule of thumb is to avoid touching multiple AZ within a region at the same time ensuring availability.
- Failure isolation zone deployments 

#### Testing

Testing should go hand-and-hand with your availability goals, test for the application resiliency as well as transient failures of dependencies durations that may last from lest than a second to hours. Testing is the only way that you ensure that you can achieve your goals. Investing in the ability to inject failures into your system works well for understanding what will happen ot your system as issues arise.

### Monitoring and Alarming

*Monitoring is essential to ensuring that you are reaching your availability requirements.* The worst possible failure is one that your customer discovers before you. Decouple your alerting from your services to ensure that if there is an issue with your services you still have the ability to be alerted.

Consider using percentile monitoring of requests to understand issues that are arising before they have the opportunity.

Monitoring at AWS consists of five distinct phases:

1. Generation
2. Aggregation
3. Real-time processing and alarming
4. Storage
5. Analytics

#### Generation

Determine services the need monitoring and define important metrics and how to extract them from log entries. Then create thresholds and corresponding alarm events.

#### Aggregation

Aggregate logs into a single source like CloudWatch or S3.

#### Real-time processing   

Alerts triggered can cause reactions within your system such as auto scaling sns email notification, lambda execution etc.

#### Storage and Analytics

CloudWatch Logs allow data to flow into Amazon S3 which can be processed by services such as EMR to gain insights. S3 Select and Amazon Athena can be used to query the data in the CloudWatch Logs.

## Challenges

- How do you configure the exponential back off and the AWS SDK services?
- How do you implement an idempotency token in a distributed system?
- When does it make sense to use *constant work* pattern
- Implement the circuit breaker pattern.
- Implement a canary deployment
- Implement a blue/green deployment
- How can you inject failures into a system?
- Implement [percentile monitoring](https://bravenewgeek.com/everything-you-know-about-latency-is-wrong/)
- Investigate [S3 Select](https://aws.amazon.com/about-aws/whats-new/2018/04/amazon-s3-select-is-now-generally-available/)
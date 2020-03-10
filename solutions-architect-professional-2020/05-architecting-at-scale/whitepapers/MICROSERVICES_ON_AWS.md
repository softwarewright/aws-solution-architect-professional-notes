# Microservices On AWS

https://d1.awsstatic.com/whitepapers/microservices-on-aws.pdf

Microservice architectures are a combination of various successful and proven concepts:

- Agile software development
- Service-oriented architectures
- API-first design
- Continuous Integration/Continuous Delivery (CI/CD)

Serverless Microservices offer the following tenents:

- No infrastructure to provision or manage
- Automatic scaling
- Pay for value billing model
- Built-in availability


## Simple Microservices Architecture on AWS

### UI

Static content is served behind CloudFront with a backing S3 bucket.

### Microservices

The popular approaches for deploying microservices are AWS Lambda and AWS Fargate.

Lambda offers the ability to load your code and everything else is taken care of for you.

Services like ECS and EKS offer the ability to use containers to host your services on AWS

Fargate allows you to run serverless containers to prevent worrying about provisioning, configuring, and scaling clusters of vms.

Use ECR to store the containers.

### Data Store

Using caches between databases and services is a common pattern for reducing read load on the database. Options like Memcache or Redis work well.

Relational Databases work well for structural data, AWS RDS offers managed database services. The issue with Relational databases they are not designed to endlessly scale, as a result you will need to apply techniques to support large numbers of queries.

NoSQL databases are designed to favor scalability, performance, and availability over consistency of relational databases. AWS offers DynamoDB which has millisecond performance, and if you need microsecond response time DAX is available.

There is an on-demand option with DynamoDB that you can pay per request and allows you to serve thousands of requests per second instantly without planning capacity.

### API Implementation

API Gateway can be the front door to your web applications. Enabling caching, authentication, rate limiting, and so much more.

### Serverless Microservices

Options for Serverless
- Amazon Aurora Serverless
- DynamoDB
- Lambda
- API Gateway
- CloudFront/S3

## Distributed System Components

### Service Discovery

ECS includes integrated service discovery making it easy for containerized service to discover and connect.

EKS uses Kubernetes allows for service discovery.

AWS Cloud Map provides a registry for resources with an API-based service discovery mechanism. Existing Route 53 auto naming resources are upgraded automatically to AWS Cloud Map.

### Service Meshes

Generally the complex part about services is the communication between them.

A Service Mesh consists of a data plane and a control plane. The data plane  consists of proxies that are deployed with the application code as a special sidecar that intercepts all network communication between microservices. The control plane is responsible for communication with the proxies.

**Service meshes are transparent**, meaning that the application code is not aware of the mesh.

AWS App Mesh works with: AWS Fargate, ECS, EKS, and self managed Kubernetes. App Mesh can monitor and control communications for microservices running across clusters, orchestration systems, or VPCs.

## Distributed Data Management

With Distributed Data Management means trading consistency for performance and thus you need to embrace the need for eventual consistency.

Since ACID transactions are not available within a distribute data management architecture a commonly used pattern is the Saga pattern. When there is a failed business transaction we undo the changes that were made.

Since it's common for state changes to affect more than a single micro service, event sourcing is useful for representing and persisting all every application change as an event record. Therefore instead of persisting the application state, data is stored as a stream of events.

Database transaction logging and version control systems are two well know examples of event sourcing.

Event sourcing has the following benefits: state can be determined and reconstructed for any point in time. It naturally produces a persistent audit trail and also facilitates debugging.

Event sourcing is frequently used with conjunction with CQRS to decouple read from write workloads

### Async Communication and Lightweight messaging

#### REST-based communication

HTTP/S protocol is the most popular way to implement synchronous communication between microservices. This relies on stateless communication between services.

#### Async Messaging and Event Passing
Communication via message passing is possible be communicating through queues.

When communicating through this style you no longer need service discovery since services are loosely coupled via the queue.

Possible implementations include SQS and SNS.

Amazon MQ allows you to use ActiveMQ if you are not ready to migrate to SQS.

#### Orchestration and State Management

Take advantages of Lambda Step functions rather than implementing your own workflows.

Step Functions automatically triggers and tracks each step and retries when there are errors. Thus the application executes in order as expected.

Step Functions also can integrate with compute resources such as:

Push:
- ECS
- SageMaker
- Glue
- DynamoDB
- SQS
- SNS
- Fargate
- Batch

Poll:

- EKS
- EC2
- ECS
- AWS Fargate
- On-Prem

### Distributed Monitoring

CloudWatch works well for centralized logging, alarms and metrics tracking.

#### Monitoring

CloudWatch is great for simple monitoring at scale, in addition there is support for custom metrics.

For services such as EKS, [Prometheus](https://github.com/coreos/kube-prometheus) and [Grafana](https://grafana.com/) 
to collect and visualize collected metrics.

#### Centralizing Logs

Most services centralize logs by default, generally through Amazon S3 and AWS CloudWatch.

EC2 Supports a daemon to send log files to CloudWatch Logs, ECS supports the awslogs driver to send logs to CloudWatch, EKS uses FluentD to forward logs into CloudWatch where it can be combined for higher level reporting using Elasticsearch and Kibana.

#### Distributed Tracing

AWS X-Ray allows you to trace requests across your microservice architecture using correlation ids. It works with EC2, ECS, Elastic Beanstalk, and Lambda.

#### Options for Log Analysis on AWS

Amazon CloudWatch Logs Insights works well for visualizing logs instantly. Amazon ElasticSearch + Kibana works well too.

CloudWatch Logs can be streamed into Amazon ES near real time through CloudWatch subscription.

Amazon ES can be used for full-text search, structured search, analytics,  and all combination. Kibana is open source data visualization for ES that seamlessly integrates.

Using Redshift together with QuickSight to analyze log files.

CloudWatch can act like a centralized log store which can be streamed through Kinesis Data Firehose.

### Chattiness

When breaking down microservice the communication overhead increases, depending on your domain consolidation of services can be a great option.

#### Protocols

HTTP protocols are simple and common. Messages exchanged by services cna be encoded in different ways such as JSON and YAML. There are also efficient binary formats such as Avro or Protocol Buffers.

#### Caching

Services like ElasticCache help reduce the latency and chattiness of microservices. API Gateway contains a built in caching layer. The goal is to find a right balance between a good hit rate and timeliness/consistency of data.

### Auditing

When dealing with potentially hundreds of distributed services you need visibility of user actions.

#### Audit Trail

AWS CloudTrail tracks changes in microservices by monitoring API calls made in the Cloud. Trails can be aggregated into a single S3 bucket.

Storing audit trails CloudWatch are captured in real-time

#### Events and Real-Time Actions

CloudTrail with CloudWatch Events allows custom events and notification across your organization.

#### Resource Inventory and Change Management

AWS Config rules allow you to define security policies with specific rules to detect, track, and alert you to policy violations.

## Challenges

- Implement the Saga Pattern with AWS Step Functions
- Implement an event store with Kinesis Data Firehose and Amazon S3
- Implement Fan Out Pattern with SQS and SNS
- Investigate the following patterns
    - Event Sourcing
    - Event Stores
    - CQRS
    - Sagas
- Investigate the following technologies
    - https://aws.amazon.com/app-mesh/
    - https://aws.amazon.com/cloud-map/
    - https://aws.amazon.com/step-functions/


## Resources

- [Twelve-Factor App](https://12factor.net/)
- [Elastalert](https://github.com/Yelp/elastalert)
- [Grafana](https://grafana.com/)
- [Prometheus](https://prometheus.io/docs/introduction/overview/)
- [Kubernetes Prometheus](https://github.com/coreos/kube-prometheus)
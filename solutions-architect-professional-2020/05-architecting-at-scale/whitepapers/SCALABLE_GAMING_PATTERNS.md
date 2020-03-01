# Introduction to Scalable Gaming Patterns on AWS

## Game Design Decisions

Many games tend to have the following features:

- Leaderboards and rankings
- Free to play
- Analytics around the game
- Content updates
- Async gameplay - real-time only multiplayer and competing against friends
- Push notifications
- Unpredictable clients since there is a variety of platforms

## Game Client Considerations

- All network calls are async and non-blocking
- Use JSON to transport data, it's compact and cross platform. The exception being UDP packets in  multiplayer.
- Use HTTP/1.1 with keepalives and reuse http connections between requests, as each connection can take 50 ms for the TCP handshake
- Use SSL for all requests
- Never store security critical data on the client device, if you need the client to access AWS use Amazon Cognito
- Never trust what a game client sends as it's an untrusted source

### Handling the Backend

Services that work well include:

- Elasticbeanstalk - for deploying the server, auto scaling, and allowing [Blue/Green Deployments](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.CNAMESwap.html). Don't manage RDS with your Elasticbeanstalk instance in production.
- RDS - for the backend database
- S3 - hosting static content and binary game data. Be sure the bucket is in the same region as your servers.

### HA, Scalability, and Security

Tips:

- Two AZs for a balance of HA and cost in production
- Single AZ for dev and test environments. Unless you require HA in non-prod
- Use a Load Balancer

### Binary Game Data with Amazon S3

Since S3 Buckets are private by default for users to download content you have two options. Make the bucket public, or the better way, create pre-signed urls using the AWS SDK. Creating the presigned url is a completely offline operation.

Use CloudFront as the game grows to provider better performance.

### Expanding Beyond AWS Elastic Beanstalk

You have the ability to use other AWS services in tandem with Elastic Beanstalk.

- ElastiCache - for caching
- DynamoDB, RDS, EC2 hosted DB - for data management

### ELB

Tips:

- Configure to balance between at least 2 AZs
- Enable SSL
- Use the DNS CNAME since IP address will change when the ELB scales
- ELB scale up roughly 50% every 5mins which usually works, but if you need more pre-warm the ELB through an AWS support request

### ALB

- Support for Amazon ECS
- HTTP/2
- IPv6 support
- WebSockets support

### Custom LB

If interested in HAProxy as a managed service consider AWS OpsWorks

### Auto Scaling

Tips:

- CPUUtilization is a good metric for auto scaling, if paired with NetworkIn or NetworkOut you can get more granularity
- Benchmark servers to determine good scaling values. Apache Bench or HTTPerf to measure response times. Take note of when the application degrades

Consider OpsWorks for bootstraping EC2 instances when you are not using Elastic Beanstalk.

## Challenges

- S3
    - Generate a Presigned Url
- [AWS OpsWorks](https://aws.amazon.com/opsworks/)

## Resources

- [Apache Bench](http://httpd.apache.org/docs/2.2/programs/ab.html)
- [loadtest](https://www.npmjs.com/package/loadtest)
- [Artillery](https://artillery.io/)
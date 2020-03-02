# Automating Elasticity

https://d1.awsstatic.com/whitepapers/cost-optimization-automating-elasticity.pdf

## Monitoring AWS Service Usage and Costs

Tools:
- [Cost Optimization Monitor](https://aws.amazon.com/answers/account-management/cost-optimization-monitor/)
    - [Detailed](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/detailed-billing-reports.html)
- [Cost Explorer](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-explorer-what-is.html)

### Tagging Resources

Tag resources to gain visibility and control over IT costs. Categorize resources by owner, purpose, or environment.

An example of cost savings is to turn off development resources after hours.

### Automating Elasticity

Companies that shut down EC2 instances outside of a 10/hour workday save 70% compared to running them 24 hours a day.

When setting up automation remember to give systems minimum level of access required to perform necessary tasks.

#### Automating Time-Based Elasticity

The AWS Instance Scheduler is a simple solution that allows you to create automatic start and stop schedules for your EC2 instances. This can be deployed using AWS CloudFormation and can perform across the entire account.

You can also use the AWS CLI/SDK or Lambdas to shut down instances.

#### Data Pipeline

allows you to reliably process and move data between AWS compute and storage services at specified intervals.

#### CloudWatch

CloudWatch can be used to stop or terminate EC2 instances that are unused or underutilized. Another things that you can do is send an email if instances have been underutilized for 8 hours.

#### Automating Volume-Based Elasticity

The best tool is EC2 Auto Scaling to increase the number of instances during demand spikes and decreasing during lulls to control costs.


## Challenges

- Use the [AWS Instance Scheduler](https://aws.amazon.com/solutions/instance-scheduler/) to stop and start instances
- Use [CloudWatch](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/UsingAlarmActions.html) to terminate underutlized EC2 Instances 
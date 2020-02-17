# AWS Migration Whitepaper

Figure out your organization's concerns up front.

- How do I build the right business case?
- How do I accurately assess my environment?
- How do I learn what I don’t know about my enterprise network topology
and application portfolio?
- How do I create a migration plan?
- How do I identify and evaluate the right partners to help me?
- How do I estimate the cost of a large transition like this?
- How long will the migration process take to complete?
- What tools will I need to complete the migration?
- How do I handle my legacy applications?
- How do I accelerate the migration effort to realize the business and
technology benefits?

## AWS Cloud Adoption Framework (AWS CAF)

### Business

Business support capabilities to optimize business value with cloud adoption.

Common Roles: Business Managers; Finance Managers; Budget Owners; Strategy Stakeholders

### People

People development, training, communications, and change management.

Common Roles: HR; Staffing; People Managers.

### Governance

Managing and measuring resulting business outcomes.

Common Roles: CIO; Program Managers; Project Managers; Enterprise Architects; Business Analysts; Portfolio Managers.

### Platform

Develop, maintain, and optimize cloud platform solutions and services.

Common Roles: CTO; IT Managers; Solution Architects.

### Security

Designs and allows that the workloads deployed or developed in the cloud align to the org's security control, resiliency, and compliance requirements.

Common Roles: CISO; IT Security Managers; IT Security Analysts; Head of Audi and Compliance.

### Operations

Allows system health and reliability through the move to the cloud, and delivers an agile cloud computing operation.

Common Roles: IT Operations Managers; IT Support Managers.

## Motivating Change

[AWS OCM Framework](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-ocm/migration-ocm.pdf)

Implement an Organizational Change Management framework to drive desired changes throughout the organization into cloud adoption. 

The only way that your organization successfully moves to the cloud is through a joined effort of the entire organization. The speed at which you get there depends on your ability to deliver the message of cloud adoption to your people.

## Business Drivers

People move to the cloud for the agility that it provides. Moving from months to provision services on-prem to minutes in the cloud.

Reason for moving to the cloud include:

- Operational Costs - Operational costs of running infrastructure including: unit price of infrastructure, matching supply and demand, investment risk for new applications, markets, and ventures.
- Workforce Productivity - Workforce productivity is how efficiently you are able to get your services to market. Through AWS provisioning you are able to deploy resources quickly and focus on what makes your product stand out.
- Cost avoidance - Eliminate the need for hardware refreshes and maintenance programs
- Operational resilience - reducing risk profile and cost of risk mitigation. Improving resiliency and uptime through HA practices and multiple regions.
- Business Agility - Ability to react quickly to changing market conditions. Allowing you to expand into new markets. take products to market quickly, and acquire assets that offer a competitive advantage. Operational speed through: standardization, flexibility in development, automation, monitoring, auto-recovery, and HA capabilities.


## Migration Strategies

When developing a migration strategy consider the 6 R's. These will help outline a migration plan for each of the application in your business' portfolio. This plan will be iterated on and mature as you progress through the migration.

### Re-host (lift and shift)

Move the application without changes. In large scale, legacy migrations, organizations are looking to move quickly to meet business objects. Therefore majority of application are re-hosted.

Most re-hosting can be automated with tools like AWS VM Import/Export. Though some people will prefer to manually make the shift into AWS to learn how to apply legacy systems to a new cloud platform.

Once the applications are in the cloud they are easier to optimize/re-architect.

### Re-platform (lift, tinker, and shift)

Make a few cloud optimizations to achieve a tangible benefit. Though you don't change the core architecture of the application. An example is using RDS to manage your database rather than maintaining the instance yourself. Another example is using Elastic Beanstalk.

This can mean saving lots of money from licensing costs.

### Re-factor/Re-architect

This is the most expensive, but can have the highest ROI if you have a product that fits. You completely re-imagine what the application's architecture looks like in the cloud.

For example moving from monolithic architecture to service oriented, serverless, or etc.

### Re-purchase

Think moving from a CRM to salesforce, and HR system to Workday, or a CMS to Drupal.

### Retire

Removing application that are no longer needed, figure out who owns each of the applications and see if they can be turned off.

### Retain

Keep the application that are critical for the business, but need major refactor before moving. Revisit all applications that fall into this category at a later point in time.

### Which Strategy is Right for Me?

Consider why you are moving to the cloud, what are your business drivers?

- Migrating for cost avoidance and to avoid hardware refresh? Re-platform.
- Data center contract ending in 12 months? Re-host then Re-factor later.

Consider a phased approach to migrating applications, prioritizing business functionality in the first phase, rather than attempting it all in a single step. Then in the next phase take advantage of the AWS platform for cost, performance, productivity, and compliance advantages.

The goal is to mitigate risk and focus on delivering value by migrating it to the cloud.

The migration strategy should allow teams to move quickly and independently. The strategy should address the following questions:

- Is there time sensitivity to the business case/driver. Such as expiring licenses.
- Who will operate your AWS environment and your applications?
- What standards are critical to impose on all applications that you migrate.
- What automation requirements will you impose on applications as a starting point for cloud operations, flexibility, and speed? Will this be for all apps or a subset? How will these standards be imposed?

## Building a Business Case for Migration

Migration business case has four categories:

1. Run cost analysis - Total Cost of ownership in comparison of run costs on AWS
2. Cost of change - migration planning/consulting costs
3. Labor productivity - Estimate reduction in number of hours spent conducting legacy operational activities 
4. Business Value - Agility, cost avoidance, risk mitigation, decommissioned asset reductions

The business cases for migration address the following questions:

- What is the future expected IT cost on AWS vs Existing base cost
- Estimated migration investment costs
- What is expected ROI, and when will the project be cash flow positive?
- What are business benefits beyond cost savings?
- How will using AWS improve your ability to respond to business?

### Cost/Value Categories

Run Cost Analysis
- TCO vs current operating model
- AWS purchasing options RI and volume discounts
- Impacts of AWS Discounts: Enterprise Discount Program, service credits, Migration Acceleration Program incentives

Cost of Change
- Migration planning/consulting costs
- Compelling events: referesh, data center renewal
- Application migration estimates and parallel environment costs

Labor Productivity
- Automation gains
- Developer productivity

Business Value
- Agility (time to deploy, scaling, etc)
- Cost avoidance
- Risk mitigation
- Decommissioned asset reduction

### Drafting the business case

- Directional - get a rough estimate of assets and get early buy in
- Refined - through additional data of the scope of the migration and workloads
- Details - Perform deep discovery of the on-prem environment and server utilization. Use automated discovery tools for deep discovery

When building the case consider the following:

- Right size mapping to run the existing applications and processes on AWS. Understand what is needed vs what is currently provisioned. Watch for over provisioning.
- Extend the right size mapping to consider resources not required full time. i.e. turn off development and test servers when not in use to reduce costs.
- Identify early candidates for migration to establish migration processes and develop experience in the migration readiness and planning phase.

Early analysis of application discovery data helps determine: run rate costs, migration cost, resource requirements, and timelines for the migration.

## People and Organization

**It's important to develop a critical mass of people with production AWS experience if you are preparing for a large migration.**

Drive change throughout the organization of best practices, governance standards and automation. Consider creating a Cloud Center of Excellence CCoE.

### Organizing Your Company's Cloud Teams

Effective CCoE teams evolves over time, size, makeup, function and purpose.

1. Start small with an informal group connected by a shared interest **experimenting with cloud implementation.**
2. Establish the CCoE dedicated to evangelizing the value of the cloud: establising best practices, methods, and governance
3. The small teams then migrate candidate applications and groupings, commonly referred to as migration waves to the cloud.
4. As lessons are learned ensure
    - Documentation
    - Improving efficiency
    - and confidence

### Creating a CCoE

- The CCoE structure will evolve and change as the org transforms. Diverse, cross functional representation is key.
- Treat the cloud as your product adn the application team leaders as your customers. Drive enablement, not command and control
- Build company culture into everything you do
- Organizational change management is central to business transformation. Use intentional and target organizational change management to change culture norms 
- Embrace a change-as-normal mindset. Change of applications, IT systems, and business direction is expected.
- Operations model decisions will determine how people fill roles that achieve

#### Structure CCoE for migration at Scale

Include people for across the impacted business segments, with cross-functional skills and experiences.

- Achieve buy-in
- Earn Trust across the org
- Establish effective guidelines that balance business requirements

There are two groups to the CCoE

- The cloud business office - ensures cloud services meet the needs of your internal customer, business services.
- Cloud Engineering

## Tools

- [AWS Monthly Calculator](https://calculator.s3.amazonaws.com/index.html)
- [AWS TCO Calculator](https://aws.amazon.com/tco-calculator/)
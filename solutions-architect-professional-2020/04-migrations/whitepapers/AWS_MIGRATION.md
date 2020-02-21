# AWS Migration Whitepaper

https://d1.awsstatic.com/whitepapers/Migration/aws-migration-whitepaper.pdf

**By the end of this your goal should go from when do we start the migration to lets start the migration now.**

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
- Cloud Engineering - Owns infrastructure automation, operational tools, processes, security tooling and tools, and migration landing zones. They optimize the speed at which a business unit can access cloud resources and optimize the use patterns.

## Migration Readiness and Planning

Tools, processes and best practices to prepare an enterprise for cloud migration.

### Assessing Migration Readiness

Things to consider

- Is there clearly defined scope and the business case for migration
- Have you evaluated the environment and application in scope through the lenses of the AWS CAF
- Is your VPC secure and can it act as a landing zone for all applications in scope
- Have your operations and employees skills been reviewed and updated to accommodate the change
- Do you have the experience necessary to move tech stacks that are in scope.

### Application Discovery

- Use an automated discovery tool
- Plan how to keep data current by continuously running your automated discovery tool
- Do an initial application discovery during business case development to accurately reflect the scope

### Discovery Tools 

AWS Application Discovery Service

An application discovery tool can:
- Automatically discover the inventory of infrastructure and application running in your data center and maintain the inventory
- Help determine how applications are dependent on each other or on underlying infrastructure
- Inventory versions of operating systems and services for analysis and planning
- Measure applications and processes running on hosts to determine performance baselines and optimization opportunities
- Provider a means to categorize applications and servers and describe them in a way that's meaningful to the people who will be involved in the migration project

### Application Portfolio Analysis

The next step of grouping found applications based on patterns. The goal is data driven insights based on app discovery. You are looking for high level analysis across your applications.


### Migration Planning 

The goal is to manage scope, schedule, resource plan, issues and risks, coordination, and communication to all stakeholders. It's recommended to use agile practices to deliver on migration.

Recommended migration plan activities:

- review project management methods, tools, and capabilities to assess any gaps
- Define project management methods and tools to be used during the migration
- Define and create teh migration project charter/communication plan, including reporting and escalation procedures
- Develop a project plan, risk/mitigation log, and roles and responsibilities matrix to manage the risks that occur during the project and identify 
- Procure and deploy project management tools to support delivery of the project
- Identify key resources and leads for each of the migration work streams defined in this section
- Facilitate the coordination and activities outlined plan
- Outline resources, timelines and cost to migrate the targeted environment to AWS

### Technical Planning

Utilize a small team (enterprise architecture) to prioritize the portfolio that you have developed. It's recommended to use agile practices, do deep analysis of the first couple of apps and then begin migration. Then move to deep analysis of the next apps while the first are being migrated. 

### VPC Environment

#### Security

Security Perspective:

Core security themes

1. Identity and Access Management
2. Logging and Monitoring
3. Infrastructure Security
4. Data Protection
5. Incident Response

Augmenting Security Themes

1. Resilience
2. Compliance Validation
3. Secure CI/CD
4. Configuration and Vulnerability analysis
5. Security Big Data Analysis

### Platform

Key elements of the platform work stream:

AWS landing zone - providers initial structure and pre-defined configurations for AWS accounts, networks, identity and billing frameworks, and customer selectable optional packages

Account Structure - defines an initial multi account structure and pre-configured baseline security that can be easily adopted into your org model

Network Structure - baseline network config that supports most common patterns for network isolation, implements baseline network connectivity between AWS and on-prem networks, and provides user-configurable options for network access and administration.

Pre-defined identity and billing frameworks - provide frameworks for cross account user identity and access management and centralized cost management and reporting.

Pre-defined user-selectable packages - provides a series of user selectable packages to integrate AWS-related logs into popular reporting tools, integrate with the AWS Service Catalog, and automate infrastructure. 

## Migrating

### First migrations - Build Experience

Develop migration skills and experience early to help develop more informed decisions while migrating more workloads into AWS.

The goal is to build confidence and experience, identify patterns, and create common processes for performing migrations.

Move 3 to 5 applications.

### Migration Execution

Now scale teams to support the initial wave of migrations. These teams should be operating in tandem.

### Application Migration Process

 - Discover - portfolio analysis and backlog planning to understand the current and future architectures. 
 - Design - The target state is developed and documented. This includes AWS architecture, application architecture, and supporting operational components and processes. This is done using information from the discovery phase.
 - Build - migration design created during the design stage is executed. Assert basic validations against the AWS application.
 - Integrate - Make external connections for the application. Running the application should demonstrate functionality and operation before the application is ready for the validate stage.
 - Validate - Put the application through a series of tests (build verification, functional, performance, disaster recovery, and business continuity) before finalized and released for cutover.
 - Cutover - perform user acceptance testing, and then execute the cutover plan. Ensure that there is an outlined rollback procedure in the plan in case migration is not successful.

### Team Models

- Cloud Business Office - Drives the program, manages resources and budgets, manages and report risk and drives communication and change management.
- Cloud Engineering and Operations - Builds and validates the fundamental components that ensure development, test, and production environments are scalable, automated, maintained and monitored. Also prepares landing zones as needed.
- Innovation - Develops repeatable solutions that will expedite migration in coordination with the platform engineering, migration, and transition teams.
- Portfolio Discovery and Planning - Accelerates downstream activities by executing application discovery and optimizing application backlogs. They work to eliminate objects and minimize wasted effort.


### Migration Factory Teams

- Re-host migration team - migrates high-volume, low-complexity applications that don't need material change. Can use migration automation tools to simplify.

- Re-platform migration team - designs and migrates applications that need a change of platform or a repeatable change in application architecture.

- Re-factor/re-architect migration team - designs and migrates complex or core business applications that have many dependencies. Migration becomes release cycle(s) within the plan.

## Tools

- [AWS Monthly Calculator](https://calculator.s3.amazonaws.com/index.html)
- [AWS TCO Calculator](https://aws.amazon.com/tco-calculator/)
- AWS CAF 
- AWS Migration Readiness Assessment

## Challenge
- What are application discovery tools available

## Resources:

[AWS Operations Whitepapers](https://aws.amazon.com/whitepapers/#operations)
[AWS Managed Services](https://aws.amazon.com/managed-services/features/)
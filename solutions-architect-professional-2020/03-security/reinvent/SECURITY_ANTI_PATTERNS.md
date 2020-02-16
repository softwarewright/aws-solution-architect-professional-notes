# Security Anti-Patterns: Mistakes to Avoid

Lack of SecOps agility
- Slow threat assessment
- Can't patch fast enough


## Four Types of Security Anti Patterns

### Account Root User

Don't 
- allow this to be a personal account

Do
- Assign this to a group distribution list address
- Enable root MFA, hardware device in office safe
- No one logs into account root, IAM only

### AWS Account Overcrowding

When you put a lot of teams in a single account things get very crowded.

Blast radius becomes very large.

Use multi account strategy. Think about the AWS accounts as family homes where specific teams have AWS accounts or where teams working together have shared accounts.

Think of them as individual business capabilities.

### Network Design

- Routing is not security
- Dynamic IP whack-a-mole
- Not defense in depth
- Not scalable

Implement AuthN and AuthZ instead
- Especially for core services
- Highly scalable and auditable

### Network Egress Backhauling

Use the Cloud to Secure the Cloud

### Security Questionnaires

- How some customers audit you

Best Practice Attestations Instead

- SOC2, PCI DSS, HIPPA, etc
- Third Party QSAs verify compliance
- Re-certification cadence

### Manual Technical Auditing vs Automated

Don't do Manual unless you have to.

**Automated**
- CloudWatch Logs + Alarms
- AWS Inspector
- AWS Macie
- Trusted Advisor
- Config Rules
- Cloud Conformity
- Cloud Custodian
- evident.io
- Dome0
- cfn-nag - CloudFormation linting https://github.com/stelligent/cfn_nag

Tips:

- Force security checks
- Limit IAM roles
- Drive controls through practical use cases
- DevSecOps increase agility as well as security
- Empower teams 

## Challenges

- AWS Shield
- VPC Endpoints
- AWS Inspector
- AWS Artifact for attestations
- AWS Config rules
- AWS Auditor Learning Path
- AWS Tech Essentials
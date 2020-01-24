# SaaS Storage Strategies

https://d1.awsstatic.com/whitepapers/Multi_Tenant_SaaS_Storage_Strategies.pdf

## Partitioning Models

- Silo - Separate database for each tenant
- Bridge - Single database with separate schemas for each tenant
- Pool - All tenants share all of the systems storage constructs. This fits well with CD and agility goals essentials to SaaS providers 

## Silo Model

Pros

- Appealing for regulatory and security constraints
- Cross-tenant impacts can be limited
- Contained Availability, minimizing tenant exposer to outage

Cons

- Provisioning and management is more complex, thus creating a layer of complexity and possible point of failure
- Ability to view and react to tenant activity is complicated  by needing system wide health of all tenants
- Limits cost optimization

## Pool Model

The unified approach to handling tenants allowing you to streamline tenant storage provisioning, migration, management, and monitoring.

Pros

- Agility - updating and managing tenants is all centralize
- Storage monitoring and management is simpler
- Clearer opportunities for optimization
- Pool improves deployment automation and operational agility 

Cons

- Higher agility means higher bar for managing scale and availability. This means a heavy investment into automation and testing of environments
- Increases the challenge of data distribution
- Some customers will demand a silo model for their regulatory and data requirements

## Hybrid: The Business Compromise

The tenants will have an influence over how you approach your storage strategy. This strategy might involve more than one of the models.

One possible model is to build a pooled solution as a foundation and then carve out separate databases for tenants that demand a siloed storage solution.

Though complicated it presents the means of tiered offerings to customers.

# AZ-104 Study Notes - Part 2: Azure Infrastructure & Cost Management

## Source
John Savill AZ-104 Administrator Associate Study Cram v2 - Part 2 of 8

---

## Topics Covered

### Administrative Units

- [ ] 📁 **Administrative Units**
  - Provide granular delegation in flat Entra ID structure
  - Create units and add users, groups, or devices
  - Grant roles at administrative unit level (not globally)
  - Roles only apply to objects within that specific unit
  - Used for delegated administration

- [ ] 🔒 **Group Membership Limitation**
  - Adding a group to admin unit does NOT grant permissions to users in that group
  - Must explicitly add users to admin unit separately
  - Safety feature: Prevents privilege escalation by adding users to groups
  - Can manage group object separately from managing users in the group

### Azure Clouds & Environments

- [ ] 🌍 **Azure Clouds (Environments)**
  - **Azure Commercial**: Public cloud for general use
  - **Azure US Government**: For US government agencies and partners
  - **Azure China**: Operated by 21Vianet in China
  - **Other Government Clouds**: Secret instances for specific agencies (unpublished URLs)
  - Each cloud has different control plane URLs and Entra endpoints

- [ ] 🔗 **Cloud Separation**
  - Each cloud has its own separate Entra ID tenants
  - Cannot use same tenant across Azure Commercial, Gov, or China
  - Each cloud has its own set of regions
  - Completely isolated environments

- [ ] 💻 **PowerShell Commands**
  - `Get-AzEnvironment`: View available Azure clouds/environments
  - Shows different URLs for each environment's control plane and Entra

### Regions & Availability

- [ ] 🗺️ **Azure Regions**
  - Geographical locations with multiple data centers
  - Hundreds of regions worldwide (West US, West US 2, West US 3, Canada Central, etc.)
  - Choose regions based on proximity to users (lower latency)
  - Consider data sovereignty and compliance requirements

- [ ] 🏢 **Availability Zones (AZ)**
  - Different sets of data centers within a region
  - Each region has up to 3 availability zones exposed to subscriptions
  - Physical location may have more, but subscription only sees 3
  - Designated as AZ1, AZ2, AZ3
  - Protection against data center failures

- [ ] 🔄 **Zone Redundant Deployment**
  - Resource spans across all availability zones
  - Provides high availability within a region
  - Automatic failover between zones
  - Higher cost but better resilience

- [ ] 🎯 **Zonal Deployment**
  - Resource exists in specific availability zone
  - You control which zone
  - Lower cost than zone redundant
  - Manual failover required

- [ ] 📍 **Availability Zone Strategy**
  - Goal: All regions will eventually have 3 availability zones
  - Avoid dependencies outside your zone or region
  - Design for zone isolation

### Region Pairs

- [ ] 👥 **Paired Regions**
  - Azure pairs regions for disaster recovery
  - Hundreds of miles apart (varies by geography)
  - Stay within same geopolitical boundary
  - **Exception**: Brazil South pairs with South Central US (originally only one Brazil region)

- [ ] 🚀 **Safe Deployment Practice**
  - Azure rollouts follow sequence: Internal → Canary → Pilot → First in pair → Second in pair
  - Paired regions don't get same update simultaneously
  - Reduces risk of simultaneous outages
  - Pairing documented by Microsoft

- [ ] ⚠️ **Pairing De-emphasis**
  - Pairing less critical for some services now
  - Databases: Choose your own replication targets
  - Storage accounts: Object-level replication options
  - Still used for safe deployment practice
  - Risk if not using pairs: Same change might deploy simultaneously

- [ ] 📖 **Pairing Documentation**
  - Documented region pairs available online
  - Gov clouds pair with other gov regions
  - Consider pairs for multi-region strategy
  - Not mandatory but recommended

### Subscriptions & Management Groups

- [ ] 📋 **Azure Subscription**
  - Where you deploy resources
  - Trusts a specific Entra tenant
  - Boundary for virtual networks and some resources
  - Used for organizing resources and billing

- [ ] 🌲 **Management Group Hierarchy**
  - Root Management Group tied to tenant
  - Create custom hierarchy below root
  - Organize by geography, business unit, prod/non-prod
  - Subscriptions tie in at any level
  - Maximum 6 levels deep (not including root and subscription)

- [ ] 🎯 **Management Group Use Cases**
  - **RBAC**: Assign roles at higher levels (inherited downward)
  - **Azure Policy**: Set governance requirements and guard rails
  - **Budgets**: Track spending across multiple subscriptions
  - Structure based on where you need different policies/roles/budgets

- [ ] ⬇️ **Inheritance Model**
  - Roles, policies, and budgets inherit downward
  - Set at high level = applies to all children
  - More generalized at top, more specific closer to resources
  - Can override or add at lower levels

- [ ] 🏗️ **Hierarchy Example**
  - Tenant Root Group → All Company Subscriptions → Prod Management Group → Specific Subscriptions
  - Tenant Root Group → All Company Subscriptions → Non-Prod Management Group → Specific Subscriptions
  - Apply controls at appropriate level in hierarchy

### Cost Management & Analysis

- [ ] 💰 **Consumption-Based Pricing**
  - Pay only for what you use
  - Stop resources when not needed
  - Optimize by choosing right SKU/size
  - Avoid leaving resources running unnecessarily

- [ ] 📊 **Cost Analysis**
  - View spending trends and forecasts
  - Filter by resource, service, location, resource group
  - Views: Accumulated cost, daily cost, cost by resource, cost by service
  - Compare to budget lines
  - Located in Cost Management + Billing

- [ ] 🔮 **Forecast**
  - Based on current spending patterns
  - Predicts end-of-period spend
  - Machine learning-based predictions
  - Helps anticipate budget overruns

- [ ] 🧠 **Smart Views**
  - Detects high-cost resource groups (e.g., "24% of your cost")
  - Identifies spending anomalies
  - Provides cost insights automatically
  - Recommends optimization opportunities

- [ ] 💡 **Azure Advisor - Cost**
  - Recommendations for cost optimization
  - Suggests stopping unused resources
  - Right-sizing recommendations
  - Also covers: Reliability, Performance, Operational Excellence, Security
  - Review at least weekly

### Budgets & Alerts

- [ ] 💵 **Budgets**
  - Set financial limit (e.g., $1000/month)
  - Track actual vs budgeted spend
  - Can create multiple budgets at different scopes

- [ ] 🚨 **Budget Alerts**
  - **Actual spend alerts**: Trigger at threshold (e.g., 80% of budget)
  - **Forecasted spend alerts**: Trigger based on predicted spend (e.g., 120% forecast)
  - Call Action Groups (SMS, email, webhook, Azure Function, etc.)
  - Add alert recipients

- [ ] 📍 **Budget Scope**
  - Set at subscription level
  - Set at management group level
  - Set at resource group level
  - Inherited and aggregated appropriately

### Resource Groups

- [ ] 📦 **Resource Groups**
  - Container for resources within subscription
  - Cannot nest resource groups
  - Apply RBAC, policies, and budgets at RG level
  - Logical grouping of resources

- [ ] 🎯 **Resource Group Strategy**
  - Group resources provisioned together
  - Group resources that run together
  - Group resources deleted together
  - Example: Business application (VMs, load balancer, database, Kubernetes)
  - Common RBAC requirements
  - Common policy requirements
  - Track spend per application

- [ ] 🏗️ **Resource Deployment**
  - All resources must be in a resource group
  - Resources can be in different regions than their RG
  - Can move resources between resource groups
  - Deleting RG deletes all contained resources

### Financial Optimization

- [ ] 🔄 **Azure Hybrid Benefit**
  - Bring existing licenses to Azure
  - **Windows Server**: Standard (move to cloud) or Datacenter (use both on-prem and cloud)
  - **SQL Server**: Bring existing licenses
  - **Red Hat Enterprise Linux**: Bring existing licenses
  - Requires Software Assurance
  - Removes OS/software licensing cost from Azure bill

- [ ] 🎫 **Azure Reservations**
  - 1-year or 3-year commitment
  - Very specific: Exact service in exact region
  - Significant discount (up to 72%)
  - Pay monthly or upfront
  - Best for stable, predictable workloads
  - Examples: Specific VM SKU, storage type in specific region

- [ ] 💰 **Azure Savings Plan**
  - 1-year or 3-year commitment
  - Flexible: Commit to hourly spend amount (e.g., $20/hour)
  - Only applies to included compute services (VMs, App Service Premium V2/V3, Dedicated Hosts)
  - Discount varies by SKU and service type
  - Newer SKUs often have better discounts
  - Applied automatically to best eligible resources each hour
  - Cannot stack with Reserved Instance on same resource

- [ ] ⚖️ **Comparison: Reservations vs Savings Plan**
  - **Reservations**: Specific service + region, higher discount, less flexible
  - **Savings Plan**: Hourly spend commitment, more flexible, varies by SKU
  - Both offer 1-year or 3-year terms
  - Can use different options for different workloads
  - Both are purely financial mechanisms (no technical change)

- [ ] 💻 **VM Cost Components**
  - **Compute**: The VM resource itself
  - **OS License**: Windows Server, RHEL (unless hybrid benefit applied)
  - **Software**: SQL Server, other licensed software running inside
  - Each component can be optimized separately

- [ ] 🧮 **Pricing Calculator**
  - Shows cost breakdown by component
  - Apply hybrid benefit to see savings
  - Compare reservation vs savings plan discounts
  - Try different SKUs to see discount variations
  - Available at: azure.microsoft.com/pricing/calculator

### Tags

- [ ] 🏷️ **Tags (Metadata)**
  - Key-value pairs for organizing resources
  - Limit: 50 tags per resource or resource group
  - Apply to subscriptions, resource groups, or individual resources
  - Use cases: Billing, filtering, tracking ownership, environments

- [ ] ❌ **Tag Inheritance**
  - Tags are NOT inherited
  - Subscription tags don't flow to resource groups
  - Resource group tags don't flow to resources
  - Must be explicitly set at each level

- [ ] 📋 **Azure Policy for Tags**
  - Use Azure Policy to copy/inherit tags
  - Enforce tag requirements
  - Automatically apply tags to resources
  - Examples in built-in policies

- [ ] 🔍 **Tag Examples**
  - **Environment**: Dev, Test, Prod
  - **Owner**: Email or name of responsible person
  - **Cost Center**: Department or billing code
  - **Business Unit**: Division or team
  - **OS Version**: Track OS for inventory
  - **Application**: Which app the resource supports

- [ ] 🎯 **Tag Usage**
  - Filter views in portal by tags
  - Cost analysis by tag values
  - Automation based on tags
  - RBAC can be applied to tagged resources
  - Search and organize resources

---

## Key Takeaways

1. **Administrative Units** - Enable delegated admin in flat Entra structure; adding group doesn't grant rights to users in group
2. **Multiple Azure Clouds** - Commercial, Gov, China are separate with own tenants and regions
3. **Availability Zones** - Always 3 per region; use zone-redundant for HA, zonal for control
4. **Region Pairs** - Used for safe deployment; stay in same geopolitical boundary (except Brazil)
5. **Management Groups** - Create hierarchy for RBAC, Policy, and Budgets inheritance
6. **Resource Groups** - Group resources by lifecycle (provision/run/delete together)
7. **Cost Optimization** - Use Advisor, right-size, stop when not needed, choose appropriate SKUs
8. **Financial Options** - Hybrid Benefit (licenses), Reservations (specific), Savings Plan (flexible)
9. **Budgets & Alerts** - Set actual and forecasted thresholds with Action Groups
10. **Tags Don't Inherit** - Must explicitly set at each level or use Azure Policy

---

## Exam Tips

- Administrative unit membership: Group ≠ Users in group (must add both separately)
- Azure clouds are completely separate (different tenants, can't share)
- Subscription always sees exactly 3 availability zones (even if more physically exist)
- Paired regions: Brazil South → South Central US (only exception to geopolitical boundary rule)
- Management groups: Maximum 6 levels deep (excluding root and subscription level)
- Resource groups cannot be nested
- Savings Plan only applies to included compute services (not all Azure services)
- Cannot combine Savings Plan + Reserved Instance on same resource (one or the other)
- Tags are NOT inherited (common exam question)
- Azure Hybrid Benefit: Datacenter = use both on-prem + cloud; Standard = move to cloud only
- Budget alerts: Actual (already spent) vs Forecasted (predicted to spend)
- Cost components: Compute + OS License + Software (can optimize each separately)

---

## Command Reference

```powershell
# View Azure environments/clouds
Get-AzEnvironment
```

## Portal Navigation

- **Management Groups**: Search "Management groups" → View hierarchy
- **Cost Analysis**: Subscription → Cost Management → Cost analysis
- **Budgets**: Subscription → Cost Management → Budgets
- **Tags**: Resource → Tags OR Resource Group → Tags
- **Azure Advisor**: Search "Advisor" → Review recommendations
- **Pricing Calculator**: azure.microsoft.com/pricing/calculator
- **Azure Regions Map**: azure.microsoft.com/global-infrastructure/geographies

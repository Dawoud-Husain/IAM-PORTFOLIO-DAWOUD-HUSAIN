# AZ-900 Fundamentals — Condensed Review for AZ-104

## Cloud Models

- **Public Cloud** — owned by cloud provider, resources shared across tenants (Azure)
- **Private Cloud** — dedicated to one org, on-prem or hosted
- **Hybrid Cloud** — combines public + private, data/apps move between both

## Service Models (Shared Responsibility)

| | You Manage | Microsoft Manages |
|---|---|---|
| **IaaS** | OS, apps, data, runtime | Hardware, networking, virtualization |
| **PaaS** | Apps, data | OS, runtime, hardware, networking |
| **SaaS** | Data, user access | Everything else |

- **IaaS** = VMs, VNets, Disks (most AZ-104 content)
- **PaaS** = App Service, Azure SQL, Functions, Container Apps
- **SaaS** = Microsoft 365, Dynamics 365

## CapEx vs OpEx

- **CapEx** = upfront spend on physical infrastructure (on-prem servers)
- **OpEx** = pay-as-you-go for cloud services (consumption-based)
- Azure = **OpEx model** — pay only for what you use

## Cloud Benefits (Know the Terms)

- **High Availability** — system uptime; measured in SLA % (99.9%, 99.95%, 99.99%)
- **Scalability** — add resources to handle load (vertical = scale up, horizontal = scale out)
- **Elasticity** — auto-scale based on demand
- **Reliability** — ability to recover from failures, deploy across regions
- **Predictability** — consistent performance and cost forecasting
- **Geo-distribution** — deploy to regions close to users

## Azure Architecture

- **Region** — geographic area with 1+ datacenters
- **Region Pair** — two regions 300+ miles apart for disaster recovery; updates roll one at a time
- **Sovereign Region** — isolated (Azure Government, Azure China 21Vianet)
- **Availability Zone** — physically separate datacenter within a region (own power/cooling/network)
  - Protects against datacenter-level failure
  - 3 zones minimum in enabled regions
- **Availability Set** — logical grouping within a datacenter
  - **Fault Domain** = separate power/network rack (up to 3)
  - **Update Domain** = group for planned maintenance (up to 20)

## Resource Hierarchy (Critical for AZ-104)

```
Management Group (up to 6 levels deep)
  └── Subscription (billing boundary)
        └── Resource Group (cannot be nested)
              └── Resource
```

- A resource belongs to exactly **1 resource group**
- A resource group belongs to exactly **1 subscription**
- Resource groups **cannot be nested**
- Deleting a resource group deletes **all resources** inside it
- Resources can be in a different region than their resource group

## Core Services (AZ-104 Relevant)

| Category | Key Services |
|---|---|
| **Compute** | VMs, VMSS, App Service, Container Instances, Container Apps, Functions |
| **Networking** | VNet, NSG, Load Balancer, App Gateway, VPN Gateway, ExpressRoute, DNS |
| **Storage** | Blob, Files, Queue, Table, Disk |
| **Identity** | Microsoft Entra ID (formerly Azure AD) |
| **Monitoring** | Azure Monitor, Log Analytics, Alerts |
| **Governance** | Azure Policy, Resource Locks, Tags, Blueprints, Management Groups |

## Storage Redundancy (Quick Reference)

| Type | Copies | Scope |
|---|---|---|
| **LRS** | 3 | Single datacenter |
| **ZRS** | 3 | 3 availability zones |
| **GRS** | 6 | 2 regions (LRS + LRS) |
| **GZRS** | 6 | ZRS primary + LRS secondary region |

## Governance Basics

- **Azure Policy** — enforce rules on resources (deny, audit, modify)
- **Resource Locks** — CanNotDelete or ReadOnly (inherited by child resources)
- **Tags** — key-value pairs for organizing/cost tracking (NOT inherited by default)
- **Management Groups** — group subscriptions for policy/RBAC at scale
- **Azure Advisor** — recommendations for cost, security, reliability, performance, operational excellence
- **Microsoft Defender for Cloud** — security posture management + threat protection

# AZ-104 Study Notes - Part 3: Azure Policy, RBAC & Networking Fundamentals

## Source
John Savill AZ-104 Administrator Associate Study Cram v2 - Part 3 of 8

---

## Topics Covered

### Azure Policy

- [ ] 🛡️ **Azure Policy Purpose**
  - Sets guard rails and governance requirements
  - Replaces manual approval processes
  - Enables self-service while maintaining compliance
  - Tracks compliance against policies
  - Prevents non-compliant resource creation

- [ ] 📋 **Policy Definition**
  - Specific condition to check
  - Effect to apply when condition is met
  - Can be built-in or custom
  - Example: "Allowed locations" - deny creation outside approved regions

- [ ] 📊 **Policy Initiative**
  - Collection of multiple policies
  - Example: Azure Security Benchmark (665+ policies)
  - Assign once instead of individually
  - Track compliance at initiative level (easier than tracking 665 separate policies)
  - Custom initiatives can be created

- [ ] ⚙️ **Policy Effects**
  - **Deny**: Blocks the action from happening
  - **Audit**: Logs non-compliance but allows action
  - **Deploy If Not Exists**: Automatically deploys resources (e.g., install agent if missing)
  - Other effects: Append, Modify, AuditIfNotExists, Disabled

- [ ] 🎯 **Policy Best Practice**
  - Start with **Audit** mode first
  - Understand impact before enforcing
  - Switch to **Deny** after validation
  - Prevents unintended disruption

- [ ] 📍 **Policy Scope**
  - Assign at Management Group level
  - Assign at Subscription level
  - Assign at Resource Group level
  - Inherited downward

- [ ] 🔐 **Microsoft Cloud Security Benchmark**
  - Free initiative
  - Regulatory compliance requires paid Defender for Cloud plan
  - Paid plan adds: FedRAMP, HIPAA, ISO, PCI-DSS, etc.

- [ ] 🔄 **Tag Inheritance via Policy**
  - Built-in policies can inherit/copy tags from parent
  - "Inherit tag from resource group if missing"
  - "Inherit tag from subscription if missing"
  - Workaround for tags not inheriting by default

### Role-Based Access Control (RBAC)

- [ ] 🔑 **RBAC Fundamentals**
  - Assign permissions at specific scope
  - **Least Privilege**: Minimum permissions at smallest scope
  - **Principle**: Identity + Role + Scope = Role Assignment
  - Always assign roles to groups (not individual users)

- [ ] 👥 **RBAC Components**
  - **Identity**: User, group, service principal (app identity)
  - **Role**: Set of actions from resource providers
  - **Scope**: Management group, subscription, resource group, or resource
  - **Role Assignment**: Combination of identity + role + scope

- [ ] 🎭 **Built-in Roles**
  - **Owner**: Full access including permission changes (~16,000 permissions)
  - **Contributor**: Full access EXCEPT permission changes
  - **Reader**: Read-only access to everything
  - Hundreds of specific roles for different services

- [ ] ✏️ **Custom Roles**
  - Create roles with specific permissions
  - Clone existing role and modify
  - Add or exclude specific actions
  - Use wildcards then exclude specific operations
  - Define valid assignment scopes

- [ ] 📍 **RBAC Scope Examples**
  - Network team: Broad scope (Management Group) for all virtual networks
  - Subscription admin: Subscription level
  - Project team: Resource Group level
  - Individual resource: Rarely used, but possible

- [ ] ⬇️ **RBAC Inheritance**
  - Permissions inherit downward
  - Management Group → Subscription → Resource Group → Resource
  - Can view inherited vs directly assigned roles

- [ ] 🔗 **Entra Roles vs Azure Roles**
  - **Entra Roles**: Manage tenant (users, groups, applications)
  - **Azure Roles**: Manage Azure resources (VMs, storage, networks)
  - Separate permission systems
  - Global Admin can elevate to User Access Administrator for Azure subscriptions

- [ ] 👑 **User Access Administrator**
  - Entra Global Admin can enable this for themselves
  - Grants RBAC permissions across all subscriptions that trust the tenant
  - Emergency access mechanism

- [ ] ⏱️ **Privileged Identity Management (PIM)**
  - Just-in-time access
  - Elevated roles granted temporarily
  - Time-limited assignments
  - Reduces standing privileged access
  - Requires Entra ID P2 license

### Resource Locks

- [ ] 🔒 **Resource Lock Types**
  - **Cannot Delete**: Can modify, cannot delete
  - **Read Only**: Cannot modify or delete (read-only)
  - Apply at subscription, resource group, or resource level

- [ ] ⬇️ **Lock Inheritance**
  - Locks inherit to child resources
  - Subscription lock → affects all resource groups and resources
  - Resource group lock → affects all resources in RG

- [ ] ⚠️ **Control Plane vs Data Plane**
  - **Locks only affect Control Plane** (Azure Resource Manager operations)
  - Control Plane: Create/delete/modify Azure resources
  - Data Plane: Operations within the resource (write records, create blobs)
  - **Example**: Storage account with Delete lock
    - ❌ Cannot delete the storage account (control plane)
    - ✅ Can still delete blobs inside (data plane)

- [ ] 📦 **Lock Examples**
  - Database with Cannot Delete: Can't delete database, can delete records in tables
  - Storage with Read Only: Can't resize/change account, can still create/delete blobs
  - Backup protection locks: Automatically created to prevent deletion

### Azure Networking Fundamentals

#### Networking Costs

- [ ] 💸 **Data Transfer Pricing**
  - **Ingress**: FREE (data coming into Azure)
  - **Egress**: CHARGED (data leaving Azure data centers)
  - Applies to internet egress and some cross-region traffic

#### Virtual Networks (VNets)

- [ ] 🌐 **Virtual Network Basics**
  - Regional resource (cannot span regions)
  - Lives within one subscription
  - Defined by one or more IPv4 CIDR ranges
  - Optionally supports IPv6 (must also have IPv4)
  - Foundation for Azure networking

- [ ] 📦 **RFC 1918 Private IP Ranges**
  - **10.0.0.0/8**: 10.0.0.0 - 10.255.255.255
  - **172.16.0.0/12**: 172.16.0.0 - 172.31.255.255
  - **192.168.0.0/16**: 192.168.0.0 - 192.168.255.255
  - Can use non-RFC1918 ranges (won't be internet routable without NAT)

- [ ] 🔢 **IP Address Planning**
  - Use unique IP ranges (avoid overlaps with on-premises and other VNets)
  - Overlapping IPs require NAT for connectivity
  - Plan for growth and peering

#### Subnets

- [ ] 🗂️ **Subnet Characteristics**
  - Subset of VNet IP space
  - Regional (spans all availability zones)
  - No AZ restriction on subnet
  - Resources in same subnet can be in different AZs

- [ ] ❌ **Reserved IP Addresses (5 per subnet)**
  - **.0**: Network address (e.g., 10.0.1.0)
  - **.1**: Azure Gateway (e.g., 10.0.1.1)
  - **.2 and .3**: DNS purposes (e.g., 10.0.1.2, 10.0.1.3)
  - **.255**: Broadcast address (e.g., 10.0.1.255)
  - **Always lose 5 IPs regardless of subnet size**

- [ ] 🔌 **Resource Connectivity**
  - Resources don't live "in" subnet
  - Virtual NIC attaches to subnet
  - Private IP allocated from subnet range via DHCP
  - Cannot run own DHCP server

- [ ] 📌 **Static Private IP**
  - Can configure Azure to always assign same IP
  - Still uses DHCP but with static reservation
  - Configured in Azure fabric

#### Public IP Addresses

- [ ] 🌍 **Public IP Overview**
  - Allow internet access to resources
  - Can be IPv4 or IPv6
  - Regional or Global (for anycast services)
  - Associated with resource network configuration

- [ ] 📋 **Public IP SKUs**
  - **Basic**: DEPRECATED (retiring Sept 30, 2025) - dynamic or static
  - **Standard**: REQUIRED going forward - always static
  - Use Standard SKU for all new deployments

- [ ] 📦 **Public IP Prefix**
  - Contiguous block of public IPs
  - Reserve range of IPs together
  - Easier management for multiple resources

- [ ] 🏠 **Bring Your Own IP (BYOIP)**
  - Bring your own public IP prefix to Azure
  - IPv4: Must be /21 to /24
  - IPv6: Must be /48
  - Process: Validate ownership → Provision → Assign → Commission
  - Complex process but available

- [ ] ⚠️ **Direct Public IP Assignment**
  - Assigning public IP directly to resource is not ideal
  - Better: Use load balancer, NAT Gateway, or Azure Firewall
  - Provides better availability and security

#### Internet Access

- [ ] 🌐 **Outbound Internet Access**
  - **Default implicit internet access**: GOING AWAY
  - Must configure explicit method:
    - Public IP on resource
    - NAT Gateway on subnet
    - Azure Firewall with user-defined routes
    - Network Virtual Appliance (NVA)
    - Standard Load Balancer with outbound rules

- [ ] 🔀 **NAT Gateway**
  - Efficient SNAT (Source Network Address Translation)
  - Associate with subnet
  - Provides outbound internet connectivity
  - Uses public IP or public IP prefix

- [ ] ⚖️ **Standard Load Balancer**
  - High availability for internet-facing services
  - Backend pool for resiliency
  - Requires outbound rules for egress
  - Public IP on frontend

#### VNet Peering

- [ ] 🔗 **VNet Peering Overview**
  - Connect virtual networks together
  - Private IP communication between VNets
  - Can be within region or inter-region (global peering)
  - Low latency, high bandwidth
  - No gateway required for basic peering

- [ ] 🌟 **Hub-Spoke Topology**
  - Hub VNet contains shared services (Gateway, Firewall)
  - Spoke VNets connect to Hub
  - Spokes don't directly peer to each other
  - Centralized connectivity and management

- [ ] 🚪 **Gateway Transit**
  - Hub has VPN Gateway or ExpressRoute Gateway
  - Spokes use hub's gateway for on-premises connectivity
  - **Hub side**: "Allow gateway transit" (previously "Allow forwarded traffic")
  - **Spoke side**: "Use remote gateway"
  - BGP shares routes from gateway to spoke VNets

- [ ] 📡 **BGP (Border Gateway Protocol)**
  - Shares routes between networks
  - Gateway tells VNet about on-premises IP ranges
  - VNet knows to send traffic across peering to gateway
  - Automatic route propagation

- [ ] ⚙️ **Peering Configuration**
  - Create peering on both sides of connection
  - Each side has own settings
  - Terminology changed from "Gateway Transit" to "Allow forwarded traffic" in portal
  - Bi-directional configuration required

---

## Key Takeaways

1. **Azure Policy** - Start with Audit mode, then enforce with Deny; use Initiatives for multiple policies
2. **RBAC Principle** - Identity + Role + Scope = Role Assignment (always use groups!)
3. **Least Privilege** - Minimum permissions at smallest scope necessary
4. **Resource Locks** - Only affect control plane, not data plane operations
5. **5 Reserved IPs** - Every subnet loses 5 IPs (.0, .1, .2, .3, .255)
6. **VNet Boundaries** - Regional and subscription-bound, cannot span
7. **Public IP** - Use Standard SKU (Basic retiring 2025); avoid direct assignment when possible
8. **Implicit Internet Access** - Going away; must explicitly configure outbound access
9. **VNet Peering** - Use hub-spoke with gateway transit for centralized connectivity
10. **Ingress Free, Egress Charged** - Plan for data transfer costs

---

## Exam Tips

- **Azure Policy**: Remember Deploy If Not Exists effect can automatically remediate
- **Regulatory Compliance**: Requires paid Defender for Cloud plan (except Cloud Security Benchmark)
- **RBAC**: Owner has ~16,000 permissions; Contributor = Owner minus permission changes
- **Custom Roles**: Can clone existing role and modify for specific needs
- **Entra vs Azure Roles**: Separate systems; Global Admin can elevate to User Access Administrator
- **Locks on Storage**: Cannot delete account, but CAN delete blobs (control vs data plane)
- **Subnet IP Math**: /24 = 256 IPs - 5 reserved = 251 usable
- **Public IP SKUs**: Basic retiring Sept 30, 2025 - always use Standard
- **Gateway Transit**: Hub allows, Spoke uses (both settings required)
- **VNet Peering**: Not transitive (spoke-to-spoke traffic doesn't work without routing)

---

## Portal Navigation

- **Azure Policy**: Search "Policy" → Overview (compliance) → Definitions → Assignments
- **RBAC**: Resource → Access Control (IAM) → Role assignments → Add
- **Custom Roles**: Management Group/Subscription → Access Control → Roles → Add custom role
- **Locks**: Resource/Resource Group/Subscription → Locks → Add
- **Virtual Networks**: Search "Virtual networks" → Create → Configure address space
- **Subnets**: Virtual Network → Subnets → Add subnet
- **Public IPs**: Search "Public IP addresses" → Create (choose Standard SKU)
- **VNet Peering**: Virtual Network → Peerings → Add peering

---

## PowerShell/CLI Reference

```powershell
# View Azure environments
Get-AzEnvironment

# Azure Policy
Get-AzPolicyDefinition
Get-AzPolicyAssignment
New-AzPolicyAssignment

# RBAC
Get-AzRoleDefinition
Get-AzRoleAssignment
New-AzRoleAssignment

# Locks
Get-AzResourceLock
New-AzResourceLock -LockLevel CanNotDelete
New-AzResourceLock -LockLevel ReadOnly

# Networking
New-AzVirtualNetwork
New-AzVirtualNetworkSubnetConfig
New-AzPublicIpAddress -Sku Standard
Add-AzVirtualNetworkPeering
```

---

## Important Concepts

### Control Plane vs Data Plane

**Control Plane (Azure Resource Manager):**
- Create, modify, delete resources
- Resize resources
- Change configurations
- **Affected by Resource Locks**

**Data Plane (Service-specific operations):**
- Write database records
- Create/delete blobs in storage
- Upload files
- **NOT affected by Resource Locks**

### RBAC Assignment Formula

```
Identity (User/Group/Service Principal)
    +
Role (Set of Actions)
    +
Scope (Management Group/Subscription/RG/Resource)
    =
Role Assignment
```

### Subnet IP Calculation

For a /24 subnet (10.0.1.0/24):
- Total IPs: 256
- Reserved: 5
- **Usable: 251**

Reserved IPs:
1. 10.0.1.0 - Network
2. 10.0.1.1 - Gateway
3. 10.0.1.2 - DNS
4. 10.0.1.3 - DNS
5. 10.0.1.255 - Broadcast

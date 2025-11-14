# AZ-104 Study Notes - Part 7: Storage Security, Managed Disks & Compute

## Source
John Savill AZ-104 Administrator Associate Study Cram v2 - Part 7 of 8

---

## Topics Covered

### Azure File Sync

- [ ] 🔄 **Azure File Sync Overview**
  - Synchronize on-premises Windows file servers with Azure Files
  - Create sync groups (one cloud endpoint, up to 100 server endpoints)
  - Changes go to cloud, then replicate to other servers
  - Bi-directional sync capability

- [ ] 📁 **Sync Group Components**
  - **Cloud Endpoint**: Azure File Share (one per sync group)
  - **Server Endpoints**: On-premises Windows file servers (up to 100)
  - Central hub-and-spoke model (cloud is hub)

- [ ] 📊 **Cloud Tiering**
  - Offload least-used files to Azure
  - Keep "thumbnail" (link) on-premises
  - Pulls down data when accessed
  - Free up local storage space
  - Infinite scale capability
  - Configure based on free space percentage

- [ ] 🎯 **Use Cases**
  - Multi-site file server consolidation
  - Disaster recovery (cloud as backup)
  - Infinite storage capacity
  - Lift-and-shift scenarios
  - Branch office file access

### Storage Account Security

#### Access Keys

- [ ] 🔑 **Storage Account Access Keys**
  - Two access keys (key1 and key2)
  - All-powerful access to entire storage account
  - Enable key rotation without downtime
  - **Rotation process**: Regenerate key2 → Switch apps to key2 → Regenerate key1

- [ ] ⚠️ **Access Key Limitations**
  - Full access to all services
  - No granular permissions
  - Hard to audit who's using them
  - Not recommended for modern applications

- [ ] 🚫 **Disable Access Keys**
  - Can completely block storage account key access
  - Setting: "Allow storage account key access" (can disable)
  - Forces use of Entra ID (RBAC)
  - **Warning**: Disabling blocks Shared Access Signatures (SAS)

#### Shared Access Signatures (SAS)

- [ ] 🎫 **Shared Access Signature (SAS)**
  - Granular access to storage resources
  - Time-limited access
  - Specify permissions (read, write, delete, etc.)
  - Specify IP ranges
  - Signed by storage account access key

- [ ] 📦 **SAS Types**
  - **Account-level SAS**: Access to multiple services (Blob, File, Queue, Table)
  - **Service-level SAS**: Access to specific service (e.g., only Blob)
  - Configure at container or blob level

- [ ] ⚙️ **SAS Configuration**
  - Choose services (Blob, File, Queue, Table)
  - Choose permissions (Read, Write, Delete, List, etc.)
  - Set start and expiry time
  - Specify allowed IP addresses
  - Specify allowed protocols (HTTP/HTTPS)

- [ ] ⚠️ **SAS Limitations**
  - Signed by access key (if keys disabled, SAS won't work)
  - Cannot be revoked individually (must regenerate key)
  - Consider using Entra ID RBAC instead

#### Data Plane RBAC

- [ ] 🔐 **Data Plane Roles**
  - **Blob Data Owner**: Full access to blobs and containers
  - **Blob Data Contributor**: Read, write, delete blobs
  - **Blob Data Reader**: Read blobs and containers
  - **File Data SMB Share Contributor**: Read, write, delete in file shares
  - **Queue Data Contributor**: Add, read, delete messages
  - **Table Data Contributor**: Read, write, delete table entities

- [ ] 🎯 **Advantages over Access Keys**
  - Granular permissions (per-service, per-container)
  - Entra ID integration (users, groups, service principals)
  - Auditable access
  - No shared secrets
  - Preferred modern approach

- [ ] 🔍 **Find Data Plane Roles**
  - Portal: Storage Account → Access Control (IAM) → Roles
  - Search for "data" to filter data plane roles
  - Available for Blob, File, Queue, Table

#### Encryption

- [ ] 🔒 **Platform-Managed Keys (Default)**
  - Microsoft manages encryption keys
  - No additional configuration required
  - Encrypted at rest automatically
  - No key rotation responsibility

- [ ] 🔑 **Customer-Managed Keys (CMK)**
  - Use your own encryption key from Azure Key Vault
  - **You are responsible for key rotation**
  - Can use Key Vault auto-rotation policies
  - Use Azure Policy to alert when <30 days until expiration
  - More control, more responsibility

- [ ] 📦 **Encryption Scopes**
  - Different encryption keys for different containers or blobs
  - Set at storage account, container, or blob level
  - **Use case**: ISV with multiple customers (different key per customer)
  - Avoid needing separate storage accounts per customer

- [ ] ⚙️ **Encryption Scope Configuration**
  - Create encryption scope with specific key
  - Assign to container (all blobs use that scope)
  - Or specify at blob upload time
  - More granular than account-level encryption

### Managed Disks

#### Disk Types & Performance

- [ ] 💾 **Standard HDD**
  - Lowest cost
  - Magnetic disk drives
  - Performance scales with capacity (bigger = faster)
  - Use for: Dev/test, infrequent access

- [ ] 📀 **Standard SSD**
  - SSD-based, better performance than HDD
  - Performance scales with capacity
  - Supports bursting
  - Use for: Web servers, lightly-used applications

- [ ] ⚡ **Premium SSD**
  - High performance SSD
  - Performance scales with capacity
  - Supports bursting
  - Can select performance tier (without changing size)
  - Use for: Production workloads, databases

- [ ] 🚀 **Premium SSD v2**
  - **Performance NOT tied to capacity**
  - Independently configure: Capacity, IOPS, Throughput
  - Pay for what you provision
  - **Dynamic adjustment**: Change IOPS/throughput without downtime
  - More flexible than Premium SSD

- [ ] ⚡ **Ultra Disk**
  - Lowest latency (sub-millisecond, ~0.5ms)
  - **Performance NOT tied to capacity**
  - Independently configure: Capacity, IOPS, Throughput
  - Highest performance option
  - Use for: IO-intensive workloads (SAP HANA, SQL, etc.)

- [ ] 📊 **Performance Scaling Models**
  - **Standard HDD/SSD, Premium SSD**: Capacity ↑ = Performance ↑
  - **Premium SSD v2, Ultra Disk**: Capacity and Performance independent

- [ ] 🔥 **Bursting**
  - Temporarily exceed baseline performance
  - Free burst credits available
  - Paid bursting options
  - Premium SSD and Standard SSD support bursting

- [ ] 📈 **Performance Tiers (Premium SSD only)**
  - Temporarily increase performance WITHOUT increasing size
  - Can shrink tier back down (cannot shrink disk size)
  - Workaround for "cannot shrink disk" limitation
  - Only available for Premium SSD (not v2)

- [ ] ⚠️ **Disk Size Constraints**
  - **Cannot shrink disks** (only grow)
  - To shrink: Create new smaller disk, copy data, swap
  - Plan disk sizes carefully
  - Use performance tiers for temporary performance boosts

- [ ] 🔗 **Disk Sharing**
  - Standard SSD and Premium SSD support shared disks
  - Multiple VMs can attach to same disk
  - Use for: Clustered applications (SQL FCI, etc.)
  - Requires application-level coordination

#### Managed Disk Encryption

- [ ] 🔐 **Disk Encryption at Rest (Default)**
  - Automatically encrypted at rest
  - Platform-managed keys by default
  - Transparent to VM

- [ ] 🔑 **Customer-Managed Keys (Disk Encryption Set)**
  - Use your own encryption key from Key Vault
  - Create **Disk Encryption Set** referencing Key Vault key
  - Assign managed disks to Disk Encryption Set
  - All disks in set use same encryption key
  - You manage key rotation

- [ ] 💻 **Azure Disk Encryption (ADE)**
  - Encryption inside the guest OS
  - **Windows**: BitLocker
  - **Linux**: dm-crypt
  - Keys stored in Azure Key Vault
  - More flexible but more constraints on management
  - Encrypts OS and data disks

- [ ] 🖥️ **Encryption at Host**
  - Encrypts data on the physical host
  - Encrypts temp disk (with platform-managed key)
  - Encrypts cache files (with same key as disk)
  - **Encrypts data in transit** (disk ↔ host)
  - Can combine with Disk Encryption Set

- [ ] 🎯 **Encryption Comparison**
  - **Platform-managed**: Default, no effort required
  - **Disk Encryption Set**: Customer key, disk-level encryption at rest
  - **Azure Disk Encryption (ADE)**: Guest OS encryption (BitLocker/dm-crypt)
  - **Encryption at Host**: Host-level, temp disk, cache, data in transit
  - **Best practice**: Disk Encryption Set + Encryption at Host

### Infrastructure as Code (IaC)

#### Provisioning Methods

- [ ] 🚫 **Azure Portal (Not Recommended for Production)**
  - Good for learning and exploration
  - Manual, error-prone at scale
  - Not repeatable
  - Hard to version control
  - Mistakes likely with complex configurations

- [ ] ⚠️ **Azure CLI / PowerShell (Better, but not ideal)**
  - Can script provisioning
  - Version control possible
  - **Problem**: Different commands for create vs update
  - Imperative (tell it HOW to do it)
  - Can't just re-run script if resource exists

- [ ] ✅ **ARM Templates / Bicep (Recommended)**
  - **Declarative**: Specify WHAT you want, not HOW
  - Idempotent: Run multiple times, same result
  - Apply template → resource created
  - Change template → re-apply → resource updated
  - Version control friendly
  - Pipeline integration

#### ARM Templates

- [ ] 📄 **ARM Template Structure (JSON)**
  - `$schema`: Template schema version
  - `contentVersion`: Your template version
  - `parameters`: Input values
  - `variables`: Computed values
  - `resources`: Azure resources to create
  - `outputs`: Values to return after deployment

- [ ] 📦 **ARM Template Resources**
  - Each resource has:
    - `type`: Resource provider and type (e.g., Microsoft.Compute/virtualMachines)
    - `apiVersion`: API version to use
    - `name`: Resource name
    - `location`: Azure region
    - `properties`: Resource-specific configuration

- [ ] 🔍 **Export Template**
  - Most resources have "Export template" option
  - Generates ARM JSON from existing resource
  - Good starting point for your own templates
  - May need cleanup/editing

- [ ] 📋 **Template Deployment**
  - Portal shows template before creating resource
  - Can download and customize
  - Deploy via Portal, CLI, PowerShell, DevOps

#### Bicep

- [ ] 🎯 **Bicep Overview**
  - Azure-native declarative language
  - Transpiles to ARM JSON
  - **More human-readable** than JSON
  - Less verbose
  - Better tooling and IntelliSense

- [ ] 🔄 **Bicep vs ARM JSON**
  - Bicep is easier to write and read
  - ARM JSON is what Azure actually uses
  - Bicep compiles to ARM JSON
  - Can decompile ARM JSON to Bicep

- [ ] ⚙️ **Bicep Features**
  - Cleaner syntax
  - Modules for reusability
  - Strong typing
  - Better error messages
  - VS Code extension

- [ ] 📚 **Template Resources**
  - Azure Quickstart Templates (GitHub)
  - Huge library of examples
  - Search by service (VM, Storage, Networking, etc.)
  - View as ARM JSON or Bicep

#### Third-Party IaC

- [ ] 🏗️ **Terraform**
  - HashiCorp's IaC tool
  - Multi-cloud support
  - Large ecosystem
  - HCL (HashiCorp Configuration Language)
  - Alternative to ARM/Bicep

### Cloud Service Models

#### Responsibility Layers

- [ ] 🏢 **On-Premises**
  - You manage everything:
    - Physical servers, storage, networking
    - Hypervisor
    - Operating system
    - Runtime
    - Application
    - Data

- [ ] ☁️ **Infrastructure as a Service (IaaS)**
  - Azure manages: Storage, Networking, Compute, Hypervisor
  - **You manage**: OS, Runtime, Application, Data
  - **You are responsible for**:
    - Patching OS
    - Antivirus/Antimalware
    - Backup
    - DR planning
    - Updates
  - Examples: Virtual Machines, Virtual Machine Scale Sets

- [ ] 🎯 **Platform as a Service (PaaS)**
  - Azure manages: Storage through Runtime
  - **You manage**: Application, Data
  - No OS patching required
  - Examples: App Services, Azure SQL Database, AKS (hybrid)
  - **Focus on business value, not infrastructure**

- [ ] ⚡ **Serverless**
  - Extreme PaaS
  - Azure manages everything except code and data
  - Event-driven, consumption-based
  - Examples: Azure Functions, Logic Apps
  - No servers to manage

- [ ] 📧 **Software as a Service (SaaS)**
  - Fully managed application
  - You manage: Identities, some configuration
  - Examples: Microsoft 365, Dynamics 365
  - Not Azure, but consume Azure services

- [ ] 🎯 **Goal: Move Right on Spectrum**
  - On-Prem → IaaS → PaaS → Serverless → SaaS
  - Focus on business value, not infrastructure
  - Your differentiation is usually the application, not the OS

### Virtual Machines

#### VM SKUs and Sizes

- [ ] 📐 **VM Dimensions**
  - **vCPU**: Number of virtual CPUs
  - **Memory**: RAM amount
  - **Storage**: Temp disk, throughput, IOPS
  - **Network**: Bandwidth, NICs
  - **Special**: GPUs, FPGAs, etc.

- [ ] 📊 **CPU-to-Memory Ratio**
  - Different workloads need different ratios
  - General Purpose: 1:4 (1 vCPU : 4 GB RAM)
  - Compute Optimized: 1:2 (more CPU, less RAM)
  - Memory Optimized: 1:8 (less CPU, more RAM)

- [ ] 🎯 **Workload Shape**
  - Understand your workload requirements
  - Database: Memory and storage intensive
  - Web server: Balanced (general purpose)
  - Batch processing: CPU intensive
  - **Match VM SKU to workload shape**

- [ ] 📏 **VM Sizing Strategy**
  - Don't pick one giant VM
  - Use multiple smaller VMs for scaling
  - Scale out (add/remove instances) as demand changes
  - More flexible and cost-effective

#### VM Series

- [ ] 🔷 **General Purpose (A, B, D, D_v2-v5)**
  - **Ratio**: 1 vCPU : 4 GB RAM
  - Balanced CPU, memory, storage
  - **D-series**: Includes temp disk
  - **B-series**: Burstable (bank credits, burst when needed)
  - Use for: Web servers, small databases, dev/test

- [ ] ⚡ **Compute Optimized (F-series)**
  - **Ratio**: 1 vCPU : 2 GB RAM
  - High CPU, lower memory
  - Use for: Batch processing, web servers, analytics

- [ ] 🧠 **Memory Optimized (E, M, D_v2-v5)**
  - **Ratio**: 1 vCPU : 8 GB RAM (or higher)
  - High memory, moderate CPU
  - **E-series**: General memory workloads
  - **M-series**: Largest RAM (up to 12 TB!)
  - **Eb/Mb-series**: Enhanced remote storage
  - Use for: Databases, in-memory caching, SAP HANA

- [ ] 💾 **Storage Optimized (L-series)**
  - Local NVMe drives
  - Very high disk throughput and IOPS
  - Use for: NoSQL databases, data warehousing

- [ ] 🎮 **GPU (N-series)**
  - NVIDIA GPUs
  - **NC-series**: Compute and AI
  - **ND-series**: Deep learning
  - **NV-series**: Graphics and visualization
  - Use for: AI/ML, rendering, simulations

- [ ] 💰 **B-series (Burstable)**
  - Low baseline CPU, can burst
  - Bank credits when using <baseline
  - Use credits to burst above baseline
  - Cheapest option for variable workloads
  - Use for: Dev/test, low-traffic websites

- [ ] 🔤 **Naming Convention**
  - **Family**: D, E, F, etc.
  - **Version**: v2, v3, v4, v5 (newer = better)
  - **Features**:
    - `s` = Premium Storage support
    - `d` = Temp disk included
    - `a` = AMD processor
  - Example: **D4s_v5** = D-series, 4 vCPU, Premium Storage, version 5

- [ ] 📦 **Storage Variants**
  - **With 's'**: Supports Premium SSD (Ds, Es, Fs)
  - **Without 's'**: Standard storage only
  - **With 'd'**: Includes temp disk
  - **Without 'd'**: No temp disk

- [ ] 📊 **Remote Storage Performance**
  - **Eb-series, Mb-series**: Enhanced remote storage throughput
  - Better for workloads with high disk I/O
  - Higher remote disk IOPS and throughput

---

## Key Takeaways

1. **Azure File Sync** - Up to 100 server endpoints, one cloud endpoint, cloud tiering
2. **Access Keys** - Two keys for rotation, all-powerful, can disable entirely
3. **SAS** - Signed by access key (won't work if keys disabled)
4. **Data Plane RBAC** - Preferred over access keys (granular, auditable)
5. **Encryption Scopes** - Different keys per container/blob (multi-tenant scenarios)
6. **Disk Encryption Set** - Customer-managed keys for managed disks
7. **Encryption at Host** - Encrypts temp disk, cache, data in transit
8. **Cannot Shrink Disks** - Only grow (plan carefully!)
9. **Premium SSD v2 & Ultra** - Performance independent of capacity
10. **Performance Tiers** - Premium SSD can change tier without resizing
11. **ARM/Bicep** - Declarative, idempotent, recommended for production
12. **IaaS → PaaS** - Reduce management overhead, focus on business value
13. **VM Ratios** - General 1:4, Compute 1:2, Memory 1:8
14. **B-series** - Burstable, bank credits, cheapest for variable workloads

---

## Exam Tips

- **File Sync**: One cloud endpoint, up to 100 server endpoints per sync group
- **Access Keys**: Two keys (key1, key2) for rotation without downtime
- **SAS Dependency**: SAS tokens signed by access key (disabling keys breaks SAS)
- **Encryption Scopes**: Can set at storage account, container, or blob level
- **Disk Encryption Set**: Create first, then assign disks to it
- **ADE vs Disk Encryption Set**: ADE = guest OS (BitLocker/dm-crypt), DES = disk-level
- **Encryption at Host**: Encrypts temp disk, cache, and data in transit
- **Disk Shrinking**: NOT POSSIBLE (only grow, or create new disk)
- **Premium SSD Performance Tier**: Can increase/decrease tier without resizing disk
- **Premium SSD v2/Ultra**: Capacity and performance configured independently
- **ARM Template**: Declarative, idempotent (can re-run safely)
- **Bicep**: Transpiles to ARM JSON, more readable
- **IaaS Responsibility**: You patch OS, configure antivirus, manage backups
- **PaaS Responsibility**: Azure manages OS, you manage app and data
- **VM Ratio**: General 1:4, Compute 1:2, Memory 1:8 (memorize!)
- **D-series 'd'**: Includes temp disk
- **'s' in VM name**: Premium Storage support (Ds, Es, Fs)
- **B-series**: Burstable (baseline + credits)
- **Eb/Mb-series**: Enhanced remote storage performance

---

## Portal Navigation

- **File Sync**: Search "Storage Sync Services" → Create sync group
- **Access Keys**: Storage Account → Security + networking → Access keys
- **SAS**: Storage Account → Security + networking → Shared access signature
- **Data Plane RBAC**: Storage Account → Access Control (IAM) → Search "data"
- **Encryption**: Storage Account → Security + networking → Encryption
- **Encryption Scopes**: Storage Account → Encryption → Encryption scopes
- **Disk Encryption Set**: Search "Disk Encryption Sets" → Create
- **ARM Template**: Any resource → Export template
- **VM Sizes**: Virtual Machine → Size → View all sizes

---

## VM Series Quick Reference

| Series | Ratio | Use Case | Special Features |
|--------|-------|----------|------------------|
| **A-series** | 1:4 | Entry-level, dev/test | Lowest cost |
| **B-series** | Variable | Burstable workloads | Bank credits, burst CPU |
| **D-series** | 1:4 | General purpose | Temp disk (if 'd'), balanced |
| **F-series** | 1:2 | Compute optimized | High CPU/RAM ratio |
| **E-series** | 1:8 | Memory optimized | High RAM |
| **M-series** | 1:8+ | Memory optimized | Massive RAM (up to 12TB) |
| **Eb/Mb** | 1:8+ | Memory + storage | Enhanced remote storage |
| **L-series** | Varies | Storage optimized | Local NVMe drives |
| **N-series** | Varies | GPU workloads | NVIDIA GPUs |

---

## Disk Type Comparison

| Type | Storage | Performance Model | IOPS/Throughput | Bursting | Best For |
|------|---------|-------------------|-----------------|----------|----------|
| **Standard HDD** | HDD | Scales with capacity | Low | No | Dev/test, infrequent |
| **Standard SSD** | SSD | Scales with capacity | Medium | Yes | Web servers, light apps |
| **Premium SSD** | SSD | Scales with capacity | High | Yes | Production, databases |
| **Premium SSD v2** | SSD | **Independent** | Configure separately | No | Flexible performance |
| **Ultra Disk** | SSD | **Independent** | Configure separately | No | Highest performance, sub-ms latency |

---

## PowerShell/CLI Commands

```powershell
# File Sync
New-AzStorageSyncService -ResourceGroupName RG -Name MySyncService
New-AzStorageSyncGroup -Name MySyncGroup

# Access Keys
Get-AzStorageAccountKey -ResourceGroupName RG -Name mystorageacct
New-AzStorageAccountKey -ResourceGroupName RG -Name mystorageacct -KeyName key1

# Disk Encryption Set
New-AzDiskEncryptionSet -ResourceGroupName RG -Name MyDES -KeyUrl $keyVaultKey

# Managed Disk
New-AzDisk -ResourceGroupName RG -DiskName MyDisk -Disk $diskConfig
Update-AzDisk -ResourceGroupName RG -DiskName MyDisk -Disk $diskUpdate

# ARM Template Deployment
New-AzResourceGroupDeployment -ResourceGroupName RG -TemplateFile template.json

# Bicep
az bicep build --file main.bicep
az deployment group create --resource-group RG --template-file main.bicep

# VM
New-AzVM -ResourceGroupName RG -Name MyVM -Size Standard_D2s_v5
```

---

## Encryption Decision Tree

```
Need encryption beyond default?
├─ Storage Account
│   ├─ Whole account with custom key?
│   │   └─ Customer-Managed Key
│   └─ Different keys per container/blob?
│       └─ Encryption Scopes
└─ Managed Disks
    ├─ Multiple disks same key?
    │   └─ Disk Encryption Set
    ├─ Guest OS encryption?
    │   └─ Azure Disk Encryption (BitLocker/dm-crypt)
    └─ Encrypt temp disk & cache?
        └─ Encryption at Host
```

---

## IaC Selection

```
Provisioning Azure resources?
├─ Learning/exploring?
│   └─ Portal (OK for learning)
├─ One-off task?
│   └─ CLI/PowerShell (acceptable)
└─ Production/repeatable?
    ├─ Azure-native?
    │   ├─ Prefer readability?
    │   │   └─ Bicep (recommended)
    │   └─ Need JSON?
    │       └─ ARM Templates
    └─ Multi-cloud?
        └─ Terraform
```

---

## VM SKU Selection

```
What's the workload?
├─ Database, in-memory cache?
│   └─ Memory Optimized (E, M-series)
├─ Batch processing, compute-heavy?
│   └─ Compute Optimized (F-series)
├─ Balanced workload?
│   └─ General Purpose (D-series)
├─ Variable/burstable load?
│   └─ B-series (burstable)
├─ AI/ML, rendering?
│   └─ GPU (N-series)
└─ High storage throughput?
    └─ Storage Optimized (L-series)

Need Premium Storage?
└─ Choose SKU with 's' (Ds, Es, Fs)

Need temp disk?
└─ Choose SKU with 'd' (Dsv5 has temp, Dv5 doesn't)
```

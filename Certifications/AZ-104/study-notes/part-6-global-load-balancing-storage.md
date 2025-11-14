# AZ-104 Study Notes - Part 6: Global Load Balancing & Azure Storage

## Source
John Savill AZ-104 Administrator Associate Study Cram v2 - Part 6 of 8

---

## Topics Covered

### Global Load Balancing Solutions

#### Azure Traffic Manager

- [ ] 🌐 **Traffic Manager Overview**
  - DNS-based global load balancing
  - Returns DNS records pointing to endpoints
  - Works with any service (layer 4 or layer 7)
  - Not actually in the data path (just DNS resolution)

- [ ] 🎯 **Traffic Manager Routing Methods**
  - **Performance**: Route to closest/fastest endpoint
  - **Priority**: Failover configuration (primary/secondary)
  - **Weighted**: Distribute traffic by percentage
  - **Geographic**: Route based on user's geographic location
  - **Multivalue**: Return multiple healthy endpoints
  - **Subnet**: Route based on source IP subnet

- [ ] 📍 **Traffic Manager Endpoints**
  - Azure PaaS services (Web Apps, etc.)
  - Public IP addresses (VMs, load balancers)
  - IPv4 or IPv6 addresses
  - Fully Qualified Domain Names (FQDNs)
  - Nested endpoints (another Traffic Manager profile)
  - Anything publicly accessible

- [ ] ⏱️ **Time To Live (TTL)**
  - Controls how long DNS record is cached
  - Lower TTL = faster failover, more DNS queries
  - Higher TTL = fewer DNS queries, slower failover
  - Balance between cost and responsiveness

#### Cross-Region Load Balancer

- [ ] 🌍 **Cross-Region Load Balancer**
  - Layer 4 global load balancing (TCP/UDP)
  - Public only (requires public IP)
  - Single global anycast IP address
  - Available from multiple Microsoft points of presence worldwide
  - Points to regional Standard Load Balancers

- [ ] 🎯 **Cross-Region LB Features**
  - Anycast IP (same IP accessible from multiple locations)
  - Routes clients to closest regional load balancer
  - Layer 4 only (TCP/UDP)
  - Must be public-facing
  - Backend: Regional Standard Load Balancers

- [ ] 📊 **Use Cases**
  - Public-facing services needing global availability
  - Automatic routing to closest region
  - High availability across regions

#### Azure Front Door

- [ ] 🚪 **Azure Front Door Overview**
  - Layer 7 global load balancing (HTTP/HTTPS)
  - Microsoft global network edge locations
  - Anycast IP with split TCP
  - CDN capabilities built-in
  - Public endpoints only

- [ ] ⚡ **Split TCP**
  - Client establishes TCP connection to nearest point of presence (PoP)
  - Client establishes TLS session with nearest PoP
  - PoP fetches content from backend over Microsoft backbone
  - Accelerates performance (short client connection + fast backbone)

- [ ] 💾 **Caching**
  - Optional content caching at edge locations
  - First user: Fetches from origin
  - Subsequent users: Served from cache (faster)
  - Combines load balancing + CDN functionality

- [ ] 🎯 **Azure Front Door Features**
  - URL-based routing
  - URL rewrite and redirection
  - SSL/TLS offloading
  - Cookie-based session affinity
  - WAF integration (Web Application Firewall)
  - Health probes
  - Multiple backends across regions/zones

- [ ] 🛡️ **WAF Integration**
  - Protect against OWASP Top 10
  - Microsoft-managed rule sets
  - Custom rules
  - Bot protection
  - Geo-filtering

- [ ] 📦 **Azure Front Door SKUs**
  - **Classic**: Legacy (don't use for new deployments)
  - **Standard**:
    - Path-based routing
    - Rules engine
    - Caching
    - Basic capabilities
  - **Premium**:
    - All Standard features
    - Microsoft-managed WAF rule sets
    - Bot protection
    - Private Link to backends
    - Enhanced security

- [ ] 🌐 **Backend Requirements**
  - Must be publicly accessible
  - Can be Application Gateways (common)
  - Can be VMs with public IPs
  - Can be anything with public DNS/IP
  - Across regions, availability zones
  - Not restricted to Azure (can be on-prem or other clouds)

---

## Azure Storage

### Storage Account Fundamentals

- [ ] 💾 **Storage Account Overview**
  - Regional service (lives in specific region)
  - Container for storage services
  - Unique namespace (accountname.blob.core.windows.net)
  - Multiple services in single account

- [ ] 📦 **Storage Account Performance Tiers**
  - **Standard**: HDD-based, lower cost, most common
  - **Premium**: SSD-based, higher performance, higher cost
  - Premium is service-specific (separate accounts for each)

- [ ] 🎯 **Storage Account Types**
  - **General Purpose v2**: Recommended for all scenarios
    - Supports: Blobs, Files, Queues, Tables
    - All features available
    - Best pricing
  - **General Purpose v1**: Legacy (avoid)
  - **Standard Block Blob**: Specific use case (avoid for general use)
  - **Premium Block Blob**: SSD, block blob only
  - **Premium Page Blob**: SSD, page blob only (not common)
  - **Premium File Shares**: SSD, SMB/NFS file shares

- [ ] ⚠️ **Premium File Shares Billing**
  - **Critical**: Billed on provisioned size, NOT used capacity
  - Performance scales with provisioned size
  - Create larger share for better performance
  - Pay for full size even if empty

### Storage Services

- [ ] 📦 **Blob Storage**
  - Object storage for unstructured data
  - **Block Blob**: Most common (images, videos, backups)
  - **Page Blob**: Random access (used for VM disks, but now Managed Disks preferred)
  - **Append Blob**: Append-only (logging scenarios)

- [ ] 📁 **Azure Files**
  - SMB and NFS file shares
  - Mount like network drive
  - Supports SMB 2.1, 3.0, 3.1.1
  - NFS 4.1 (premium only)
  - Entra ID integration for authentication

- [ ] 📊 **Table Storage**
  - NoSQL key-value store
  - Schema-less (no fixed schema)
  - Partition key + Row key
  - Add any properties/attributes
  - Flexible, lightweight database

- [ ] 📬 **Queue Storage**
  - Message queue service
  - FIFO (First In, First Out)
  - Decouple application components
  - Up to 64 KB per message
  - Millions of messages

### Storage Redundancy

- [ ] 🔄 **Locally Redundant Storage (LRS)**
  - 3 copies within single storage cluster
  - Same datacenter/availability zone
  - Protects against drive/rack failures
  - Lowest cost
  - **No protection against datacenter failure**

- [ ] 🏢 **Zone Redundant Storage (ZRS)**
  - 3 copies across 3 availability zones
  - Same region, different zones
  - Protects against datacenter failure
  - Only in regions with availability zones
  - Higher cost than LRS

- [ ] 🌍 **Geo-Redundant Storage (GRS)**
  - 3 copies in primary region (single cluster - LRS)
  - 3 copies in paired secondary region (single cluster - LRS)
  - **Total: 6 copies**
  - Asynchronous replication to secondary
  - Protects against regional failure

- [ ] 🌐 **Geo-Zone-Redundant Storage (GZRS)**
  - 3 copies across 3 zones in primary region (ZRS)
  - 3 copies in paired secondary region (single cluster - LRS)
  - **Total: 6 copies**
  - Asynchronous replication to secondary
  - Best of both: zone redundancy + geo redundancy

- [ ] 👁️ **Read Access Geo-Redundant (RA-GRS)**
  - Same as GRS + read access to secondary region
  - Can read from replica even when primary is available
  - Load balancing read operations
  - Secondary endpoint: accountname-secondary.blob.core.windows.net

- [ ] 🔍 **Read Access Geo-Zone-Redundant (RA-GZRS)**
  - Same as GZRS + read access to secondary region
  - Highest durability and availability
  - Can read from secondary while primary is up

- [ ] ⚠️ **Asynchronous Replication**
  - Primary → Secondary is always asynchronous
  - Synchronous would impact performance (high latency between regions)
  - Write acknowledged as soon as durable in primary
  - May lose recent data if primary fails before replication
  - Check last sync time before failover

- [ ] 🔄 **Failover Capabilities**
  - Can manually trigger failover to secondary
  - Check last sync time (shows replication lag)
  - Potential data loss for data written after last sync
  - Secondary becomes new primary

### Blob Access Tiers

- [ ] 🔥 **Hot Tier**
  - Frequently accessed data
  - **Highest storage cost, lowest access cost**
  - Default tier for new blobs
  - Optimized for frequent reads/writes
  - No minimum storage duration

- [ ] ❄️ **Cool Tier**
  - Infrequently accessed data
  - **Lower storage cost, higher access cost than Hot**
  - Data accessed occasionally
  - **Minimum 30 days storage** (early deletion fee)
  - Good for short-term backup, disaster recovery

- [ ] 🧊 **Cold Tier**
  - Rarely accessed data
  - **Even lower storage cost, higher access cost than Cool**
  - Data accessed very infrequently
  - **Minimum 90 days storage** (early deletion fee)
  - Must be online (instant access)

- [ ] 📦 **Archive Tier**
  - Long-term archival data
  - **Lowest storage cost, highest access cost**
  - **Offline storage** - cannot access directly
  - Must rehydrate to another tier before reading
  - Rehydration takes hours (up to 15 hours)
  - **Minimum 180 days storage** (early deletion fee)
  - Compliance, long-term retention

- [ ] ⏱️ **Early Deletion Fees**
  - **Cool**: 30 days minimum
  - **Cold**: 90 days minimum
  - **Archive**: 180 days minimum
  - Deleted before minimum = charged for remaining days

- [ ] 🎯 **Tier Selection Strategy**
  - Hot: Constant interaction (website images, active databases)
  - Cool: Occasional access (monthly reports, recent backups)
  - Cold: Rare access but instant availability needed (quarterly data)
  - Archive: Long-term retention, can wait hours (compliance data)

- [ ] 🔄 **Tier Management**
  - Set at blob level (not container level)
  - Can mix tiers in same container
  - Manually change tier via portal, PowerShell, CLI
  - Archive blobs cannot be accessed (download greyed out)
  - Must change tier to read archived blob

### Lifecycle Management

- [ ] 📅 **Lifecycle Management Overview**
  - Automate tier transitions
  - Automatically delete old data
  - Rule-based policies
  - Save money by optimizing storage costs

- [ ] 🎯 **Lifecycle Rules**
  - Based on last modified date
  - Based on last accessed date (requires access tracking)
  - Based on creation date
  - Filter by blob name prefix
  - Filter by blob index tags

- [ ] 🔀 **Lifecycle Actions**
  - Move to Cool tier after X days
  - Move to Cold tier after X days
  - Move to Archive tier after X days
  - Delete blob after X days
  - Example: Hot → Cool (15 days) → Cold (45 days) → Archive (135 days)

- [ ] ⚠️ **Respect Minimum Durations**
  - Design rules to avoid early deletion fees
  - Cool: Must stay 30 days before moving
  - Cold: Must stay 90 days before moving
  - Archive: Must stay 180 days before deleting

### Object Replication

- [ ] 🔄 **Object Replication Overview**
  - Replicate blobs between storage accounts
  - Source and destination can be in any region
  - Not limited to paired regions (unlike GRS)
  - Container-level replication
  - Asynchronous replication

- [ ] 🎯 **Object Replication Use Cases**
  - Replicate to non-paired region
  - Compliance requirements for specific regions
  - Minimize latency (replicate closer to users)
  - Multiple read replicas in different regions

- [ ] ⚙️ **Object Replication Configuration**
  - Choose source storage account and container
  - Choose destination storage account and container
  - Specify replication rules
  - Optional filters (blob name prefix, etc.)
  - Multiple replication policies possible

- [ ] 📊 **Comparison: GRS vs Object Replication**
  - **GRS**: Automatic, to paired region only, entire account
  - **Object Replication**: Manual config, any region, specific containers

### Storage Tools & Migration

- [ ] 🖥️ **Azure Portal**
  - Web-based management
  - Storage Browser integrated
  - Upload/download files
  - Configure settings
  - View metrics

- [ ] 📦 **Azure Storage Explorer**
  - Desktop application (Windows, Mac, Linux)
  - Manage multiple accounts
  - Upload/download blobs
  - Create/delete containers, file shares, queues, tables
  - Add queue messages, table entities
  - Cross-platform
  - Free tool

- [ ] ⚡ **AzCopy**
  - Command-line tool
  - High-performance data transfer
  - Upload, download, copy between accounts
  - **Server-side asynchronous copy**: Copy between storage accounts without downloading to client
  - Sync capabilities
  - Resume failed transfers
  - Supports SAS tokens, Entra authentication

- [ ] 📦 **Azure Data Box**
  - Physical device for large data transfers
  - Avoid network bandwidth limits
  - **Data Box Disk**: 8 TB SSDs, mail to Microsoft
  - **Data Box**: 80 TB device, ship to Microsoft
  - **Data Box Heavy**: 800 TB device
  - Microsoft imports data to your storage account
  - Can also export data from Azure

- [ ] 🏭 **Azure Data Factory**
  - Cloud ETL service
  - Orchestrate data movement
  - Create data pipelines
  - Transform data
  - Schedule and monitor
  - Large-scale migrations

- [ ] 🐧 **Blob Fuse**
  - Mount blob storage as Linux file system
  - Access blobs via standard file operations
  - Linux only
  - Virtual file system

### Storage Security & Features

- [ ] 🔒 **Storage Account Firewall**
  - Restrict access to specific networks
  - Allow specific VNets/subnets (service endpoints)
  - Private endpoints
  - Allow specific public IP ranges
  - Trusted Azure services can bypass

- [ ] 📂 **Hierarchical Namespace**
  - Enable Azure Data Lake Storage Gen2
  - POSIX-style ACLs
  - Directory-level operations
  - Better performance for big data analytics
  - Organize blobs in directory structure

- [ ] 🔄 **Versioning**
  - Automatic versioning of blobs
  - Every overwrite creates new version
  - Restore previous versions
  - Protection against accidental deletion/modification

- [ ] 🗑️ **Soft Delete**
  - Recover deleted blobs
  - Recover deleted containers
  - Recover deleted file shares
  - Retention period: 1-365 days
  - Blobs: 1-365 days retention
  - File shares: 1-365 days retention (default 14 days shown)

- [ ] 📊 **Change Feed**
  - Log of all changes to blobs
  - Ordered, guaranteed log
  - Process changes in order
  - Audit trail
  - Event-driven scenarios

- [ ] 📸 **Snapshots (File Shares)**
  - Point-in-time copies
  - Incremental (only changed data)
  - Restore individual files or entire share
  - Can configure backup vault to orchestrate

- [ ] 🔐 **Entra ID Integration**
  - Authenticate with Entra identities
  - RBAC for data plane access
  - Available for Blobs and Files
  - Fine-grained permissions

---

## Key Takeaways

1. **Traffic Manager** - DNS-based, works with anything, routing methods (performance/priority/weighted)
2. **Cross-Region LB** - Layer 4, anycast IP, points to regional Standard LBs
3. **Azure Front Door** - Layer 7, split TCP, caching, WAF, public only
4. **General Purpose v2** - Always use for new storage accounts
5. **Premium Files Billing** - Charged on provisioned size, NOT used capacity
6. **Storage Redundancy** - LRS (3 local) → ZRS (3 zones) → GRS (6, two regions) → GZRS (best)
7. **Blob Tiers** - Hot (frequent), Cool (30 days min), Cold (90 days min), Archive (180 days min, offline)
8. **Early Deletion Fees** - Cool 30d, Cold 90d, Archive 180d
9. **Lifecycle Management** - Automate tier transitions, respect minimums
10. **Object Replication** - Any region (not just paired), container-level
11. **AzCopy** - Server-side async copy between storage accounts
12. **Data Box** - Physical devices for large migrations

---

## Exam Tips

- **Traffic Manager**: DNS-based, not in data path, supports nested endpoints
- **Front Door Split TCP**: Client connects to edge, edge fetches from backend
- **General Purpose v1**: Don't use (legacy)
- **Premium Files**: Remember provisioned billing (not consumption)
- **Blob Types**: Block (most common), Page (VMs), Append (logs)
- **LRS**: 3 copies in same cluster (no datacenter failure protection)
- **ZRS**: 3 copies across zones (only in zones-enabled regions)
- **GRS/GZRS**: Asynchronous replication (potential data loss on failover)
- **RA-GRS/RA-GZRS**: Can read from secondary even when primary is up
- **Hot Tier**: High storage cost, low access cost (remember the tradeoff!)
- **Archive**: Offline, cannot access without rehydration (hours)
- **Early Deletion**: Cool 30d, Cold 90d, Archive 180d (common exam question)
- **Lifecycle Management**: Respect minimum durations to avoid fees
- **Object Replication**: Not limited to paired regions (unlike GRS)
- **AzCopy**: Server-side async copy (doesn't download to client)
- **Soft Delete**: 1-365 days retention for blobs and file shares

---

## Portal Navigation

- **Traffic Manager**: Search "Traffic Manager profiles" → Create → Configure routing
- **Front Door**: Search "Front Door and CDN profiles" → Create → Choose SKU
- **Storage Account**: Search "Storage accounts" → Create → General Purpose v2, Standard
- **Blob Tiers**: Storage account → Containers → Select blob → Change tier
- **Lifecycle Management**: Storage account → Lifecycle management → Add rule
- **Object Replication**: Storage account → Object replication → Add replication policy
- **Redundancy**: Storage account → Data management → Redundancy → Change
- **Failover**: Storage account → Geo-replication → Prepare for failover
- **Soft Delete**: Storage account → Data protection → Enable soft delete

---

## Routing Method Selection (Traffic Manager)

| Routing Method | Use Case |
|----------------|----------|
| **Performance** | Route users to closest/fastest endpoint |
| **Priority** | Active/passive failover scenario |
| **Weighted** | Distribute traffic by percentage (A/B testing, gradual rollout) |
| **Geographic** | Compliance, data sovereignty, specific region serving |
| **Multivalue** | Return multiple healthy endpoints (client chooses) |
| **Subnet** | Different endpoints based on source IP ranges |

---

## Storage Redundancy Comparison

| Type | Copies | Primary | Secondary | Protection | Cost |
|------|--------|---------|-----------|------------|------|
| **LRS** | 3 | Single cluster | None | Drive/rack failure | Lowest |
| **ZRS** | 3 | 3 zones | None | Datacenter failure | Low-Medium |
| **GRS** | 6 | Single cluster | Single cluster (paired region) | Regional failure | Medium |
| **GZRS** | 6 | 3 zones | Single cluster (paired region) | Zone + regional | Highest |
| **RA-GRS** | 6 | Single cluster | Single cluster + read access | Regional + read HA | Medium-High |
| **RA-GZRS** | 6 | 3 zones | Single cluster + read access | Zone + regional + read HA | Highest |

---

## Blob Tier Comparison

| Tier | Storage Cost | Access Cost | Availability | Minimum Duration | Use Case |
|------|--------------|-------------|--------------|------------------|----------|
| **Hot** | Highest | Lowest | Online | None | Frequently accessed |
| **Cool** | Lower | Higher | Online | 30 days | Infrequently accessed |
| **Cold** | Even Lower | Even Higher | Online | 90 days | Rarely accessed, instant access |
| **Archive** | Lowest | Highest | **Offline** | 180 days | Long-term, can wait hours |

---

## PowerShell/CLI Commands

```powershell
# Traffic Manager
New-AzTrafficManagerProfile -Name TM -ResourceGroupName RG -RoutingMethod Performance

# Storage Account
New-AzStorageAccount -ResourceGroupName RG -Name mystorageacct -SkuName Standard_GRS

# Blob Tier
Set-AzStorageBlobContent -Container mycontainer -File myfile.txt -StandardBlobTier Cool

# Lifecycle Management
Add-AzStorageAccountManagementPolicyAction -Action TierToCool -DaysAfterModificationGreaterThan 30

# AzCopy
azcopy copy 'source' 'destination' --recursive
azcopy sync 'source' 'destination'

# Failover
Invoke-AzStorageAccountFailover -ResourceGroupName RG -Name mystorageacct
```

---

## Decision Trees

### Global Load Balancing Selection

```
What layer?
├─ Layer 4 (TCP/UDP)
│   └─ Cross-Region Load Balancer
├─ Layer 7 (HTTP/HTTPS)
│   ├─ Need caching/WAF/split TCP?
│   │   └─ Azure Front Door
│   └─ Simple DNS routing?
│       └─ Traffic Manager
└─ Just DNS-based?
    └─ Traffic Manager (works with any layer)
```

### Storage Redundancy Selection

```
Need regional protection?
├─ Yes
│   ├─ Need zone redundancy in primary?
│   │   ├─ Yes → GZRS or RA-GZRS
│   │   └─ No → GRS or RA-GRS
│   └─ Need read access to secondary?
│       └─ Yes → RA-GRS or RA-GZRS
├─ No (single region)
│   ├─ Region has availability zones?
│   │   ├─ Yes → ZRS
│   │   └─ No → LRS
│   └─ Need lowest cost?
│       └─ LRS
└─ Budget?
    └─ Lowest cost → LRS
```

### Blob Tier Selection

```
How often accessed?
├─ Constantly (daily)
│   └─ Hot
├─ Occasionally (weekly/monthly)
│   └─ Cool
├─ Rarely (quarterly)
│   └─ Cold
└─ Archival (yearly or compliance)
    └─ Archive

Can you wait hours for access?
├─ No → Use Cold (not Archive)
└─ Yes → Archive (if long-term)
```

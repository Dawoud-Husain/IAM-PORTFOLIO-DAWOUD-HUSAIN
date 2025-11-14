# AZ-104 Study Notes - Part 8: VM Advanced, Containers, Monitoring & Network Troubleshooting (FINAL)

## Source
John Savill AZ-104 Administrator Associate Study Cram v2 - Part 8 of 8 (FINAL)

---

## Topics Covered

### VM Disks & Extensions

#### Ephemeral OS Disks

- [ ] ⚡ **Ephemeral OS Disk**
  - OS runs on host's local storage (not managed disk)
  - **Much cheaper** - no managed disk cost
  - **Very low latency** - local to host
  - **Non-persistent** - lost when VM deallocated
  - Use when VM state doesn't matter (can recreate easily)

- [ ] 🎯 **Ephemeral Use Cases**
  - Virtual Machine Scale Sets (VMSS)
  - AKS node pools
  - Stateless applications
  - VMs created/deleted frequently
  - Dev/test environments where state doesn't matter

- [ ] ⚠️ **Ephemeral Requirements**
  - VM must support it (check SKU properties)
  - Used for temp or caching purposes
  - VM SKU must have temp disk (D-variant)
  - Faster deployment than managed disks

#### Temp Disks

- [ ] 💾 **Temp Disk**
  - Local storage on physical host
  - **Drive D:** on Windows
  - **/dev/sdb** on Linux
  - Only on D-variant VMs (e.g., Ds_v5 has temp, D_v5 doesn't)
  - Not persistent (lost on VM moves/deallocations)
  - Use for: Page files, temp data, caching

- [ ] 📀 **Data Disk Caching**
  - Optional caching for data disks
  - Uses host's local storage
  - Options: None, ReadOnly, ReadWrite
  - Improves read performance
  - Configure per disk

#### VM Extensions

- [ ] 🔧 **VM Extensions**
  - Add capabilities to VMs
  - Post-deployment configuration
  - Run scripts, install software
  - Managed by Azure

- [ ] 📋 **Common Extensions**
  - **Custom Script Extension**: Run PowerShell or Bash scripts
  - **Azure Backup Extension**: Enable backup
  - **Azure Monitor Agent**: Collect metrics and logs
  - **Desired State Configuration (DSC)**: Configuration management
  - **Antimalware Extension**: Malware protection
  - **Diagnostic Extension**: Collect diagnostics data

- [ ] ⚙️ **Run Command**
  - Execute individual commands on VM
  - Ad-hoc operations
  - No need for full script extension
  - Portal, CLI, PowerShell

- [ ] 🕐 **Auto-Shutdown**
  - Automatically shut down VM on schedule
  - Save costs (pay per second)
  - Useful for dev/test environments
  - Can configure notifications

- [ ] 🔐 **Managed Identities**
  - VM can authenticate to Azure services
  - No credentials in code
  - System-assigned or user-assigned
  - RBAC for access control

### Backup & Disaster Recovery

#### Backup Vault Types

- [ ] 🏦 **Recovery Services Vault**
  - Legacy/traditional vault
  - Supports: VMs, SQL in VMs, SAP HANA, Azure Files
  - Older services use this

- [ ] 📦 **Backup Vault**
  - Newer vault type
  - Supports: Azure Disks, Blobs, PostgreSQL, PostgreSQL Flexible, AKS
  - Use for newer Azure services

- [ ] 🎯 **Backup Center**
  - Central management for all backups
  - Configure backups across vaults
  - Monitor backup health
  - Restore operations
  - Works with both vault types

### High Availability - Availability Sets vs Zones

#### Availability Sets

- [ ] 🏢 **Availability Set**
  - Distribute VMs across fault domains (racks)
  - Within single datacenter
  - Protects against rack-level failures
  - **Fault Domains**: Different racks (power, network isolation)
  - **Update Domains**: VMs updated in groups (not all at once)

- [ ] ⚠️ **Availability Set Best Practices**
  - Never mix different workloads in same availability set
  - Create separate availability set per application tier
  - Example: Web tier in AvSet1, DB tier in AvSet2
  - Round-robin distribution across fault domains
  - If mixed: Risk all of one service in one rack

- [ ] 📊 **Fault Domains**
  - Separate racks within datacenter
  - Different power, cooling, networking
  - Azure distributes VMs across fault domains
  - Typical: 2-3 fault domains per availability set

#### Availability Zones

- [ ] 🌐 **Availability Zones (Preferred)**
  - Separate datacenters within region
  - Isolate power, cooling, networking, control plane
  - Different fabric controllers
  - Much larger blast radius protection
  - **Always prefer over Availability Sets** (if region supports)

- [ ] ✅ **Availability Zone Advantages**
  - Datacenter-level failure protection
  - Higher SLA than Availability Sets
  - Better than rack-level protection
  - Entire power substation can fail, others still OK

- [ ] 🎯 **AvSet vs AvZone Decision**
  - Availability Zones: If region supports (best option)
  - Availability Sets: If region doesn't support zones
  - Zones > Sets (always prefer zones)

### Virtual Machine Scale Sets (VMSS)

#### Scale Set Types

- [ ] 📊 **Uniform Scale Sets (Traditional)**
  - All VMs identical
  - Created from template
  - Traditional autoscaling
  - Less flexible
  - Legacy option

- [ ] 🎯 **Flexible Scale Sets (Recommended)**
  - Can have different VM SKUs
  - Mix spot and regular VMs
  - Add individual VMs manually
  - Optional scaling profile
  - More control and flexibility
  - Recommended for new deployments

#### Scaling Concepts

- [ ] 📈 **Horizontal Autoscaling**
  - Add/remove VM instances
  - Based on load/metrics
  - Scale out (add) / Scale in (remove)
  - Preferred scaling method

- [ ] 📏 **Vertical Scaling**
  - Change VM size (bigger/smaller)
  - Requires VM restart
  - Downtime during resize
  - Not practical for most scenarios

- [ ] ⚙️ **Scaling Profile**
  - VM template (SKU, image, config)
  - Minimum instance count
  - Maximum instance count
  - Scale rules (when to add/remove)

- [ ] 📋 **Scale Rules**
  - Metric-based: CPU > 70% → Add 2 instances
  - Metric-based: CPU < 30% → Remove 1 instance
  - Time-based: Scale up during business hours
  - Custom metrics: Application-specific

- [ ] 💰 **Spot Instances**
  - Use Azure's spare capacity
  - Up to 90% cheaper
  - Can be evicted anytime
  - Use for: Batch jobs, dev/test, interruptible workloads
  - Flexible Scale Sets can mix spot and regular

### Containers

#### Container Basics

- [ ] 📦 **Container Fundamentals**
  - Virtualize OS (not hardware)
  - Share kernel, isolated at user mode
  - Lightweight compared to VMs
  - Fast startup
  - Portable

- [ ] 🏗️ **Container Components**
  - **Base Image**: Starting point (e.g., Ubuntu, Alpine, .NET)
  - **Dockerfile**: Instructions to build custom image
  - **Custom Image**: Your application image
  - **Container Registry**: Store images
  - **Container Host**: Runs containers

- [ ] 🔧 **Dockerfile**
  - Text file with build instructions
  - `FROM`: Base image
  - `RUN`: Execute commands
  - `COPY`: Add files
  - `CMD`: Default command to run
  - Creates layered image

#### Azure Container Services

- [ ] 📦 **Azure Container Registry (ACR)**
  - Private Docker registry
  - Store container images
  - Geo-replication
  - Image scanning
  - Webhooks for automation

- [ ] 🎯 **Azure Container Instances (ACI)**
  - Simplest way to run containers
  - Serverless containers
  - Pay per second
  - Fast startup
  - No orchestration
  - Good for: Burst scenarios, individual containers, simple apps

- [ ] 📊 **Container Groups (ACI)**
  - Multiple containers on same host
  - Share networking and storage
  - Co-located containers
  - Example: App + sidecar logging container

#### Azure Kubernetes Service (AKS)

- [ ] ☸️ **AKS Overview**
  - Managed Kubernetes orchestrator
  - For complex container deployments
  - Automatic scaling, healing, updates
  - Rich networking and storage

- [ ] 🎛️ **AKS Components**
  - **Control Plane**: API server, scheduler, etcd (managed by Azure, free)
  - **Node Pools**: Groups of VMs running containers (you pay for VMs)
  - **Nodes**: Individual VMs in pool (built on VMSS)
  - **Pods**: Smallest deployable unit, runs containers
  - **Kubelet**: Agent on each node, talks to control plane

- [ ] 💾 **AKS Storage**
  - **Persistent Volumes**: Durable storage
  - **Persistent Volume Claims**: Request for storage
  - Backed by: Azure Files, Azure Disks, Azure NetApp Files, Azure Container Storage, Elastic SAN

- [ ] 🌐 **AKS Networking Models**
  - **Kubenet**: Separate IP space, NAT required, less flexible (becoming unpopular)
  - **Azure CNI**: Pods use same IP space as nodes, can exhaust IPs
  - **Azure CNI Dynamic**: Pods use different subnet, allocated in batches of 16
  - **Azure CNI Overlay (Recommended)**: Separate CIDR for pods, native plumbing, no NAT issues

- [ ] 📈 **AKS Autoscaling**
  - **Horizontal Pod Autoscaler (HPA)**: Scale pods based on metrics
  - **KEDA**: Event-driven autoscaling, more functions/triggers
  - **Cluster Autoscaler**: Add/remove nodes when pods need capacity

- [ ] ⚡ **AKS Burst to ACI**
  - Virtual Kubelet represents ACI
  - Burst pods to Azure Container Instances
  - Short-term capacity expansion
  - Serverless burst capacity

### Platform as a Service (PaaS)

#### App Service

- [ ] 🌐 **App Service Overview**
  - Fully managed web app hosting
  - Windows or Linux
  - Multiple runtime stacks (.NET, Java, Node.js, Python, PHP, Ruby)
  - Built-in scaling
  - DevOps integration

- [ ] 📦 **App Service Plan**
  - Defines compute resources
  - Multiple apps share same plan (same worker nodes)
  - Different pricing tiers
  - Regional (apps in plan in same region)

- [ ] 💰 **App Service Pricing Tiers**
  - **Free/Shared**: Shared infrastructure, limited features
  - **Basic**: Dedicated VMs, custom domains, no auto-scale
  - **Standard**: Auto-scale, staging slots, backups, VNet integration
  - **Premium**: Enhanced performance, more scale, zone redundancy
  - **Isolated**: App Service Environment (ASE), VNet injection

- [ ] 🔄 **Deployment Slots**
  - Staging and production environments
  - Swap between slots (blue-green deployment)
  - Test before production
  - Available in Standard tier and above

- [ ] ⚙️ **Scaling Options**
  - **Manual**: Set instance count
  - **Rules-based**: CPU/Memory thresholds
  - **Elastic**: Automatic based on HTTP load, pre-warmed instances

- [ ] 🚀 **Deployment Methods**
  - Azure DevOps pipelines
  - GitHub Actions
  - FTP/FTPS
  - ZIP/WAR deploy
  - Git (local or external)
  - Docker containers

- [ ] 🏢 **App Service Environment (ASE)**
  - Fully isolated and dedicated
  - Deploys into your VNet
  - No shared components
  - High scale (100 instances)
  - Higher cost

---

## Monitoring & Observability

### Azure Monitor Components

#### Activity Log

- [ ] 📋 **Activity Log**
  - Subscription-level control plane events
  - Who did what, when
  - Resource creation, deletion, configuration changes
  - **Free** - no configuration needed
  - 90-day retention by default
  - Can export to storage/Log Analytics for longer retention

- [ ] 🔍 **Activity Log Filtering**
  - View at subscription, resource group, or resource level
  - Filter by: Timespan, Event severity, Operation, Resource type
  - Useful for auditing and troubleshooting

#### Metrics

- [ ] 📊 **Azure Monitor Metrics**
  - Time-based numerical data
  - **Free** - automatically collected
  - Host metrics (CPU, disk, network)
  - Resource-specific metrics
  - Near real-time (1-minute granularity)
  - Stored for 93 days

- [ ] 🎯 **Metric Features**
  - Aggregation: Average, Min, Max, Sum, Count
  - Splitting: Split by dimension (e.g., per LUN, per instance)
  - Filtering: Filter to specific values
  - Real-time charts in portal

- [ ] 💻 **Guest Metrics**
  - Metrics from inside VM
  - Requires Azure Monitor Agent
  - Custom performance counters
  - Application-specific metrics

#### Logs & Diagnostic Settings

- [ ] 📝 **Resource Logs**
  - Detailed operational data
  - **NOT enabled by default**
  - Must configure Diagnostic Settings
  - Choose log categories to capture
  - More detailed than Activity Log

- [ ] ⚙️ **Diagnostic Settings**
  - Configure per resource
  - Choose logs and metrics to capture
  - Choose destination(s):
    - **Storage Account**: Cheapest, least useful
    - **Event Hub**: External SIEM integration
    - **Log Analytics Workspace**: Analytics and alerting (best option)

- [ ] 🎯 **Diagnostic Setting Destinations**
  - Can send to multiple destinations simultaneously
  - Storage: Archive, compliance
  - Event Hub: Real-time streaming to external systems
  - Log Analytics: Query, analyze, alert

### Log Analytics Workspace

#### Workspace Types

- [ ] 📊 **Analytics Logs (Traditional)**
  - Full KQL query capabilities
  - Ingestion cost + storage cost
  - **Included retention**: 30 days (or 90 with Sentinel)
  - Beyond retention: Pay for storage (up to 2 years interactive)
  - Can run queries without additional cost
  - Can create alerts

- [ ] 💰 **Basic Logs**
  - Cheaper ingestion cost
  - **Limited KQL** (subset of capabilities)
  - **8 days retention only**
  - **Pay per query** (no free queries)
  - No cross-table queries
  - After 8 days → Archive (if configured)
  - Use for: High-volume, low-value logs

- [ ] 📦 **Archive Logs**
  - Long-term retention (up to 12 years)
  - Very cheap storage
  - **No direct queries**
  - Must restore or search to use
  - **Search Job**: Run query, pay for compute
  - **Restore Job**: Bring data back to interactive, pay for compute

- [ ] 🎯 **Log Type Selection**
  - Analytics: Important logs, need alerting, need full KQL
  - Basic: High-volume logs (e.g., container logs with v2 schema)
  - Archive: Compliance, long-term retention

#### Data Collection

- [ ] 🔧 **Azure Monitor Agent**
  - Modern agent for VMs
  - Replaces: Log Analytics agent, Diagnostic extension
  - Collect: Performance counters, Event logs, Syslog
  - Configure via Data Collection Rules

- [ ] 📋 **Data Collection Rules (DCR)**
  - Define what data to collect
  - Define where to send data
  - Can filter and transform data
  - Associate with VMs or Scale Sets

- [ ] 🔍 **KQL (Kusto Query Language)**
  - Query language for Log Analytics
  - Pipe-based syntax
  - Powerful filtering, aggregation, visualization
  - Example: `SecurityEvent | where TimeGenerated > ago(1h)`

### Application Insights

- [ ] 📱 **Application Insights**
  - Application performance monitoring (APM)
  - Built on Log Analytics
  - Auto-instrumentation for many platforms
  - Detect performance issues and failures

- [ ] 🧪 **Synthetic Monitoring**
  - Create synthetic transactions
  - Test application availability
  - Test from multiple locations worldwide
  - Alert if tests fail

- [ ] 📊 **Application Insights Features**
  - Request rates, response times, failure rates
  - Dependency tracking
  - Exception tracking
  - Custom events and metrics
  - User behavior analytics

### Alerts & Actions

#### Alert Rules

- [ ] 🚨 **Alert Sources**
  - Metric alerts (e.g., CPU > 80%)
  - Log query alerts (KQL query returns results)
  - Activity Log alerts (control plane events)
  - Service Health alerts (Azure incidents)
  - Resource Health alerts (resource availability)

- [ ] 🎯 **Alert Types**
  - **Static threshold**: Fixed value (CPU > 70%)
  - **Dynamic threshold**: Machine learning baseline
    - Sensitivity: Low, Medium, High
    - Detects anomalies from normal patterns

- [ ] ⚙️ **Alert Configuration**
  - Scope: Which resources
  - Condition: What to monitor
  - Severity: 0 (Critical) to 4 (Informational)
  - Action: What to do when alert fires

#### Action Groups

- [ ] 📞 **Action Group**
  - Define actions to take when alert fires
  - Reusable across multiple alerts
  - Multiple action types in one group

- [ ] 🔔 **Action Types**
  - **Email/SMS/Push**: Notify people
  - **Voice**: Phone call
  - **Azure Function**: Run custom code
  - **Logic App**: Complex workflows
  - **Webhook**: Call external HTTPS endpoint
  - **Secure Webhook**: Authenticated webhook
  - **Automation Runbook**: Run PowerShell
  - **ITSM**: Create ticket in ServiceNow, etc.
  - **Azure Role**: Assign RBAC role

#### Alert Processing Rules

- [ ] 🎛️ **Alert Processing Rules**
  - Act on alerts AFTER they're raised
  - Cleaner than configuring action groups on every alert
  - Two purposes:
    1. **Apply Action Groups**: Call action group based on rules
    2. **Suppress Alerts**: Don't notify during maintenance, off-hours

- [ ] ⏰ **Suppression Use Cases**
  - Maintenance windows
  - Weekends/holidays
  - Out of business hours for non-critical alerts
  - Only page for Severity 0-1, email for Severity 2-4

- [ ] 🎯 **Processing Rule Filters**
  - Resource type
  - Resource group
  - Subscription
  - Alert severity
  - Alert monitor service
  - Time-based schedules

---

## Network Troubleshooting

### Network Watcher

- [ ] 🔍 **Network Watcher Overview**
  - Network monitoring and diagnostics
  - Regional service (enable per region)
  - Troubleshoot connectivity issues
  - Diagnose network problems

- [ ] 🗺️ **Topology**
  - Visual representation of VNet
  - See resources and connections
  - Download topology diagram

- [ ] ✅ **IP Flow Verify**
  - Test if traffic is allowed or denied
  - Specify: Source IP, Dest IP, Port, Protocol
  - Shows which NSG rule allows/denies
  - Quick troubleshooting for NSG issues

- [ ] 🧭 **Next Hop**
  - Shows routing decision for traffic
  - Where would packet go from this VM?
  - See: Next hop type and IP
  - Types: Internet, VirtualAppliance, VNet, None

- [ ] 🔧 **VPN Troubleshoot**
  - Diagnose VPN Gateway issues
  - Check connections
  - Generate diagnostics

- [ ] 📊 **NSG Flow Logs**
  - Log IP traffic through NSGs
  - See allowed and denied flows
  - Source/dest IP, port, protocol, decision
  - Store in Storage Account
  - Analyze with Traffic Analytics

- [ ] 📈 **Traffic Analytics**
  - Process NSG Flow Logs
  - Visualize traffic patterns
  - Top talkers, geo map
  - Security insights

- [ ] 📦 **Packet Capture**
  - Capture network packets on VM
  - Requires extension on VM
  - Download PCAP file
  - Analyze with Wireshark
  - Time-limited captures

- [ ] 🧪 **Connection Troubleshoot**
  - Test connectivity from VM to endpoint
  - Uses extension on source VM
  - Synthetic test packet
  - Shows: Reachable, latency, hops

- [ ] 🔐 **NSG Diagnostics**
  - Comprehensive NSG evaluation
  - What rules apply to this flow?
  - Effective security rules
  - Quick "what-if" analysis

---

## Key Takeaways

1. **Ephemeral Disks** - Cheaper, faster, but non-persistent (VMSS, AKS)
2. **Temp Disk** - Drive D: (Windows), /dev/sdb (Linux), only on D-variant VMs
3. **Availability Zones > Availability Sets** - Datacenter-level protection vs rack-level
4. **Never Mix Workloads** - Separate availability sets per application tier
5. **Flexible VMSS** - Recommended over Uniform (more flexible)
6. **Spot Instances** - Up to 90% cheaper, can be evicted (batch/dev/test)
7. **AKS Networking** - Overlay is recommended (over Kubenet, CNI)
8. **Container vs VM** - Containers share kernel, VMs have separate OS
9. **Diagnostic Settings** - NOT enabled by default, must configure
10. **Log Analytics Types** - Analytics (full KQL), Basic (8 days, pay per query), Archive (12 years, restore to query)
11. **Alert Processing Rules** - Apply actions or suppress based on conditions
12. **Action Groups** - Reusable notification/action definitions
13. **Network Watcher** - IP Flow Verify, Next Hop, Connection Troubleshoot
14. **Backup Vaults** - Recovery Services (legacy), Backup Vault (newer services)

---

## Exam Tips

- **Ephemeral OS Disk**: Requires D-variant VM, lost on deallocation, cheapest option
- **Temp Disk Location**: D: (Windows), /dev/sdb (Linux)
- **Availability Sets**: Never mix workloads, create separate AvSet per tier
- **Availability Zones**: Always prefer over AvSets if region supports (higher SLA)
- **Fault Domains**: Racks within datacenter (AvSets)
- **VMSS Types**: Flexible = recommended, Uniform = legacy
- **Spot Instances**: Evictable, 90% cheaper, good for batch/interruptible
- **Container Registry**: ACR = private registry for Docker images
- **ACI vs AKS**: ACI = simple, serverless; AKS = orchestration, complex apps
- **AKS Control Plane**: Free, managed by Azure
- **AKS Networking**: Overlay recommended (not Kubenet)
- **App Service Plan**: Multiple apps share worker nodes
- **Deployment Slots**: Standard tier minimum, swap staging to production
- **Activity Log**: Free, 90 days, control plane events
- **Metrics**: Free, automatically collected, 93 days retention
- **Diagnostic Settings**: NOT default, must enable, choose destination
- **Log Analytics Types**: Analytics (30-90d), Basic (8d), Archive (12y)
- **Basic Logs**: Pay per query, limited KQL, 8 days max
- **Archive**: Must restore or run search job to query (costs $)
- **Alert Processing Rules**: Apply actions OR suppress (two different purposes)
- **Action Groups**: Reusable, multiple action types
- **IP Flow Verify**: Test if NSG allows/denies traffic
- **Connection Troubleshoot**: Requires extension on VM, synthetic test
- **Recovery Services Vault**: VMs, SQL, SAP HANA, Azure Files
- **Backup Vault**: Disks, Blobs, PostgreSQL, AKS

---

## Exam Preparation Tips

### Before the Exam

1. ✅ **Complete Hands-On Labs**
   - Microsoft Learn modules (free Azure sandbox)
   - Create trial subscription and practice
   - Focus on: VMs, Storage, Networking, Monitoring

2. ✅ **Understand, Don't Memorize**
   - Know WHY, not just WHAT
   - Understand decision criteria
   - Practice scenario-based thinking

3. ✅ **Focus on Weak Areas**
   - Review score report if retaking
   - Double down on weak domains
   - Use these study notes to target gaps

4. ✅ **Practice Time Management**
   - 100-120 minutes for exam
   - 40-60 questions typical
   - ~2 minutes per question
   - Flag and skip difficult ones

### During the Exam

1. 🎯 **Read Carefully**
   - Look for keywords: "most cost-effective", "least administrative effort"
   - These keywords often indicate the answer

2. ❌ **Eliminate Obviously Wrong Answers**
   - Usually can eliminate 1-2 options immediately
   - Increases your odds to 50/50

3. 🤔 **Think Logically**
   - Microsoft doesn't create features to confuse
   - What makes the most sense?
   - What would you actually do?

4. 📋 **Ordering Questions**
   - Put steps in logical order
   - Think: What MUST happen first?
   - Dependencies matter

5. 🚫 **Don't Panic on Unknown Topics**
   - You might see 1-2 questions on unfamiliar topics
   - Make intelligent guess
   - Move on, don't waste time

### If You Don't Pass First Time

1. 📊 **Review Score Report**
   - Shows performance by domain
   - Identify weakest areas

2. 🔄 **Study Weak Domains**
   - Re-read those sections in notes
   - Do hands-on labs for those topics
   - Watch videos if needed

3. ✅ **Retake When Ready**
   - Wait 24 hours minimum
   - Usually get it second time
   - Many people don't pass first attempt (it's OK!)

---

## Portal Navigation

- **VM Extensions**: VM → Extensions + applications → Add
- **Backup**: VM → Operations → Backup
- **Availability Set**: Search "Availability sets" → Create
- **VMSS**: Search "Virtual machine scale sets" → Create → Choose Flex
- **Container Registry**: Search "Container registries" → Create
- **ACI**: Search "Container Instances" → Create
- **AKS**: Search "Kubernetes services" → Create
- **App Service**: Search "App Services" → Create
- **Activity Log**: Subscription/Resource → Activity log
- **Metrics**: Resource → Monitoring → Metrics
- **Diagnostic Settings**: Resource → Monitoring → Diagnostic settings
- **Log Analytics**: Search "Log Analytics workspaces" → Create
- **Alerts**: Monitor → Alerts → Create alert rule
- **Action Groups**: Monitor → Alerts → Action groups
- **Alert Processing Rules**: Monitor → Alerts → Alert processing rules
- **Network Watcher**: Search "Network Watcher" → Regional service
- **Backup Center**: Search "Backup Center" → Central management

---

## Decision Trees

### Availability Decision

```
Need high availability for VMs?
├─ Region supports Availability Zones?
│   ├─ Yes → Use Availability Zones (best option)
│   │   └─ Deploy VMs across zones (1, 2, 3)
│   └─ No → Use Availability Sets
│       └─ Create separate AvSet per application tier
└─ How many tiers?
    └─ Web tier → AvSet1, App tier → AvSet2, DB tier → AvSet3
```

### Container Hosting Decision

```
Need to run containers?
├─ Simple, single container?
│   └─ Azure Container Instances (ACI)
├─ Multiple containers, no orchestration?
│   └─ Container Groups (ACI)
├─ Complex app, need orchestration?
│   ├─ Kubernetes experience?
│   │   └─ Azure Kubernetes Service (AKS)
│   └─ Just want to run containers?
│       └─ App Service (containers) or ACI
└─ Need full control?
    └─ VMs with Docker
```

### Log Analytics Workspace Table Type

```
Which table type for logs?
├─ Need full KQL and alerting?
│   └─ Analytics Logs
├─ High volume, rarely query?
│   └─ Basic Logs (pay per query)
├─ Long-term compliance only?
│   └─ Archive (restore to query)
└─ Cost sensitive?
    ├─ Query frequently → Analytics
    └─ Query rarely → Basic
```

### Monitoring Destination

```
Enable diagnostic settings?
Where to send logs?
├─ Need to query and alert?
│   └─ Log Analytics Workspace (best)
├─ Need cheapest archive?
│   └─ Storage Account
├─ External SIEM?
│   └─ Event Hub
└─ Need all of above?
    └─ Can send to multiple simultaneously
```

---

## PowerShell/CLI Commands

```powershell
# VM Extensions
Set-AzVMExtension -ResourceGroupName RG -VMName VM -Name CustomScript
Get-AzVMExtension -ResourceGroupName RG -VMName VM

# Availability Set
New-AzAvailabilitySet -ResourceGroupName RG -Name MyAvSet -Location Region

# VMSS
New-AzVmss -ResourceGroupName RG -Name MyVMSS -OrchestrationMode Flexible

# Container Registry
New-AzContainerRegistry -ResourceGroupName RG -Name MyACR -Sku Basic

# ACI
New-AzContainerGroup -ResourceGroupName RG -Name MyACI

# AKS
New-AzAksCluster -ResourceGroupName RG -Name MyAKS

# Diagnostic Settings
Set-AzDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId

# Log Analytics
New-AzOperationalInsightsWorkspace -ResourceGroupName RG -Name MyWorkspace

# Alerts
New-AzMetricAlertRuleV2 -ResourceGroupName RG -Name MyAlert

# Action Group
New-AzActionGroup -ResourceGroupName RG -Name MyActionGroup

# Network Watcher
Test-AzNetworkWatcherIPFlow -NetworkWatcher $nw -TargetVirtualMachineId $vmId
Get-AzNetworkWatcherNextHop -NetworkWatcher $nw
```

---

## Final Study Checklist

### Core Concepts to Know Cold

- [ ] Entra ID vs AD DS (Part 1)
- [ ] Management Groups, Subscriptions, Resource Groups hierarchy (Part 2)
- [ ] Azure Policy vs RBAC (Part 3)
- [ ] VNet peering non-transitivity (Part 3)
- [ ] NSG priority (1 = highest, 65500 = lowest) (Part 3)
- [ ] Service Endpoints vs Private Endpoints (Part 5)
- [ ] Load Balancer vs Application Gateway (Part 5)
- [ ] Storage redundancy types (LRS, ZRS, GRS, GZRS) (Part 6)
- [ ] Blob tiers and early deletion fees (Part 6)
- [ ] Cannot shrink disks (Part 7)
- [ ] VM CPU:Memory ratios (1:4, 1:2, 1:8) (Part 7)
- [ ] Availability Zones > Availability Sets (Part 8)
- [ ] Log Analytics workspace types (Part 8)

### Hands-On Skills to Practice

- [ ] Create VNet with subnets and NSGs
- [ ] Create VM and connect via Bastion
- [ ] Configure VNet peering
- [ ] Create storage account with blob tiers
- [ ] Set up lifecycle management
- [ ] Configure diagnostic settings
- [ ] Create alert rules with action groups
- [ ] Use Network Watcher tools
- [ ] Deploy VMSS with autoscaling
- [ ] Configure Azure Backup

### Common Exam Traps

- ⚠️ Tags don't inherit (use policy to copy)
- ⚠️ VNet peering is NOT transitive
- ⚠️ NSG rules: Lower number = higher priority
- ⚠️ Basic Load Balancer retiring Sept 2025
- ⚠️ Premium Files billed on provisioned size
- ⚠️ Archive tier is OFFLINE (must rehydrate)
- ⚠️ SAS tokens depend on access keys
- ⚠️ Cannot shrink disks (only grow)
- ⚠️ Diagnostic settings NOT enabled by default
- ⚠️ Basic Logs limited to 8 days

---

## Good Luck! 🎓

You've covered all 8 parts of the AZ-104 study material. Key to success:

1. **Hands-on practice** - Create resources, break them, fix them
2. **Understand WHY** - Not just memorize facts
3. **Think logically** - Microsoft's features make sense
4. **Don't panic** - Eliminate wrong answers, make intelligent guesses
5. **Review weak areas** - Use score report to target gaps

**You got this!** 💪

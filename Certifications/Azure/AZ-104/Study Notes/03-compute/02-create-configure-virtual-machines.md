# 3.2 — Create and Configure Virtual Machines

**Domain Weight: 20–25% (HIGH PRIORITY)**

---

## Core Concepts

### Create a Virtual Machine

Azure Virtual Machines (VMs) provide on-demand, scalable computing resources in the cloud. You create VMs through the Azure portal, CLI, PowerShell, or ARM/Bicep templates by specifying a subscription, resource group, region, image, size, and administrator credentials. VMs are billed per-second based on the size and OS.

**VM Creation — Portal Basics Tab:**

- **Subscription** — billing account
- **Resource group** — logical container
- **VM name** — must follow Azure naming conventions
- **Region** — datacenter location (affects available sizes/images)
- **Availability options** — None, Availability Zone, Availability Set, or VMSS
- **Security type** — Standard, Trusted Launch, or Confidential
- **Image** — OS (e.g., Windows Server 2022, Ubuntu 22.04 LTS)
- **Size** — CPU/RAM/disk configuration (e.g., Standard_DS2_v3)
- **Administrator account** — username + password or SSH key
- **Inbound port rules** — RDP (3389), SSH (22), HTTP (80), HTTPS (443)

**Authentication Types:**

| OS | Options |
| ------- | --------------------------------- |
| Windows | Password only |
| Linux | SSH public key OR password |

**Key Defaults:**

- OS disk: Premium SSD (for supported sizes)
- Networking: new VNet + subnet auto-created
- Public IP: created by default (Standard SKU)
- Boot diagnostics: enabled by default

**Password Requirements:**

- Minimum 12 characters
- Must include: uppercase, lowercase, digit, special character

---

### Configure Azure Disk Encryption

Azure provides multiple disk encryption options to protect data at rest on VM disks. Azure Disk Encryption (ADE) uses BitLocker (Windows) or DM-Crypt (Linux) for volume-level encryption inside the guest OS and integrates with Azure Key Vault for key management. Server-Side Encryption (SSE) is always on by default for all managed disks.

**Encryption Options Comparison:**

| Feature | Server-Side Encryption (SSE) | Encryption at Host | Azure Disk Encryption (ADE) |
| ---------------------- | ---------------------------- | ------------------ | --------------------------- |
| OS + data disks at rest | YES | YES | YES |
| Temp disk encryption | NO | YES (PMK only) | YES |
| Cache encryption | NO | YES | YES |
| Uses VM CPU | NO | NO | YES |
| Requires Key Vault | Optional (CMK) | Optional (CMK) | YES (required) |
| Custom Linux images | YES | YES | NO |

**Azure Disk Encryption (ADE) Key Facts:**

- Windows: uses **BitLocker**
- Linux: uses **DM-Crypt**
- Integrated with **Azure Key Vault** (required)
- Supports Key Encryption Key (KEK) for extra protection
- Zone resilient
- **ADE retiring September 15, 2028** — use **Encryption at Host** for new VMs
- Cannot be used on **Basic-tier VMs**
- VM must be **deallocated** to move to another subscription with ADE enabled (must disable encryption first)

**Server-Side Encryption (SSE):**

- Always enabled automatically on all managed disks
- 256-bit AES encryption, FIPS 140-2 compliant
- Default: platform-managed keys (PMK)
- Optional: customer-managed keys (CMK) via Disk Encryption Set (DES)
- No performance impact, no extra cost

**Encryption at Host:**

- Encrypts temp disks, caches, AND data flows between compute and storage
- Does NOT use VM CPU
- Must be enabled at the subscription level first

**Mnemonic — Encryption: "BAD-K"**

- **B**itLocker = Windows ADE
- **A**lways-on = SSE (default for all managed disks)
- **D**M-Crypt = Linux ADE
- **K**ey Vault = required for ADE

---

### Move a Virtual Machine to Another Resource Group, Subscription, or Region

Azure VMs can be moved between resource groups, subscriptions, or regions depending on the scenario. Moving across resource groups/subscriptions is a metadata operation, while moving across regions requires Azure Resource Mover and involves recreating the VM in the target region.

**Move Across Resource Groups / Subscriptions:**

- VM + all dependent resources (disks, NICs, NSGs) must move together
- Cross-subscription: all resources must be in the **same resource group** first
- Role assignments do NOT move — must recreate after move
- ADE-encrypted VMs: must **disable encryption** before cross-subscription move (cross-RG is OK if deallocated)
- Dynamic IPs may change; static IPs are preserved

**3-Step Process for Cross-Subscription Move:**

1. Consolidate dependent resources into one resource group
2. Move resource group from source to target subscription
3. (Optional) Redistribute resources into different RGs in target subscription

**Move Across Regions (Azure Resource Mover):**

- Uses **Azure Resource Mover** service
- Supported resources: VMs, disks, NICs, availability sets, VNets, Public IPs, NSGs, LBs
- Process: Prepare → Initiate Move → Commit → Clean up source
- Requires **Owner** access on the subscription
- Creates a system-assigned managed identity for the move
- Check quota availability in the target region

**Permissions Required:**

- Source RG: `Microsoft.Resources/subscriptions/resourceGroups/moveResources/action`
- Destination RG: `Microsoft.Resources/subscriptions/resourceGroups/write`

---

### Manage Virtual Machine Sizes

VM sizes determine the CPU, RAM, storage, and networking capacity allocated to a VM. You can resize a running or deallocated VM, but resizing is a disruptive operation that causes a restart. The new size must be available on the hardware cluster hosting the VM.

**VM Size Families:**

| Family | Optimized For | Example Series |
| --------------- | ---------------------------------- | ------------------- |
| **B** | Burstable (dev/test) | B2s, B4ms |
| **D** | General purpose | Dsv5, Ddsv5 |
| **E** | Memory optimized | Esv5, Edsv5 |
| **F** | Compute optimized (high CPU) | Fsv2 |
| **L** | Storage optimized (high disk I/O) | Lsv3 |
| **N** | GPU (AI/ML, rendering) | NCasv3, NDv2 |
| **M** | Memory optimized (very large) | Msv2 |

**VM Naming Convention:** `Standard_D2s_v5`

- **D** = family
- **2** = vCPUs
- **s** = Premium Storage capable
- **v5** = generation

**Resizing Rules:**

- Resizing a running VM causes a **restart**
- If size not available on current cluster → must **deallocate first**
- Deallocating releases **dynamic IPs** (static IPs preserved)
- OS and data disks are **NOT affected** by deallocation
- Can't resize: temp disk VM ↔ no temp disk VM
- In availability set: may need to **deallocate ALL VMs** in the set to resize
- Premium Storage: choose sizes with **"s"** in the name

---

### Manage Virtual Machine Disks

Azure Managed Disks are block-level storage volumes managed by Azure, attached to VMs. There are three disk roles (OS, data, temporary) and five disk types offering different performance tiers. Managed disks provide 99.999% availability with three replicas of data.

**Disk Roles:**

| Role | Description | Persistent? |
| ----------- | ------------------------------------------------ | ----------- |
| **OS disk** | Contains the operating system, max 4,095 GiB | YES |
| **Data disk** | Application data, up to 64 per VM (size-dependent) | YES |
| **Temp disk** | Temporary storage (page file, swap), data lost on stop/deallocation | NO |

**Managed Disk Types:**

| Type | Scenario | Max Size | Max IOPS | Max Throughput | OS Disk? |
| --------------- | ------------------------------ | -------- | -------- | -------------- | -------- |
| Ultra Disk | IO-intensive (SAP HANA, SQL) | 65,536 GiB | 400,000 | 10,000 MB/s | NO |
| Premium SSD v2 | High perf, low latency | 65,536 GiB | 80,000 | 1,200 MB/s | NO |
| Premium SSD | Production workloads | 32,767 GiB | 20,000 | 900 MB/s | YES |
| Standard SSD | Web servers, dev/test | 32,767 GiB | 6,000 | 750 MB/s | YES |
| Standard HDD | Backup, infrequent access | 32,767 GiB | 2,000 | 500 MB/s | YES* |

\* Standard HDD as OS disk retiring September 8, 2028

**Key Disk Facts:**

- Managed disks: 99.999% availability, 3 replicas (LRS = 11 9s durability, ZRS = 12 9s)
- Can convert between Premium SSD, Standard SSD, Standard HDD (requires restart)
- Cannot convert directly to/from Ultra Disk (use snapshot method)
- Disk type can be changed **max 2 times per day**
- Data disks max: **up to 64** per VM (depends on VM size)
- MBR partition limits OS disk to 2 TiB usable (convert to GPT for full 4 TiB)

---

### Deploy Virtual Machines to Availability Zones and Availability Sets

Availability zones and availability sets are mechanisms to protect VMs from infrastructure failures. Availability zones protect against datacenter-level failures within a region (physically separate locations). Availability sets protect against hardware failures within a single datacenter using fault domains and update domains.

**Availability Zones:**

- **3 zones** per supported Azure region
- Each zone: independent power, network, cooling
- SLA: **99.99%** for VMs spread across zones
- Zonal VM = pinned to a specific zone
- Regional (non-zonal) VM = placed in any zone (risky)
- No extra cost for zonal deployment
- Requires **Standard Load Balancer** (Basic LB not supported across zones)
- Must use **Managed Disks**

**Availability Sets:**

- Logical grouping within a single datacenter
- SLA: **99.95%** for 2+ VMs in an availability set
- **Fault Domains (FDs):** max 3 — separate power + network (share nothing)
- **Update Domains (UDs):** max 20 — groups rebooted together during maintenance
- Only one UD rebooted at a time (30 min recovery before next)
- No extra cost for availability sets
- Cannot combine availability sets with availability zones (unless using Proximity Placement Groups)
- Must add VM to availability set **at creation time** — cannot add later
- Only managed disks in managed availability sets

**Mnemonic — "AZ vs AS" (Zones vs Sets):**

- **AZ** = Across Zones (datacenter protection, 99.99%)
- **AS** = Across hardware (rack protection, 99.95%)

---

### Deploy and Configure Azure Virtual Machine Scale Sets

Azure Virtual Machine Scale Sets (VMSS) let you create and manage a group of load-balanced VMs that can automatically scale based on demand or schedule. Scale sets distribute VMs across fault domains and availability zones, providing high availability. There is no cost for the scale set itself — you pay only for each VM instance.

**Key VMSS Features:**

- Auto-scaling based on metrics (CPU, memory) or schedule
- Supports up to **1,000 VMs** (marketplace/gallery images) or **600** (managed images)
- Automatic OS upgrades
- Distribute across availability zones
- Integrates with Azure Load Balancer and Application Gateway

**Orchestration Modes:**

| Feature | Flexible (Recommended) | Uniform |
| ----------------------- | -------------------------------------- | ----------------------------------- |
| VM type | Standard Azure IaaS VMs | Scale set-specific VMs |
| Max instances (with FD) | 1,000 | 100 |
| Mix OS types | YES (Linux + Windows) | NO |
| Mix Spot + On-demand | YES | NO |
| Azure Backup support | YES | NO |
| Azure Site Recovery | YES | NO |
| Standard VM API | YES | NO (uses VMSS VM API) |
| Add existing VM | YES | NO |
| Service Fabric | NO | YES |

**Orchestration mode is set at creation and CANNOT be changed later.**

**Autoscale Rules:**

- **Scale-out:** increase instances when metric exceeds threshold (e.g., CPU > 75% for 10 min)
- **Scale-in:** decrease instances when metric drops (e.g., CPU < 25% for 10 min)
- Can set: **default, minimum, maximum** instance counts
- Schedule-based: scale at specific times (e.g., business hours)
- Autoscale does NOT start stopped/deallocated VMs — creates new ones

**Upgrade Policies:**

| Policy | Behavior |
| ------------- | ------------------------------------------------- |
| **Automatic** | Instances upgraded immediately (random order) |
| **Rolling** | Upgraded in batches with configurable pause |
| **Manual** | No auto-upgrade; admin triggers manually |

---

## What The Exam Tests

- VM creation required fields: subscription, RG, name, region, image, size, admin credentials
- Encryption: ADE uses BitLocker (Windows) / DM-Crypt (Linux) + Key Vault
- SSE is always enabled by default on managed disks
- Moving VM cross-subscription: all dependent resources must be in same RG
- Resize causes restart; deallocate if size not available on current cluster
- Availability zones = 99.99% SLA; availability sets = 99.95% SLA
- Availability set: max 3 FDs, max 20 UDs
- VMSS Flexible vs Uniform orchestration differences
- Orchestration mode cannot be changed after creation
- VMSS max instances: 1,000 (Flexible), 100 (Uniform with FD guarantees)
- Autoscale creates new VMs, doesn't start deallocated ones
- Ultra Disk and Premium SSD v2 cannot be used as OS disks
- Must add VM to availability set at creation time

---

## Important Limits / Numbers

| Item | Value |
| ------------------------------------------------- | ------------------------------------------------ |
| Max data disks per VM | **Up to 64** (depends on VM size) |
| OS disk max capacity | **4,095 GiB** (2 TiB usable with MBR) |
| Availability set fault domains | **Max 3** |
| Availability set update domains | **Max 20** |
| Availability zone SLA | **99.99%** |
| Availability set SLA | **99.95%** (2+ VMs) |
| VMSS max instances (Flexible) | **1,000** |
| VMSS max instances (Uniform, single placement) | **100** |
| Managed disk availability | **99.999%** |
| Disk type changes per day | **Max 2** |
| Password minimum length | **12 characters** |
| VM size "s" designation | Premium Storage capable |
| ADE retirement date | **September 15, 2028** |
| Standard HDD OS disk retirement | **September 8, 2028** |

---

## CLI + PowerShell Commands

### Azure CLI

```bash
# Create a VM
az vm create \
  --resource-group myRG \
  --name myVM \
  --image Ubuntu2204 \
  --size Standard_DS2_v3 \
  --admin-username azureuser \
  --generate-ssh-keys

# Resize a VM
az vm resize \
  --resource-group myRG \
  --name myVM \
  --size Standard_DS3_v2

# List available sizes for a VM
az vm list-vm-resize-options \
  --resource-group myRG \
  --name myVM \
  --output table

# Deallocate a VM
az vm deallocate --resource-group myRG --name myVM

# Start a VM
az vm start --resource-group myRG --name myVM

# Enable ADE on a VM
az vm encryption enable \
  --resource-group myRG \
  --name myVM \
  --disk-encryption-keyvault myKeyVault

# Disable ADE (required before cross-sub move)
az vm encryption disable \
  --resource-group myRG \
  --name myVM \
  --volume-type all

# Create a VMSS
az vmss create \
  --resource-group myRG \
  --name myVMSS \
  --image Ubuntu2204 \
  --orchestration-mode Flexible \
  --instance-count 2 \
  --admin-username azureuser \
  --generate-ssh-keys \
  --zones 1 2 3

# Create autoscale profile
az monitor autoscale create \
  --resource-group myRG \
  --resource myVMSS \
  --resource-type Microsoft.Compute/virtualMachineScaleSets \
  --name autoscale \
  --min-count 2 --max-count 10 --count 2
```

### PowerShell

```powershell
# Create a VM
New-AzVM `
  -ResourceGroupName "myRG" `
  -Name "myVM" `
  -Location "EastUS" `
  -Image "Win2022AzureEdition" `
  -Size "Standard_DS2_v3"

# Resize a VM
$vm = Get-AzVM -ResourceGroupName "myRG" -VMName "myVM"
$vm.HardwareProfile.VmSize = "Standard_DS3_v2"
Update-AzVM -VM $vm -ResourceGroupName "myRG"

# List available sizes
Get-AzVMSize -ResourceGroupName "myRG" -VMName "myVM"

# Deallocate
Stop-AzVM -ResourceGroupName "myRG" -Name "myVM" -Force

# Enable ADE
Set-AzVMDiskEncryptionExtension `
  -ResourceGroupName "myRG" `
  -VMName "myVM" `
  -DiskEncryptionKeyVaultUrl $kvURL `
  -DiskEncryptionKeyVaultId $kvID

# Disable ADE
Disable-AzVMDiskEncryption `
  -ResourceGroupName "myRG" `
  -VMName "myVM" `
  -VolumeType All

# Move resources to new RG
Move-AzResource -DestinationResourceGroupName "newRG" `
  -ResourceId $vm.Id
```

---

## Confusion Matrix

| Feature | Availability Zone | Availability Set |
| ----------------------------- | ----------------------------- | ------------------------------ |
| Protection level | Datacenter failure | Hardware rack failure |
| SLA | 99.99% | 99.95% |
| Physically separate | YES (different buildings) | NO (same datacenter) |
| Supports VMSS | YES | YES (limited) |
| Load balancer required | Standard LB | Basic or Standard LB |
| Can add VM after creation | YES (choose zone at creation) | NO (must be at creation) |
| Max fault domains | 3 zones | 3 FDs |

| Feature | Flexible Orchestration | Uniform Orchestration |
| ---------------------- | ---------------------- | --------------------- |
| Recommended | YES | NO (legacy) |
| Standard VM APIs | YES | NO (VMSS VM APIs) |
| Max instances | 1,000 | 100 |
| Mix VM sizes | YES | NO |
| Mix OS | YES | NO |
| Azure Backup | YES | NO |
| Add existing VM | YES | NO |
| Can change mode later | NO | NO |

| Encryption | SSE | ADE | Encryption at Host |
| ------------------- | ------------- | ------------ | ------------------ |
| Always on | YES | NO (enable) | NO (enable) |
| Temp disk | NO | YES | YES |
| Key Vault required | NO (optional) | YES | NO (optional) |
| Uses VM CPU | NO | YES | NO |
| Retiring | NO | YES (2028) | NO |

---

## Decision Rules

| If... | Then... |
| -------------------------------------------------------- | --------------------------------------------------------------------- |
| Need highest availability (datacenter protection) | Use **Availability Zones** (99.99% SLA) |
| Need rack-level protection within same datacenter | Use **Availability Set** (99.95% SLA) |
| Need auto-scaling group of VMs | Use **VMSS with Flexible orchestration** |
| New VM needs disk encryption | Use **Encryption at Host** (not ADE, which is retiring) |
| Existing ADE VM needs to move cross-subscription | **Disable ADE first**, then move |
| VM resize fails — size not available | **Deallocate** the VM (or all VMs in availability set) |
| Need Premium Storage | Choose VM size with **"s"** in the name |
| Need to add data disk to running VM | Can attach **hot-add** without restart (managed disks) |
| Ultra Disk / Premium SSD v2 needed | Can only be used as **data disks**, not OS disks |
| Need to move VM to another region | Use **Azure Resource Mover** |

---

## 5 Common Scenario Patterns

**Scenario 1: High availability across datacenters**

> "Contoso needs a web app with 99.99% SLA in East US."

- Deploy VMs across **3 availability zones** in East US
- Use **Standard Load Balancer** (required for cross-zone)
- Use **Managed Disks** (required for zones)

**Scenario 2: VM resize blocked**

> "An admin tries to resize a VM to Standard_E8s_v5 but the size isn't listed."

- The size is not available on the current hardware cluster
- **Deallocate** the VM first → more sizes become available
- Deallocating releases dynamic IPs but preserves OS/data disks

**Scenario 3: Encrypt existing VM disks**

> "A compliance audit requires all VM disks to be encrypted with customer-managed keys."

- For new VMs: enable **Encryption at Host** with a Disk Encryption Set (DES)
- For existing VMs with ADE: plan migration to Encryption at Host before Sept 2028
- ADE requires **Azure Key Vault** in the same region

**Scenario 4: Auto-scaling web application**

> "Traffic spikes during business hours. Contoso wants VMs to scale automatically."

- Create **VMSS** with **Flexible orchestration**
- Configure autoscale: scale out when CPU > 75%, scale in when CPU < 25%
- Set min = 2, max = 10, default = 2
- Add **schedule-based rule**: scale to 5 at 9 AM, scale to 2 at 6 PM

**Scenario 5: Move VM to new subscription**

> "Finance wants VM billing moved from Dev subscription to Prod subscription."

- Ensure VM + all dependent resources (disks, NICs, public IPs) are in the **same RG**
- If ADE enabled: **disable encryption** first
- Move the entire RG to the new subscription
- Recreate role assignments in the new subscription (they don't transfer)

---

## Exam Traps

> **TRAP 1:** Resizing a **running** VM causes a **restart** — even if the new size is available on the same cluster. Always treat resize as disruptive.

> **TRAP 2:** You must add a VM to an **availability set at creation time** — you cannot add an existing VM to an availability set later.

> **TRAP 3:** **Ultra Disks** and **Premium SSD v2** cannot be used as **OS disks** — they are data disks only.

> **TRAP 4:** Availability zones require a **Standard Load Balancer** — Basic LB does not support cross-zone scenarios.

> **TRAP 5:** When moving a VM **cross-subscription**, all dependent resources (disks, NICs) must be in the **same resource group** first.

> **TRAP 6:** **ADE-encrypted VMs** cannot move cross-subscription without first **disabling encryption**.

> **TRAP 7:** VMSS **orchestration mode cannot be changed** after creation — Flexible or Uniform is permanent.

> **TRAP 8:** VMSS autoscale **creates new VM instances** — it does NOT start stopped/deallocated VMs.

> **TRAP 9:** **SSE (Server-Side Encryption) is always enabled** on managed disks — you cannot turn it off. ADE is the optional add-on that requires Key Vault.

> **TRAP 10:** In an availability set, if the desired size isn't available, you may need to **deallocate ALL VMs** in the set to resize any single VM.

---

## Rapid-Fire Q&A

**Q: What encryption does ADE use on Windows?**
A: BitLocker

**Q: What encryption does ADE use on Linux?**
A: DM-Crypt

**Q: Is SSE enabled by default on managed disks?**
A: Yes — always on, 256-bit AES, cannot be disabled

**Q: What does ADE require?**
A: Azure Key Vault (in the same region as the VM)

**Q: What is the SLA for VMs across availability zones?**
A: 99.99%

**Q: What is the SLA for VMs in an availability set?**
A: 99.95% (requires 2+ VMs)

**Q: Max fault domains in an availability set?**
A: 3

**Q: Max update domains in an availability set?**
A: 20

**Q: Can you add a VM to an availability set after creation?**
A: No — must be set at VM creation time

**Q: What happens when you resize a running VM?**
A: It restarts

**Q: What happens to dynamic IPs when you deallocate?**
A: They are released (static IPs are preserved)

**Q: Can Ultra Disks be used as OS disks?**
A: No — data disks only

**Q: Max data disks per VM?**
A: Up to 64 (depends on VM size)

**Q: What is the recommended VMSS orchestration mode?**
A: Flexible orchestration

**Q: Can you change VMSS orchestration mode after creation?**
A: No — it is permanent

**Q: What load balancer is required for availability zones?**
A: Standard Load Balancer (Basic LB not supported)

**Q: How do you move a VM to another region?**
A: Use Azure Resource Mover

**Q: What must you do before moving an ADE-encrypted VM cross-subscription?**
A: Disable encryption first

**Q: Does VMSS autoscale start deallocated VMs?**
A: No — it creates new instances

**Q: What "s" means in VM size name (e.g., Standard_DS2_v3)?**
A: Premium Storage capable

# 2.3 — Configure Azure Files and Azure Blob Storage

**Domain Weight: 15–20% (MEDIUM PRIORITY)**

---

## Core Concepts

### Create and Configure a File Share in Azure Storage

Azure Files provides fully managed file shares in the cloud accessible via SMB or NFS protocols. File shares replace or supplement on-premises file servers. You choose between SSD (premium/low-latency) and HDD (standard/cost-effective) media, and configure protocol, access tier, identity-based authentication, and networking.

**SMB vs NFS:**

| Feature | SMB | NFS |
|---|---|---|
| Protocol versions | 3.1.1, 3.0, 2.1 | NFSv4.1 only |
| Port | **TCP 445** | **TCP 2049** |
| Media tiers | SSD and HDD | **SSD only** |
| Redundancy | LRS, ZRS, GRS, GZRS | **LRS, ZRS only** |
| Authentication | Identity-based (Kerberos) + shared key | **Host-based only** (no identity auth) |
| Authorization | Windows ACLs (NTFS) | UNIX permissions (UID/GID) |
| Case sensitivity | Case insensitive | Case sensitive |
| Internet accessible | Yes (SMB 3.0+) | **No** (requires private/service endpoint) |
| Azure File Sync | Yes | **No** |
| Azure Backup | Yes | **No** |
| Client OS | Windows, Linux, macOS | **Linux only** |
| Hard/symbolic links | No | Yes |

- A single share uses **one protocol only** (SMB or NFS), but the same storage account can have both
- NFS requires **FileStorage** (premium) account type

**Storage Account Types for File Shares:**

| Account Type | Media | Protocols | Billing |
|---|---|---|---|
| **FileStorage** (premium) | SSD | SMB + NFS | Provisioned v1 or v2 |
| **StorageV2** (general purpose) | HDD | SMB only | Pay-as-you-go or provisioned v2 |

**Access Tiers (HDD Pay-as-you-go Only):**

| Tier | Storage Cost | Transaction Cost | Best For |
|---|---|---|---|
| **Transaction Optimized** | Highest | Lowest | High-transaction workloads, migration |
| **Hot** | Medium | Medium | Active workloads |
| **Cool** | Lowest | Highest | Infrequently accessed |

- SSD/premium shares = "Premium" tier (no tier choice)
- Max **5 tier changes per 30 days**, minimum **24 hours** between changes

**Identity-Based Authentication (SMB Only):**

| Source | User Types | Client Requirement |
|---|---|---|
| **On-prem AD DS** | Hybrid only | Domain-joined + DC connectivity |
| **Entra Domain Services** | Cloud + hybrid | Domain-joined to managed domain |
| **Entra Kerberos** | Hybrid (GA), cloud (preview) | Entra-joined or hybrid-joined |

- Only **ONE identity source** per storage account
- NFS = host-based auth only (no identity-based)
- SMB RBAC roles: Reader, Contributor, Elevated Contributor, Privileged Reader/Contributor

**Mounting:**

- Windows: `net use Z: \\<account>.file.core.windows.net\<share>` (port 445 must be open)
- Linux SMB: `mount -t cifs` with `cifs-utils`
- Linux NFS: `mount -o vers=4,minorversion=1` (requires private/service endpoint)
- Port 445 blocked? → VPN + private endpoint, or Azure File Sync with SMB over QUIC (port 443)

---

### Create and Configure a Container in Blob Storage

A blob container is a logical grouping of blobs within a storage account, similar to a directory. Every blob must reside in a container. Containers support metadata, access policies, and anonymous access levels. You cannot nest containers inside other containers.

**Container Naming Rules:**

- **3–63 characters** long
- **Lowercase letters, numbers, and hyphens (-)** only
- Must start with a **letter or number**
- No **consecutive hyphens** allowed
- All letters must be **lowercase**

**Public Access Levels:**

| Level | Description |
|---|---|
| **Private** (default) | No anonymous access — authorization required |
| **Blob** | Anonymous read for blobs only — cannot list blobs |
| **Container** | Anonymous read for blobs + can list blobs in container |

- Requires TWO settings: **account-level** (`AllowBlobPublicAccess`) AND **container-level**
- Account-level disabled = **overrides** all container settings → 403 Forbidden
- Cannot set access level per individual blob — only per container
- `$web` container (static websites) is **always public** regardless of settings

---

### Configure Storage Tiers

Azure Blob Storage access tiers optimize cost based on data access patterns. Hot, Cool, and Cold are online tiers with millisecond latency. Archive is an offline tier requiring rehydration before access. Tiers can be set at the account level (default) or per individual blob.

**Tier Summary:**

| Tier | Min Retention | Latency | Online? | Redundancy |
|---|---|---|---|---|
| **Hot** | None | ms | Yes | All |
| **Cool** | **30 days** | ms | Yes | All |
| **Cold** | **90 days** | ms | Yes | All |
| **Archive** | **180 days** | **Hours** | **No** | LRS, GRS, RA-GRS only |

- Default account tier = **Hot** (can be set to Cool or Cold, **NOT Archive**)
- Blobs without explicit tier **inherit** account default (shown as "inferred")
- Access tiers apply to **block blobs only** (not append or page)

**Rehydration from Archive:**

| Method | How It Works | Advantage |
|---|---|---|
| **Copy Blob** (recommended) | Copies to new blob in online tier; source stays in archive | Avoids early deletion fee |
| **Set Blob Tier** | Changes tier in place; cannot be cancelled | Simpler but risks early deletion charge |

| Priority | Speed | Cost |
|---|---|---|
| **Standard** | Up to **15 hours** | Lower |
| **High** | May complete in **< 1 hour** | Higher |

- Can upgrade Standard → High during pending rehydration; **cannot downgrade**
- Archive blob **metadata** is readable; **data is NOT** until rehydrated
- Lifecycle management policies **CANNOT rehydrate** from archive

**Tier Change Billing:**

- Warmer → cooler = billed as **write** to destination tier
- Cooler → warmer = billed as **read** from source tier + data retrieval
- Early deletion charges if moved before minimum retention (prorated)

---

### Configure Soft Delete for Blobs and Containers

Soft delete provides a safety net against accidental deletion. Blob soft delete protects individual blobs, snapshots, and versions. Container soft delete protects entire containers and their contents. These are **separate features** — enabling one does NOT enable the other. Neither protects against storage account deletion.

**Blob Soft Delete:**

- Retention: **1–365 days** (default 7 days)
- Overwriting a blob creates a **soft-deleted snapshot** of prior state
- Restore via `Undelete Blob` — restores blob + all soft-deleted snapshots
- Enabled by default in Azure portal (NOT via CLI/PowerShell)
- Does NOT protect against container or storage account deletion

**Container Soft Delete:**

- Retention: **1–365 days** (default 7 days)
- Restores container + all blobs, versions, and snapshots
- Must restore to **original name** — if name is reused, restore fails
- Does NOT protect against storage account deletion

**Interaction with Versioning:**

| Action | Soft Delete Only | Versioning + Soft Delete |
|---|---|---|
| Overwrite blob | Creates soft-deleted snapshot | Creates previous version (NO snapshot) |
| Delete blob | Blob marked soft-deleted | Current → previous version; NO snapshot |
| Delete previous version | N/A | Version is **soft-deleted** |
| Undelete | Restores blob + snapshots | Restores ALL soft-deleted versions |

- With versioning + soft delete: `Undelete Blob` restores versions but does **NOT** promote to current → must use `Copy Blob`
- Soft-deleted data billed at **active data rates**

---

### Configure Snapshots and Soft Delete for Azure Files

Share snapshots are point-in-time, read-only copies of an Azure file share. They are incremental — only changes since the last snapshot are stored. File share soft delete protects entire shares from accidental deletion. Snapshots protect individual files; soft delete protects the share itself.

**Share Snapshots:**

- Captured at **file share level**, retrieved at **individual file level**
- **Incremental** — only changed data stored
- Read-only — cannot modify a snapshot
- Same redundancy as parent share
- Do **NOT count** toward share size quota
- Windows access: **Previous Versions** tab in File Explorer (VSS)
- Deleting a share **deletes all its snapshots** — cannot keep snapshots without the parent

**File Share Soft Delete:**

- Retention: **1–365 days** (default 7 days, enabled by default on new accounts)
- Enabled at **storage account level** — applies to ALL file shares
- Restores share + all contents **including snapshots**
- Azure Backup forces minimum **14 days** retention
- To permanently delete early: undelete → disable soft delete → delete again → re-enable

**Mnemonic — "SS": Snapshots = files, Soft delete = shares**

---

### Configure Blob Lifecycle Management

Lifecycle management policies automate tiering and deletion of blobs based on rules. A policy is a JSON document with up to 100 rules. Rules define conditions (age-based) and actions (tier or delete). Policies run once per day and apply the least expensive action when multiple match.

**Available Actions:**

| Action | Description | Restrictions |
|---|---|---|
| `tierToCool` | Move to Cool | Not for append/page, not Premium |
| `tierToCold` | Move to Cold | Not for append/page, not Premium |
| `tierToArchive` | Move to Archive | Not for ZRS/GZRS, encryption scopes |
| `enableAutoTierToHotFromCool` | Auto-move accessed cool blobs to Hot | Only with `daysAfterLastAccessTimeGreaterThan` |
| `delete` | Delete the blob | Not for immutable containers |

- Action priority (cheapest wins): delete > tierToArchive > tierToCold > tierToCool

**Run Conditions:**

| Condition | Applies To |
|---|---|
| `daysAfterModificationGreaterThan` | Current version (last modified) |
| `daysAfterCreationGreaterThan` | Current version, versions, snapshots |
| `daysAfterLastAccessTimeGreaterThan` | Current version only (requires access tracking) |
| `daysAfterLastTierChangeGreaterThan` | `tierToArchive` only — prevents re-archiving |

**Filters:**

- `blobTypes`: `blockBlob`, `appendBlob` (required)
- `prefixMatch`: up to 10 prefixes, must start with container name, case-sensitive
- `blobIndexMatch`: up to 10 tag conditions

**Key Rules:**

- Policies run **once per day** (up to 24 hours for first run)
- Max **100 rules** per policy
- Cannot rehydrate from archive — only move cooler or delete
- Access time tracking: only **first read in 24 hours** updates `LastAccessTime`

---

### Configure Blob Versioning

Blob versioning automatically maintains previous versions of a blob whenever it is modified or deleted. Each version has a unique version ID (timestamp). Versions are immutable. This provides built-in protection against accidental modifications without manual snapshots.

**How It Works:**

- Create blob → single version = current version
- Modify blob → current becomes previous version, new current created
- Delete blob (no version ID) → current becomes previous, all previous versions persist
- Version ID = **timestamp** of when the version was created

**Key Rules:**

- Supported on GPv2, Premium Block Blob, legacy Blob Storage
- **NOT supported** with hierarchical namespace (Data Lake Storage)
- Every write creates a new version → potential **significant cost increase**
- Deleting a previous version requires **Storage Blob Data Owner** role (not Contributor)
- Each version can have its own access tier
- Disabling versioning does NOT delete existing versions
- Must delete object replication policies before disabling versioning

**Recommended Configuration:** Enable ALL THREE — container soft delete + blob soft delete + versioning

---

## What The Exam Tests

- SMB vs NFS protocol differences (ports, media, redundancy, auth, OS support)
- NFS requires premium/SSD only, no identity auth, no internet access, no File Sync, no Backup
- Container naming rules (3–63 chars, lowercase + numbers + hyphens, no consecutive hyphens)
- Public access requires both account-level AND container-level settings
- Access tier minimum retention periods and early deletion charges
- Archive tier is offline, cannot be default, not supported on ZRS-based accounts
- Rehydration methods (Copy Blob vs Set Blob Tier) and priorities
- Lifecycle management cannot rehydrate from archive
- Blob soft delete vs container soft delete are separate features
- Versioning + soft delete interaction (no soft-deleted snapshots, Undelete restores all versions)
- File share soft delete retention and Azure Backup minimum (14 days)
- Share snapshots are incremental, max 200, do not count toward quota
- Lifecycle rules: max 100, prefixes must include container name, run once per day

---

## Important Limits / Numbers

| Item | Value |
|---|---|
| Max file size (Azure Files) | **4 TiB** |
| Max share size (provisioned v2) | **256 TiB** |
| Max share size (pay-as-you-go / prov v1) | **100 TiB** |
| Max snapshots per file share | **200** |
| Max snapshot retention | **10 years** |
| Max stored access policies per share | **5** |
| File share tier changes per 30 days | **5** (min 24 hrs between) |
| Container name length | **3–63 characters** |
| Blob soft delete retention | **1–365 days** |
| Container soft delete retention | **1–365 days** |
| Azure Backup minimum soft delete retention | **14 days** |
| Cool tier min retention | **30 days** |
| Cold tier min retention | **90 days** |
| Archive tier min retention | **180 days** |
| Archive rehydration (standard priority) | **Up to 15 hours** |
| Archive rehydration (high priority) | **< 1 hour** (for <10 GB) |
| Max lifecycle management rules | **100** |
| Max prefixes per lifecycle rule | **10** |
| Max blob index tag conditions per rule | **10** |
| Lifecycle policy run frequency | **Once per day** |
| Recommended max versions per blob | **< 1,000** |
| SMB port | **TCP 445** |
| NFS port | **TCP 2049** |

---

## CLI + PowerShell Commands

### Azure CLI

```bash
# Create SMB file share with access tier
az storage share-rm create --resource-group myRG --storage-account myStorage \
  --name myshare --access-tier Hot --quota 100

# Create NFS file share
az storage share-rm create --resource-group myRG --storage-account myStorage \
  --name mynfsshare --enabled-protocols NFS --root-squash NoRootSquash

# Create file share snapshot
az storage share snapshot --name myshare --account-name myStorage

# Enable file share soft delete
az storage account file-service-properties update --resource-group myRG \
  --account-name myStorage --enable-delete-retention true --delete-retention-days 7

# Restore deleted file share
az storage share-rm restore -n deletedshare --deleted-version 01D64EB9886F00C4 \
  -g myRG --storage-account myStorage

# Create blob container
az storage container create --name mycontainer --account-name myStorage \
  --public-access off

# Set blob tier
az storage blob set-tier --account-name myStorage --container-name mycontainer \
  --name myblob --tier Cool --auth-mode login

# Enable blob soft delete
az storage account blob-service-properties update --account-name myStorage \
  --resource-group myRG --enable-delete-retention true --delete-retention-days 7

# Enable container soft delete
az storage account blob-service-properties update --account-name myStorage \
  --resource-group myRG --enable-container-delete-retention true \
  --container-delete-retention-days 7

# Enable blob versioning
az storage account blob-service-properties update --account-name myStorage \
  --resource-group myRG --enable-versioning true

# Enable access time tracking
az storage account blob-service-properties update --account-name myStorage \
  --resource-group myRG --enable-last-access-tracking true

# Create lifecycle management policy
az storage account management-policy create --account-name myStorage \
  --resource-group myRG --policy @policy.json
```

### PowerShell

```powershell
# Create SMB file share
New-AzRmStorageShare -ResourceGroupName "myRG" -StorageAccountName "myStorage" `
  -Name "myshare" -AccessTier Hot -QuotaGiB 100

# Create NFS file share
New-AzRmStorageShare -ResourceGroupName "myRG" -StorageAccountName "myStorage" `
  -Name "mynfsshare" -EnabledProtocol NFS -RootSquash NoRootSquash

# Create snapshot
New-AzRmStorageShare -ResourceGroupName "myRG" -StorageAccountName "myStorage" `
  -Name "myshare" -Snapshot

# Enable file share soft delete
Update-AzStorageFileServiceProperty -ResourceGroupName "myRG" `
  -StorageAccountName "myStorage" `
  -EnableShareDeleteRetentionPolicy $true -ShareRetentionDays 7

# Create blob container
New-AzStorageContainer -Name "mycontainer" -Permission Off -Context $ctx

# Enable blob soft delete
Enable-AzStorageBlobDeleteRetentionPolicy -ResourceGroupName "myRG" `
  -StorageAccountName "myStorage" -RetentionDays 7

# Enable container soft delete
Enable-AzStorageContainerDeleteRetentionPolicy -ResourceGroupName "myRG" `
  -StorageAccountName "myStorage" -RetentionDays 7

# Enable versioning
Update-AzStorageBlobServiceProperty -ResourceGroupName "myRG" `
  -StorageAccountName "myStorage" -IsVersioningEnabled $true

# Lifecycle policy rule
$action = Add-AzStorageAccountManagementPolicyAction `
  -BaseBlobAction TierToCool -daysAfterModificationGreaterThan 30
Add-AzStorageAccountManagementPolicyAction -InputObject $action `
  -BaseBlobAction TierToArchive -daysAfterModificationGreaterThan 90
Add-AzStorageAccountManagementPolicyAction -InputObject $action `
  -BaseBlobAction Delete -daysAfterModificationGreaterThan 365
$filter = New-AzStorageAccountManagementPolicyFilter -BlobType blockBlob
$rule = New-AzStorageAccountManagementPolicyRule -Name "myrule" `
  -Action $action -Filter $filter
Set-AzStorageAccountManagementPolicy -ResourceGroupName "myRG" `
  -StorageAccountName "myStorage" -Rule $rule
```

---

## Confusion Matrix

| Feature | Azure Files (SMB) | Azure Files (NFS) | Blob Storage |
|---|---|---|---|
| Protocol | SMB 2.1/3.x | NFSv4.1 | REST/HTTP |
| Media | SSD + HDD | SSD only | HDD (Standard) or SSD (Premium) |
| Identity auth | AD DS, Entra DS, Entra Kerberos | None (host-based) | Entra ID, SAS, account key |
| Access tiers | Transaction Optimized/Hot/Cool | Premium only | Hot/Cool/Cold/Archive |
| Snapshots | Share-level (max 200) | Share-level (max 200) | Blob-level (unlimited) |
| Soft delete | Share-level only | Share-level only | Blob-level + container-level |
| Versioning | No | No | Yes (automatic) |
| File Sync | Yes | No | No |

| Feature | Blob Soft Delete | Container Soft Delete | Blob Versioning |
|---|---|---|---|
| Protects against | Blob/snapshot/version deletion + overwrites | Container deletion | Blob modification + deletion |
| Scope | Individual blob | Entire container + contents | Individual blob (auto versions) |
| Retention | 1–365 days | 1–365 days | Until explicitly deleted |
| Restore limitation | Restores all snapshots/versions at once | Must restore to original name | Undelete restores all versions; must Copy Blob for current |

| Feature | Share Snapshots | File Share Soft Delete |
|---|---|---|
| Protects | Individual files within a share | Entire file share |
| Max count | 200 per share | N/A |
| Granularity | File-level retrieval | Share-level only |
| Default state | Manual | Enabled (7 days) |

---

## Decision Rules

| If... | Then... |
|---|---|
| Need file share for Linux-only workloads with POSIX semantics | Use **NFS** (requires premium/SSD + private endpoint) |
| Need file share accessible from Windows + Linux + macOS | Use **SMB** |
| Need Azure File Sync or Azure Backup for file shares | Use **SMB** (NFS not supported) |
| Need identity-based auth for file shares | Use **SMB** with AD DS / Entra DS / Entra Kerberos |
| Port 445 blocked for SMB | Use VPN + private endpoint, or Azure File Sync + SMB over QUIC (443) |
| Need to protect against accidental file share deletion | Enable **file share soft delete** |
| Need to recover individual deleted files in a share | Use **share snapshots** (or Azure Backup) |
| Need to auto-tier blobs based on age | Use **lifecycle management policy** |
| Need to rehydrate from archive | Use **Copy Blob** (avoids early deletion fee) or Set Blob Tier |
| Need protection against accidental blob deletion | Enable **blob soft delete + container soft delete + versioning** |
| Lifecycle policy keeps re-archiving rehydrated blobs | Add `daysAfterLastTierChangeGreaterThan` condition |
| Need to prevent anonymous blob access | Disable `AllowBlobPublicAccess` at **account level** |

---

## 5 Common Scenario Patterns

**Scenario 1: Replacing an on-premises file server**

> "Contoso wants to migrate their on-premises Windows file server to Azure Files with AD DS authentication and local caching."

- Create **SMB** file share on StorageV2 (HDD) or FileStorage (SSD)
- Enable **on-prem AD DS** identity-based auth (one source per account)
- Deploy **Azure File Sync** on Windows Server for local caching + cloud tiering
- Mount via `net use` with domain credentials; port 445 required or use VPN

**Scenario 2: Auto-tiering aging blobs to reduce costs**

> "A company stores daily log files in Hot tier but wants to automatically move them to Cool after 30 days, Archive after 90 days, and delete after 365 days."

- Create lifecycle management policy with 3 actions on `blockBlob`
- `tierToCool` at `daysAfterModificationGreaterThan: 30`
- `tierToArchive` at `daysAfterModificationGreaterThan: 90`
- `delete` at `daysAfterModificationGreaterThan: 365`
- Add `daysAfterLastTierChangeGreaterThan` to prevent re-archiving rehydrated blobs

**Scenario 3: Recovering a deleted container**

> "An admin accidentally deleted a blob container with important data."

- If **container soft delete** is enabled → restore within retention period (1–365 days)
- Must restore to **original name** — fails if name is already reused
- If not enabled → data is permanently lost (use resource locks to prevent account deletion)

**Scenario 4: Protecting blobs with versioning + soft delete**

> "A dev team wants automatic protection against accidental blob overwrites and deletions."

- Enable **blob versioning** — every write creates a previous version
- Enable **blob soft delete** (7+ days) — deleted versions are preserved
- Enable **container soft delete** — protects whole containers
- To restore after deletion: `Undelete Blob` → then `Copy Blob` to promote a previous version to current

**Scenario 5: NFS file share for Linux workloads**

> "A team needs a POSIX-compliant file share for a Linux-based containerized application."

- Create **FileStorage** (premium/SSD) account — NFS requires premium
- Create NFS share with `--enabled-protocols NFS`
- Configure **private endpoint** or service endpoint (NFS not internet-accessible)
- Redundancy limited to **LRS or ZRS** (no geo-redundancy for NFS)
- No Azure Backup or File Sync support for NFS

---

## Exam Traps

> **TRAP 1:** NFS requires **premium/SSD only** (FileStorage account), supports **LRS/ZRS only** (no GRS), has **no identity-based auth**, is **not internet-accessible**, and does NOT support Azure File Sync or Azure Backup.

> **TRAP 2:** Container public access requires **TWO settings** — account-level `AllowBlobPublicAccess` AND container-level access. Account-level disabled = overrides everything → 403.

> **TRAP 3:** Blob soft delete and container soft delete are **separate features** — enabling one does NOT enable the other. Neither protects against **storage account deletion** (use resource locks).

> **TRAP 4:** Container soft delete must restore to the **original name** — if that name is already in use by a new container, the restore **fails**.

> **TRAP 5:** Lifecycle management policies **CANNOT rehydrate** blobs from archive — they can only move blobs to cooler tiers or delete them.

> **TRAP 6:** Without `daysAfterLastTierChangeGreaterThan`, a lifecycle policy will **re-archive a rehydrated blob** because `Set Blob Tier` does not update last modified time.

> **TRAP 7:** With **versioning + soft delete** both enabled, overwriting a blob creates a **previous version, NOT a soft-deleted snapshot**. Undelete restores ALL versions but does NOT set a current version — must use Copy Blob.

> **TRAP 8:** Deleting a **previous blob version** requires **Storage Blob Data Owner** role — Storage Blob Data Contributor is NOT sufficient.

> **TRAP 9:** File share **soft delete protects shares, NOT individual files** — for individual file recovery, use **share snapshots** or Azure Backup.

> **TRAP 10:** Azure Backup forces file share soft delete retention to minimum **14 days** — resets if configured lower.

> **TRAP 11:** Share snapshots (max **200**) do **NOT count** toward share size quota, but deleting a file share **deletes ALL its snapshots**.

> **TRAP 12:** Only **ONE identity source** per storage account for Azure Files — cannot mix AD DS and Entra Kerberos on the same account.

---

## Rapid-Fire Q&A

**Q: What port does SMB use?**
A: TCP 445

**Q: What port does NFS use?**
A: TCP 2049

**Q: Can NFS file shares use identity-based authentication?**
A: No — host-based (network-level) only

**Q: What media tier does NFS require?**
A: SSD/premium only (FileStorage account)

**Q: What redundancy options does NFS support?**
A: LRS and ZRS only (no geo-redundancy)

**Q: How many snapshots can a file share have?**
A: 200 maximum

**Q: Do file share snapshots count toward share size quota?**
A: No

**Q: What does file share soft delete protect — shares or files?**
A: Shares only (use snapshots for individual files)

**Q: What is the default soft delete retention for new accounts?**
A: 7 days (enabled by default)

**Q: What minimum retention does Azure Backup enforce for file share soft delete?**
A: 14 days

**Q: What are the valid container public access levels?**
A: Private (default), Blob (read blobs only), Container (read + list blobs)

**Q: What are the container naming rules?**
A: 3–63 chars, lowercase letters + numbers + hyphens, no consecutive hyphens, start with letter/number

**Q: Can Archive be set as the default account access tier?**
A: No — only Hot, Cool, or Cold

**Q: What are the two methods to rehydrate from Archive?**
A: Copy Blob (recommended — avoids early deletion fee) and Set Blob Tier (in-place)

**Q: Can lifecycle management rehydrate blobs from Archive?**
A: No — it can only move to cooler tiers or delete

**Q: What is the max number of lifecycle management rules?**
A: 100 per policy

**Q: How often do lifecycle policies run?**
A: Once per day

**Q: Are blob soft delete and container soft delete the same feature?**
A: No — they are separate features that must be enabled independently

**Q: With versioning + soft delete, does overwriting create a soft-deleted snapshot?**
A: No — it creates a previous version instead

**Q: What role is needed to delete a previous blob version?**
A: Storage Blob Data Owner (not Contributor)

**Q: What is the recommended data protection configuration?**
A: Enable all three: container soft delete + blob soft delete + blob versioning

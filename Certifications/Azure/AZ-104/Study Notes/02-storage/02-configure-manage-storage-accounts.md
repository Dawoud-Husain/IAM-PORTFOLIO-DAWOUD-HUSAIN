# 2.2 — Configure and Manage Storage Accounts

**Domain Weight: 15–20% (MEDIUM PRIORITY)**

---

## Core Concepts

### Create and Configure Storage Accounts

An Azure Storage account is a top-level resource that contains all Azure Storage data objects: blobs, files, queues, and tables. Each account has a globally unique name and a specific type/performance tier that determines capabilities and pricing. You configure redundancy, access tiers, networking, and encryption at creation time — some settings cannot be changed later.

**Storage Account Types (kind):**

| Type | Use Case | Redundancy | Hierarchical NS |
|---|---|---|---|
| **Standard GPv2 (StorageV2)** | Recommended for most scenarios | All options | Yes |
| **Premium Block Blobs** | High transactions, low latency | LRS, ZRS only | Yes |
| **Premium File Shares** | Enterprise file shares (SMB/NFS) | LRS, ZRS only | No |
| **Premium Page Blobs** | VHDs for unmanaged disks | LRS only | No |

- **Legacy types** (GPv1, BlobStorage) — deprecated, always choose GPv2
- You **CANNOT change** account type (kind) after creation
- You **CANNOT convert** between Standard and Premium

**Storage Account Naming Rules:**

- **3–24 characters** long
- **Lowercase letters and numbers only** (no hyphens, underscores, uppercase)
- Must be **globally unique** across all of Azure

**Performance Tiers:**

- **Standard** — HDDs, general-purpose, recommended for most workloads
- **Premium** — SSDs, low latency; choose sub-type: block blobs, file shares, or page blobs

**Access Tiers (Block Blobs Only):**

| Tier | Optimized For | Min Retention | Latency | Availability |
|---|---|---|---|---|
| **Hot** | Frequent access | None | ms | 99.9% |
| **Cool** | Infrequent (30+ days) | **30 days** | ms | 99% |
| **Cold** | Rare (90+ days) | **90 days** | ms | 99% |
| **Archive** | Rarely accessed | **180 days** | **Hours** | 99% |

- Default account tier = **Hot** (can set to Cool or Cold, NOT Archive)
- Archive = **offline** — must rehydrate before read/modify (up to 15 hours)
- Archive metadata remains readable (read-only)
- Archive NOT supported on ZRS, GZRS, or RA-GZRS accounts
- Access tiers apply to **block blobs only** (not append or page blobs)
- **Early deletion charges** if removed before minimum retention (prorated)

**Standard Endpoints:**

- Blob: `https://<account>.blob.core.windows.net`
- Files: `https://<account>.file.core.windows.net`
- Queue: `https://<account>.queue.core.windows.net`
- Table: `https://<account>.table.core.windows.net`
- Data Lake: `https://<account>.dfs.core.windows.net`

---

### Configure Azure Storage Redundancy

Azure Storage always maintains at least 3 copies of your data. Redundancy protects against hardware failures, outages, and disasters. The option you choose trades off between cost, availability, and durability. Primary-region options keep data in one region; geo-redundant options replicate asynchronously to a paired secondary region.

**Primary Region Options:**

| Option | Copies | Location | Durability |
|---|---|---|---|
| **LRS** | 3 | Single datacenter | 11 nines |
| **ZRS** | 3 | 3 availability zones | 12 nines |

**Geo-Redundant Options:**

| Option | Copies | Primary | Secondary | Durability | Read from Secondary? |
|---|---|---|---|---|---|
| **GRS** | 6 | LRS (1 DC) | LRS (1 DC) | 16 nines | No (failover only) |
| **RA-GRS** | 6 | LRS (1 DC) | LRS (1 DC) | 16 nines | **Yes** |
| **GZRS** | 6 | ZRS (3 zones) | LRS (1 DC) | 16 nines | No (failover only) |
| **RA-GZRS** | 6 | ZRS (3 zones) | LRS (1 DC) | 16 nines | **Yes** |

- Secondary region data is **always stored using LRS** (even in GZRS)
- Secondary region is **fixed** (Azure-paired) — cannot be changed
- RA-* = read access to secondary endpoint: `<account>-secondary.blob.core.windows.net`
- RA-* read availability = **99.99%** vs 99.9% for non-RA

**Mnemonic — "LZG": Layers of protection**
- **L**RS = Local (1 datacenter, 3 copies)
- **Z**RS = Zones (3 zones, 3 copies)
- **G** prefix = Geo (adds 3 more copies in secondary region = 6 total)

**Account Type vs. Redundancy:**

| Account Type | LRS | ZRS | GRS/RA-GRS | GZRS/RA-GZRS |
|---|---|---|---|---|
| Standard GPv2 | Yes | Yes | Yes | Yes |
| Premium Block Blobs | Yes | Yes | No | No |
| Premium File Shares | Yes | Yes | No | No |
| Premium Page Blobs | Yes | No | No | No |

**Failover Behavior:**

- Geo-replication is **asynchronous** — potential data loss (RPO ~15 min for block blobs)
- After **unplanned failover** → account converts to **LRS** in new primary (must re-enable geo manually)
- After **planned failover** → geo-redundancy maintained
- Must wait **72 hours** between redundancy conversions
- Azure Files does **NOT** support RA-GRS or RA-GZRS

---

### Configure Object Replication

Object replication asynchronously copies block blobs between a source and destination storage account. It uses replication policies with rules that map source containers to destination containers. This is useful for minimizing latency across regions, distributing data, or optimizing costs by tiering replicated data.

**Prerequisites:**

- Both accounts must be **GPv2 or Premium Block Blob**
- **Blob versioning** enabled on **BOTH** source and destination
- **Change feed** enabled on **SOURCE** only
- Supports **block blobs ONLY** (not append, not page)
- Does **NOT** support hierarchical namespace (Data Lake Storage)

**Key Rules:**

- Each source can replicate to max **2 destination accounts**
- Each destination can have max **2 replication policies**
- Max **1,000 rules** per policy (only **10 via portal** — use JSON for more)
- Destination container is **write-protected** while policy is active (409 Conflict)
- Read and delete operations on destination ARE permitted
- Blob **snapshots are NOT replicated**
- Blob **version IDs are NOT preserved** (new IDs at destination)
- Blobs in **archive tier CANNOT be replicated**
- Customer-managed **failover NOT supported** for either account
- Cross-tenant replication disabled by default (accounts after Dec 15, 2023)

**Copy Scope Options:**

- New objects only (default — after rule creation)
- All objects in the container
- Custom date/time cutoff
- Prefix filter (up to 5 prefixes per rule)

---

### Configure Storage Account Encryption

Azure Storage automatically encrypts all data at rest using 256-bit AES (SSE). This cannot be disabled. You choose who manages the encryption keys: Microsoft or you. Infrastructure encryption adds a second layer. Encryption in transit enforces HTTPS/TLS for all requests.

**Key Management Options:**

| Feature | Microsoft-Managed | Customer-Managed (CMK) | Customer-Provided |
|---|---|---|---|
| Services | All | Blob, Files (Queue/Table special) | Blob only |
| Key stored in | Microsoft | Azure Key Vault / HSM | Customer's store |
| Key rotation | Automatic (Microsoft) | Customer responsibility | Customer |

**CMK Requirements:**

- Key Vault must have **soft delete AND purge protection** enabled
- Key types: RSA or RSA-HSM, sizes **2048, 3072, 4096**
- Managed identity needs permissions: `wrapkey`, `unwrapkey`, `get`
- **New account** + CMK = must use **user-assigned managed identity**
- **Existing account** = can use user-assigned or system-assigned
- Azure checks for new key versions **once daily** (wait 24 hours before disabling old key)
- Revoking key access → all operations fail with **403 Forbidden**

**Infrastructure Encryption (Double Encryption):**

- Encrypts data twice with **two different algorithms and keys**
- Infrastructure layer always uses **Microsoft-managed keys**
- Must be enabled **at account creation** — cannot be added later
- Only needed for compliance requirements

**Encryption Scopes:**

- Scope encryption to a **container or individual blob**
- Up to **10,000 scopes** per account
- Cannot be **deleted**, only disabled
- Billed for minimum **30 days** per scope
- Blobs with encryption scopes **cannot use archive tier**

**Encryption in Transit:**

- **Secure transfer required** = HTTPS only (enabled by default)
- HTTP requests are **rejected** when enabled
- Minimum TLS version default = **TLS 1.2**
- SMB requires **3.x with AES encryption** when secure transfer is on

---

### Manage Data by Using Azure Storage Explorer and AzCopy

AzCopy is a command-line tool for bulk data transfers to/from Azure Storage. Storage Explorer is a free GUI desktop app for interactive storage management. Both run on Windows, macOS, and Linux. Storage Explorer actually uses AzCopy internally for copy operations.

**AzCopy Key Commands:**

| Command | Purpose |
|---|---|
| `azcopy copy` | Copy data (never deletes at destination) |
| `azcopy sync` | One-way sync (can delete at destination) |
| `azcopy make` | Create container or file share |
| `azcopy remove` | Delete blobs or files |
| `azcopy login` | Authenticate via Entra ID |
| `azcopy list` | List entities in a resource |

**Copy vs. Sync — Critical Differences:**

| Feature | `azcopy copy` | `azcopy sync` |
|---|---|---|
| Deletes at destination | **Never** | Yes (with `--delete-destination=true`) |
| Direction | Any | **One-way** |
| Indexes source/dest | No | Yes (more memory, more cost) |
| Best for | One-time bulk copies | Ongoing synchronization |

**AzCopy Authentication:**

- **Entra ID** (preferred) — `azcopy login` / managed identity / service principal
- **SAS token** — append to each URL
- Required RBAC: Storage Blob Data Reader (download), Storage Blob Data Contributor (upload)

**Storage Explorer Features:**

- GUI for managing blobs, files, queues, tables, Data Lake
- Generate SAS tokens (always signed with account key, never user delegation)
- Manage stored access policies
- Connect via: Entra ID, account key, SAS, public/anonymous

**Mnemonic — AzCopy "CSMRL": Copy, Sync, Make, Remove, Login**

---

## What The Exam Tests

- Storage account naming rules (3–24 chars, lowercase + numbers only, globally unique)
- Which settings CANNOT be changed after creation (account type, performance tier, infrastructure encryption)
- Access tier minimum retention periods (Cool=30, Cold=90, Archive=180)
- Archive tier restrictions (offline, cannot use with ZRS/GZRS, not a default tier)
- Redundancy copy counts (LRS/ZRS=3, GRS/GZRS=6)
- Secondary region always uses LRS (even in GZRS)
- RA-GRS/RA-GZRS vs GRS/GZRS (read access to secondary)
- Azure Files does NOT support RA-GRS or RA-GZRS
- Premium accounts only support LRS/ZRS (no geo options)
- Object replication prerequisites (versioning both, change feed source, block blobs only)
- CMK requires Key Vault with soft delete + purge protection
- `azcopy copy` vs `azcopy sync` behavioral differences
- Storage Explorer SAS always uses account key

---

## Important Limits / Numbers

| Item | Value |
|---|---|
| Storage account name length | **3–24 characters** |
| Max storage accounts per region per sub | **250** (500 with quota increase) |
| Max storage account capacity | **5 PiB** |
| Cool tier min retention | **30 days** |
| Cold tier min retention | **90 days** |
| Archive tier min retention | **180 days** |
| Archive rehydration time | **Up to 15 hours** (standard) |
| LRS copies | **3** (1 datacenter) |
| ZRS copies | **3** (3 availability zones) |
| GRS/GZRS copies | **6** (3 primary + 3 secondary) |
| Wait between redundancy changes | **72 hours** |
| Geo-replication RPO (block blobs) | **~15 minutes** |
| Object replication max destinations per source | **2** |
| Object replication max rules per policy | **1,000** (10 via portal) |
| Encryption scopes per account | **10,000** |
| Encryption scope min billing | **30 days** |
| CMK key version check frequency | **Once daily (24 hours)** |
| AzCopy optimal file count per job | **< 10 million** |
| Max IP address rules per account | **400** |
| Max private endpoints per account | **200** |

---

## CLI + PowerShell Commands

### Azure CLI

```bash
# Create storage account
az storage account create \
  --name mystorageacct \
  --resource-group myRG \
  --location eastus \
  --sku Standard_LRS \
  --kind StorageV2 \
  --access-tier Hot \
  --min-tls-version TLS1_2 \
  --allow-blob-public-access false

# Enable infrastructure encryption
az storage account create \
  --name mystorageacct \
  --resource-group myRG \
  --location eastus \
  --sku Standard_RAGRS \
  --kind StorageV2 \
  --require-infrastructure-encryption

# Enable secure transfer
az storage account update \
  --name mystorageacct \
  --resource-group myRG \
  --https-only true

# Create container with encryption scope
az storage container create \
  --account-name mystorageacct \
  --name mycontainer \
  --default-encryption-scope myscope \
  --prevent-encryption-scope-override true
```

### AzCopy

```bash
# Login with Entra ID
azcopy login

# Login with managed identity
azcopy login --identity

# Copy local file to blob
azcopy copy 'C:\data\file.txt' \
  'https://acct.blob.core.windows.net/container/file.txt'

# Copy with SAS token
azcopy copy 'C:\data' \
  'https://acct.blob.core.windows.net/container?<SAS>' \
  --recursive=true

# Server-to-server copy between accounts
azcopy copy \
  'https://source.blob.core.windows.net/container/*' \
  'https://dest.blob.core.windows.net/container/'

# Sync local to blob (one-way, can delete)
azcopy sync 'C:\data' \
  'https://acct.blob.core.windows.net/container' \
  --delete-destination=true

# Copy to specific access tier
azcopy copy 'source' 'dest' --block-blob-tier=Cool

# Create container
azcopy make 'https://acct.blob.core.windows.net/newcontainer'

# Remove blobs
azcopy remove 'https://acct.blob.core.windows.net/container/blob.txt'
```

### PowerShell

```powershell
# Create storage account
New-AzStorageAccount -ResourceGroupName "myRG" `
  -Name "mystorageacct" `
  -Location "eastus" `
  -SkuName "Standard_RAGRS" `
  -Kind "StorageV2" `
  -AllowBlobPublicAccess $false `
  -MinimumTlsVersion "TLS1_2"

# Create with infrastructure encryption
New-AzStorageAccount -ResourceGroupName "myRG" `
  -Name "mystorageacct" `
  -Location "eastus" `
  -SkuName "Standard_RAGRS" `
  -Kind "StorageV2" `
  -RequireInfrastructureEncryption

# Create encryption scope
New-AzStorageEncryptionScope -ResourceGroupName "myRG" `
  -StorageAccountName "mystorageacct" `
  -EncryptionScopeName "myscope" `
  -StorageEncryption

# Get object replication policy
Get-AzStorageObjectReplicationPolicy `
  -ResourceGroupName "myRG" `
  -StorageAccountName "destacct"
```

---

## Confusion Matrix

| Feature | Standard GPv2 | Premium Block Blob | Premium File Shares | Premium Page Blobs |
|---|---|---|---|---|
| Disk type | HDD | SSD | SSD | SSD |
| Redundancy | All | LRS, ZRS | LRS, ZRS | LRS only |
| Access tiers | Hot/Cool/Cold/Archive | N/A (Premium only) | N/A | N/A |
| Use case | General purpose | High transaction blobs | Enterprise file shares | Unmanaged VM disks |

| Feature | LRS | ZRS | GRS | RA-GRS | GZRS | RA-GZRS |
|---|---|---|---|---|---|---|
| Copies | 3 | 3 | 6 | 6 | 6 | 6 |
| Zones | No | 3 | No | No | 3 (primary) | 3 (primary) |
| Regions | 1 | 1 | 2 | 2 | 2 | 2 |
| Read secondary | No | No | No | **Yes** | No | **Yes** |
| Archive support | Yes | No | Yes | Yes | No | No |

| Feature | `azcopy copy` | `azcopy sync` |
|---|---|---|
| Deletes destination files | Never | Yes (optional) |
| Indexes before transfer | No | Yes |
| Memory usage | Lower | Higher |
| Cost | Lower | Higher |
| Best for | Bulk one-time | Ongoing sync |

| Feature | AzCopy | Storage Explorer |
|---|---|---|
| Interface | CLI | GUI |
| Best for | Automation, bulk | Interactive browsing |
| Generates SAS | No | Yes (account key only) |
| Sync capability | Yes | No |
| Supports S3/GCS | Yes (source) | No |

---

## Decision Rules

| If... | Then... |
|---|---|
| Need general-purpose storage | Use **Standard GPv2 (StorageV2)** |
| Need low-latency block blob access | Use **Premium Block Blob** account |
| Need maximum durability + zone protection | Use **GZRS or RA-GZRS** |
| Need read access during regional outage | Use **RA-GRS or RA-GZRS** |
| Data rarely accessed, can tolerate hours latency | Use **Archive** tier |
| Need to enforce HTTPS only | Enable **Secure transfer required** (default) |
| Need encryption key control | Use **CMK** with Azure Key Vault |
| Need double encryption for compliance | Enable **Infrastructure encryption** at creation |
| Need ongoing data sync between accounts | Use **object replication** (block blobs) or **azcopy sync** |
| Need bulk automated transfers | Use **AzCopy** |
| Need interactive storage browsing | Use **Storage Explorer** |
| Need to replicate blobs across regions | Use **object replication** (versioning + change feed required) |

---

## 5 Common Scenario Patterns

**Scenario 1: Choosing redundancy for a critical app**

> "Contoso needs maximum availability for a web app with blob storage. Users in the secondary region should be able to read data during a primary region outage."

- Use **RA-GZRS** — zone redundancy in primary + read access to secondary
- 6 copies, 16 nines durability, 99.99% read availability
- Secondary endpoint: `<account>-secondary.blob.core.windows.net`

**Scenario 2: Cost-optimized archival storage**

> "A company must retain regulatory documents for 7 years. Data is accessed less than once a year."

- Use **Standard GPv2** with **LRS or GRS** (Archive NOT supported on ZRS)
- Set blob tier to **Archive** (lowest storage cost, 180-day min retention)
- Use **lifecycle management** to auto-tier from Hot → Archive
- Rehydrate to Hot/Cool when needed (up to 15 hours)

**Scenario 3: Replicating data to a secondary region for analytics**

> "Contoso processes data in East US but analysts in West Europe need access to the same blobs."

- Configure **object replication** from East US (source) to West Europe (destination)
- Enable **blob versioning** on both accounts, **change feed** on source
- Only block blobs are replicated
- Destination container is read-only while policy is active

**Scenario 4: Migrating large dataset to Azure**

> "An admin needs to upload 500 GB of on-premises data to Azure Blob Storage nightly."

- Use **AzCopy sync** with `--delete-destination=true` for ongoing sync
- Authenticate via **managed identity** or **SAS token**
- Enable **soft delete** before using delete-destination flag
- For first-time bulk copy, `azcopy copy --recursive` is faster (no indexing)

**Scenario 5: Customer-managed encryption keys**

> "A healthcare company requires full control over encryption keys for compliance."

- Create Azure Key Vault with **soft delete + purge protection** enabled
- Use **user-assigned managed identity** (required for new accounts)
- Grant identity: `wrapkey`, `unwrapkey`, `get` permissions on Key Vault
- Enable **infrastructure encryption** at account creation for double encryption
- CMK covers Blob and Files; Queue/Table need special account configuration

---

## Exam Traps

> **TRAP 1:** You **CANNOT change** the storage account type (kind) or performance tier (Standard/Premium) after creation — must create a new account and migrate data.

> **TRAP 2:** Archive tier is **NOT a valid default account access tier** — only Hot, Cool, or Cold. Archive is set per-blob only.

> **TRAP 3:** Archive tier is **NOT supported** on ZRS, GZRS, or RA-GZRS accounts — only LRS, GRS, RA-GRS.

> **TRAP 4:** Secondary region always uses **LRS** — even in GZRS, the secondary is NOT zone-redundant.

> **TRAP 5:** Azure Files does **NOT support RA-GRS or RA-GZRS** — file shares configured as RA-GRS are billed/treated as GRS.

> **TRAP 6:** After **unplanned failover**, the account converts to **LRS** — geo-redundancy is lost and must be re-enabled manually.

> **TRAP 7:** Premium accounts (Block Blob, File Shares) only support **LRS and ZRS** — no geo-redundancy options. Premium Page Blobs = **LRS only**.

> **TRAP 8:** Object replication requires **versioning on BOTH accounts** but change feed on **source only**. Destination container is **write-protected** (409 error on writes).

> **TRAP 9:** `azcopy copy` **never deletes** destination files — only `azcopy sync` with `--delete-destination` can delete. Sync is **one-way only**.

> **TRAP 10:** Infrastructure encryption (double encryption) must be enabled **at account creation time** — it CANNOT be added to an existing account.

> **TRAP 11:** CMK on a **new storage account** requires **user-assigned managed identity** — system-assigned is NOT supported at creation.

> **TRAP 12:** Storage account names: **lowercase + numbers ONLY** (no hyphens), **3–24 chars**, **globally unique**.

---

## Rapid-Fire Q&A

**Q: What characters are allowed in a storage account name?**
A: Lowercase letters and numbers only — no hyphens, underscores, or uppercase (3–24 chars)

**Q: Can you change a storage account from Standard to Premium?**
A: No — must create a new account and migrate data

**Q: What is the default access tier for a new GPv2 account?**
A: Hot

**Q: Can Archive be set as the default account access tier?**
A: No — only Hot, Cool, or Cold

**Q: What is the minimum retention for Archive tier?**
A: 180 days (early deletion charges apply)

**Q: How many copies does LRS maintain?**
A: 3 copies in a single datacenter

**Q: How many copies does GRS maintain?**
A: 6 copies (3 primary + 3 secondary)

**Q: What redundancy does the secondary region always use?**
A: LRS (even in GZRS)

**Q: Can you choose your secondary region in GRS?**
A: No — Azure determines the paired region automatically

**Q: What happens to redundancy after an unplanned failover?**
A: Account becomes LRS — must manually re-enable geo-redundancy

**Q: Does Azure Files support RA-GRS?**
A: No — treated as GRS

**Q: What is required for object replication?**
A: Versioning on both accounts, change feed on source, block blobs only, GPv2 or Premium Block Blob accounts

**Q: Can you write to the destination container during object replication?**
A: No — writes return 409 Conflict (reads and deletes are allowed)

**Q: What is the difference between `azcopy copy` and `azcopy sync`?**
A: Copy never deletes at destination; sync can delete and is one-way only

**Q: What authentication does Storage Explorer use for SAS generation?**
A: Always account key (never user delegation SAS)

**Q: Can SSE (encryption at rest) be disabled?**
A: No — always enabled on all storage accounts

**Q: When must infrastructure encryption be enabled?**
A: At storage account creation time — cannot be added later

**Q: What managed identity type is required for CMK on a NEW storage account?**
A: User-assigned managed identity (system-assigned not supported at creation)

**Q: What Key Vault settings are required for CMK?**
A: Both soft delete AND purge protection must be enabled

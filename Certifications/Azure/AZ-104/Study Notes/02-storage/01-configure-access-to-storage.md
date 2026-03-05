# 2.1 — Configure Access to Storage

**Domain Weight: 15–20% (MEDIUM PRIORITY)**

---

## Core Concepts

### Configure Azure Storage Firewalls and Virtual Networks

Azure Storage firewalls let you restrict which networks can reach your storage account's data plane. By default, storage accounts accept connections from any network — you must explicitly set the default action to **Deny** and then whitelist specific VNets, IP ranges, or trusted Azure services. Firewall rules apply to data plane operations only (REST, SMB); control plane (ARM) operations are not affected.

**Default Action:**

- Must set to **Deny** for any network rules to take effect
- If set to **Allow** (default), all traffic is permitted regardless of rules

**Four Types of Network Rules:**

| Rule Type | What It Allows | Limit |
|-----------|---------------|-------|
| **VNet rules** | Traffic from specific subnets (via service endpoints) | **200 VNet rules** |
| **IP rules** | Traffic from specific public IP ranges | **200 IP rules** |
| **Resource instance rules** | Traffic from specific Azure resource instances | Per-resource |
| **Trusted services exception** | Traffic from Azure trusted services list | Toggle on/off |

**IP Rule Restrictions (EXAM CRITICAL):**

- Only **public internet** IPs allowed — NO private ranges (10.x, 172.16–31.x, 192.168.x)
- CIDR format (e.g., 16.17.18.0/24) or individual IPs
- /31 and /32 prefix sizes **NOT supported** — use individual IP addresses instead
- **IPv4 only** — no IPv6
- IP rules have **NO effect** on requests from the **same Azure region** — use VNet rules instead

**Service Endpoints:**

- Enable `Microsoft.Storage` on the subnet to route storage traffic over Azure backbone
- When enabled, source IP switches from **public to private** — existing IP-based rules for that subnet stop working
- Service endpoint routes **override BGP and UDR** for the service address prefix

**Trusted Azure Services:**

- Exception: "Allow Azure services on the trusted services list"
- Includes: Azure Backup, Azure Monitor, Site Recovery, Event Grid, Data Factory, Key Vault, etc.
- Trusted services use **strong authentication** (managed identity)
- Takes **highest precedence** — works even if public network access is **Disabled**

**Subnet Deletion Trap:**

- Deleting a subnet **removes** it from storage VNet rules
- Recreating a subnet with the same name does **NOT** restore access — must re-add the rule

---

### Create and Use Shared Access Signature (SAS) Tokens

A Shared Access Signature (SAS) is a URI that grants restricted access to Azure Storage resources with specific permissions, time window, and IP constraints. SAS tokens let you delegate access without sharing account keys. There are three types — user delegation, service, and account — each signed differently and with different capabilities.

**Three Types of SAS:**

| Type | Signed With | Scope | Stored Access Policy? |
|------|-------------|-------|----------------------|
| **User delegation SAS** | Entra credentials (user delegation key) | Blob Storage only | **NO** |
| **Service SAS** | Storage account key | One service (Blob, Queue, Table, or Files) | **YES** |
| **Account SAS** | Storage account key | One or more services + service-level ops | **NO** |

> **Microsoft recommendation:** Always use **user delegation SAS** when possible — secured with Entra credentials, not account keys.

**User Delegation SAS Key Facts:**

- Must first request a **user delegation key** via API
- Maximum key validity: **7 days**
- Permissions granted = **intersection** of RBAC permissions AND SAS token permissions
- Not affected by access key rotation

**SAS Token Key Parameters:**

| Parameter | Code | Meaning |
|-----------|------|---------|
| Signed Services | `ss` | b=Blob, q=Queue, t=Table, f=File |
| Signed Permissions | `sp` | r=Read, w=Write, d=Delete, l=List, a=Add, c=Create |
| Expiry Time | `se` | UTC datetime |
| Signed IP | `sip` | Allowed IP range |
| Signed Protocol | `spr` | https or https,http |
| Signature | `sig` | HMAC SHA256 + Base64 |

**Two Forms:**

- **Ad hoc SAS** — all constraints (start, expiry, permissions) in the URI itself
- **SAS with stored access policy** — constraints inherited from server-side policy (service SAS only)

**Best Practices:**

- Always use **HTTPS**
- Set start time **15 minutes in the past** (clock skew)
- Use **near-term expiration** for ad hoc SAS
- Grant **least privilege**
- Azure Storage does NOT track SAS tokens — **you** must track and manage them

---

### Configure Stored Access Policies

Stored access policies are server-side definitions attached to a container, file share, queue, or table that control the start time, expiry, and permissions for service SAS tokens. They are the only way to modify or revoke a SAS after it has been issued without rotating the storage account key.

**Supported On:**

- Blob containers, File shares, Queues, Tables

**Key Limits:**

- Maximum **5 stored access policies** per container/share/queue/table
- Signed identifier: up to **64 characters**
- Propagation delay: up to **30 seconds** after create/update (requests may return 403 during this window)

**Parameter Placement Rule:**

- A parameter can be on the SAS URI **OR** the stored access policy, but **NOT on both**
- Options: all on SAS, all on policy, or split (never duplicate)

**Revocation Methods:**

| Method | Effect |
|--------|--------|
| **Delete** the policy | Immediately revokes all associated SAS |
| **Rename** (change signed identifier) | Breaks association with existing SAS |
| **Set expiry to the past** | Causes associated SAS to expire |

> **EXAM TRAP:** Recreating a deleted policy with the **same name** restores access — all existing SAS tokens become valid again. To truly revoke, use a **different name**.

---

### Manage Access Keys

Every Azure Storage account has two 512-bit access keys (key1 and key2) that grant full access to all data in the account. Two keys exist so you can rotate one while applications use the other, enabling zero-downtime key rotation. Access keys are the least-secure authorization method — Microsoft recommends Entra ID with managed identities instead.

**Security Hierarchy (best → worst):**

1. **Entra ID + managed identities** (best)
2. **User delegation SAS**
3. **Service/Account SAS with stored access policies**
4. **Storage account access keys** (least secure)

**Key Rotation Process:**

1. Update all apps to use **key2** (secondary)
2. Regenerate **key1** (primary)
3. Update all apps to use **new key1**
4. Regenerate **key2** (secondary)

> **CRITICAL:** Rotating a key **revokes all service SAS and account SAS** generated with that key. User delegation SAS is NOT affected.

**Key Expiration Policy:**

- Set a reminder/compliance period for rotation (e.g., 60 days)
- Monitor via Azure Policy: "Storage account keys should not be expired"
- Cannot set if `KeyCreationTime` is null — rotate each key once first

**Disabling Shared Key Access:**

- Can disallow Shared Key authorization entirely on the storage account
- When disabled: **service SAS + account SAS become invalid**
- Only user delegation SAS and Entra ID authentication work
- Required for using **Entra Conditional Access** policies on storage

**Azure Key Vault Integration:**

- Key Vault can manage and auto-rotate storage account keys
- Key Vault needs **Storage Account Key Operator Service Role** on the storage account
- Do NOT manually regenerate keys if Key Vault manages them

---

### Configure Identity-Based Access for Azure Files

Azure Files supports identity-based authentication over SMB using Kerberos, allowing users to access file shares with their organizational credentials instead of storage account keys. You configure one of three identity sources per storage account, then assign share-level RBAC permissions and directory/file-level NTFS ACLs. This is the recommended approach for enterprise file share access.

**Three Identity Sources (choose ONE per storage account):**

| Source | Requirements | Supported Identities |
|--------|-------------|---------------------|
| **On-premises AD DS** | Domain-join storage acct; sync to Entra via Connect | Hybrid only |
| **Entra Domain Services** | Enable managed domain; domain-join VMs | Cloud-only + hybrid |
| **Entra Kerberos** | Entra-joined or hybrid-joined clients | Hybrid + cloud-only (preview) |

**Two-Layer Permission Model:**

1. **Share-level** — Azure RBAC roles (gatekeeper)
2. **Directory/file-level** — Windows ACLs / NTFS permissions (granular)
3. **Most restrictive** wins when both layers are evaluated

**Azure RBAC Roles for Azure Files:**

| Role | Access Level |
|------|-------------|
| Storage File Data SMB Share **Reader** | Read |
| Storage File Data SMB Share **Contributor** | Read, write, delete |
| Storage File Data SMB Share **Elevated Contributor** | Read, write, delete, **modify ACLs** |

**Default Share-Level Permission:**

- Applies to ALL authenticated identities — no sync requirement
- Set at storage account level, applies to all file shares
- If both default + specific RBAC assigned → user gets the **higher** permission

**NTFS ACL Setup:**

- Must first mount share with **storage account key** for initial ACL configuration
- Use `net use` command (NOT PowerShell mount) to connect
- Use **icacls** or Windows File Explorer to set ACLs
- Share-level RBAC permissions take up to **30 minutes** to propagate

**Mnemonic — Storage Access Methods: "KUSA"**

- **K**eys — full access, least secure
- **U**ser delegation SAS — Entra-backed, most secure SAS
- **S**ervice/Account SAS — key-signed, use stored access policies
- **A**CL + RBAC — identity-based for Azure Files

---

## What The Exam Tests

- When to set default action to Deny (before network rules take effect)
- IP rule restrictions (no private IPs, no /31 or /32, IPv4 only, no same-region effect)
- Which SAS type supports stored access policies (service SAS only)
- User delegation SAS max key validity (7 days)
- How to revoke a SAS (delete/rename stored access policy or rotate key)
- Max stored access policies per container (5)
- Key rotation impact on SAS tokens (revokes service/account SAS)
- Security hierarchy for storage access (Entra ID > user delegation > SAS > keys)
- Disabling Shared Key = service SAS + account SAS invalid
- Azure Files identity sources — only ONE per storage account
- Two-layer permission model (RBAC + NTFS ACLs, most restrictive wins)
- Share-level RBAC roles for Azure Files (Reader, Contributor, Elevated Contributor)

---

## Important Limits / Numbers

| Item | Value |
|------|-------|
| VNet rules per storage account | **200** |
| IP rules per storage account | **200** |
| IP rule prefix — NOT supported | **/31 and /32** (use individual IPs) |
| User delegation key max validity | **7 days** |
| Stored access policies per container/share/queue/table | **5** |
| Stored access policy identifier max length | **64 characters** |
| Policy propagation delay | **Up to 30 seconds** |
| Access keys per storage account | **2** (key1 + key2) |
| Access key size | **512-bit** |
| Share-level RBAC propagation | **Up to 30 minutes** |
| Identity sources per storage account (Azure Files) | **1 only** |
| SAS start time offset (clock skew) | **15 minutes in the past** |

---

## CLI + PowerShell Commands

### Azure CLI

```bash
# --- Storage Firewall ---
# Set default action to Deny
az storage account update --name <sa> --resource-group <rg> --default-action Deny

# Add IP rule
az storage account network-rule add --account-name <sa> --resource-group <rg> --ip-address "16.17.18.0/24"

# Add VNet rule
az storage account network-rule add --account-name <sa> --resource-group <rg> --subnet <subnet-id>

# Configure trusted services bypass
az storage account update --name <sa> --resource-group <rg> --bypass Logging Metrics AzureServices

# --- SAS Tokens ---
# Generate blob SAS
az storage blob generate-sas --account-name <sa> --container-name <container> \
  --name <blob> --permissions rw --expiry 2025-01-01T00:00:00Z

# Generate container SAS
az storage container generate-sas --account-name <sa> --name <container> \
  --permissions rwdl --expiry 2025-01-01T00:00:00Z

# --- Stored Access Policies ---
# Create stored access policy
az storage container policy create --container-name <container> --name <policy> \
  --permissions rwdl --expiry 2025-12-31T00:00:00Z --account-name <sa>

# Delete stored access policy (revokes all associated SAS)
az storage container policy delete --container-name <container> --name <policy> --account-name <sa>

# --- Access Keys ---
# List access keys
az storage account keys list --resource-group <rg> --account-name <sa>

# Regenerate primary key
az storage account keys renew --resource-group <rg> --account-name <sa> --key primary

# Regenerate secondary key
az storage account keys renew --resource-group <rg> --account-name <sa> --key secondary

# Set key expiration policy
az storage account update --name <sa> --resource-group <rg> --key-exp-days 60

# --- Azure Files Identity ---
# Set default share-level permission
az storage account update --name <sa> --resource-group <rg> \
  --default-share-permission StorageFileDataSmbShareContributor
```

### PowerShell

```powershell
# --- Storage Firewall ---
# Set default deny
Update-AzStorageAccountNetworkRuleSet -ResourceGroupName <rg> -Name <sa> -DefaultAction Deny

# Add IP rule
Add-AzStorageAccountNetworkRule -ResourceGroupName <rg> -AccountName <sa> -IPAddressOrRange "16.17.18.0/24"

# Configure bypass
Update-AzStorageAccountNetworkRuleSet -ResourceGroupName <rg> -Name <sa> -Bypass AzureServices,Metrics,Logging

# --- SAS Tokens ---
# Generate blob SAS
$ctx = New-AzStorageContext -StorageAccountName <sa> -StorageAccountKey <key>
New-AzStorageBlobSASToken -Container "mycontainer" -Blob "myblob" -Permission rwd `
  -ExpiryTime (Get-Date).AddHours(2) -Context $ctx

# User delegation SAS (OAuth)
$ctx = New-AzStorageContext -StorageAccountName <sa> -UseConnectedAccount
New-AzStorageBlobSASToken -Container "mycontainer" -Blob "myblob" -Permission rwd `
  -ExpiryTime (Get-Date).AddHours(2) -Context $ctx

# --- Stored Access Policies ---
# Create stored access policy
New-AzStorageContainerStoredAccessPolicy -Container "mycontainer" -Policy "mypolicy" `
  -Permission rwdl -ExpiryTime (Get-Date).AddYears(1) -Context $ctx

# Generate SAS from stored access policy
New-AzStorageBlobSASToken -Container "mycontainer" -Blob "myblob" -Policy "mypolicy" -Context $ctx

# --- Access Keys ---
# Get access keys
Get-AzStorageAccountKey -ResourceGroupName <rg> -Name <sa>

# Regenerate key1
New-AzStorageAccountKey -ResourceGroupName <rg> -Name <sa> -KeyName key1

# Set key expiration policy
Set-AzStorageAccount -ResourceGroupName <rg> -Name <sa> -KeyExpirationPeriodInDay 60

# --- Azure Files Identity ---
# Enable Entra Kerberos
Set-AzStorageAccount -ResourceGroupName <rg> -AccountName <sa> `
  -EnableAzureActiveDirectoryKerberosForFile $true

# Set default share-level permission
Set-AzStorageAccount -ResourceGroupName <rg> -AccountName <sa> `
  -DefaultSharePermission StorageFileDataSmbShareContributor
```

---

## Confusion Matrix

| Feature | Storage Account Keys | Service/Account SAS | User Delegation SAS |
|---------|---------------------|--------------------|--------------------|
| Signed with | Account key | Account key | Entra credentials |
| Scope | Full account access | Scoped to resource | Blob Storage only |
| Stored access policy | N/A | YES (service SAS) | NO |
| Affected by key rotation | YES | YES (invalidated) | NO |
| Disabled by "no Shared Key" | YES | YES | NO |
| Security level | Lowest | Medium | Highest (of SAS) |

| Feature | Service Endpoints | Private Endpoints |
|---------|------------------|-------------------|
| IP used by client | Private (Azure backbone) | Private IP from VNet |
| On-prem access | Requires public IP NAT | Via VPN/ExpressRoute |
| Data exfiltration protection | No | Yes |
| Cross-tenant | No | Yes |
| Cost | Free | Per-endpoint + data |
| Microsoft preference | -- | **Recommended** |

| Feature | Stored Access Policy | Ad Hoc SAS |
|---------|---------------------|------------|
| Constraints defined | Server-side | In the SAS URI |
| Modifiable after issuance | YES | NO |
| Revocable without key rotation | YES | NO |
| Supported SAS types | Service SAS only | All three types |
| Max per container | 5 | Unlimited |

| Azure Files Identity | On-prem AD DS | Entra Domain Services | Entra Kerberos |
|---------------------|--------------|----------------------|----------------|
| Domain controller | On-prem DC | Azure-managed DC | None needed |
| Identities | Hybrid only | Cloud + hybrid | Hybrid + cloud (preview) |
| Client requirement | Domain-joined | Domain-joined to managed domain | Entra-joined |
| Sync tool needed | Entra Connect | -- | -- |

---

## Decision Rules

| If... | Then... |
|-------|---------|
| Need to restrict storage to specific VNet | Enable **service endpoint** on subnet + add VNet rule + set default Deny |
| Need to allow on-prem access through firewall | Add **public IP** rule (private IPs not allowed) |
| Need most secure delegated access | Use **user delegation SAS** (Entra-backed) |
| Need to revoke a SAS without rotating keys | Use **stored access policy** (service SAS only) |
| Need to grant scoped access to Blob + Queue + Table | Use **account SAS** (multi-service) |
| Shared Key disabled on storage account | Only **user delegation SAS** and **Entra ID** auth work |
| Need to auto-rotate storage keys | Use **Azure Key Vault** managed storage |
| Rotating key1 and apps use service SAS signed by key1 | All those SAS tokens are **invalidated** |
| Need enterprise file share with SSO | Use **Azure Files + identity-based auth** (AD DS or Entra Kerberos) |
| Need both RBAC + granular file permissions | Configure **share-level RBAC** + **NTFS ACLs** (most restrictive wins) |

---

## 5 Common Scenario Patterns

**Scenario 1: Restricting storage to a VNet**

> "Contoso wants their storage account accessible only from the App subnet in their VNet."

- Enable `Microsoft.Storage` service endpoint on the App subnet
- Add VNet rule for that subnet to the storage account
- Set default action to **Deny**
- Enable **trusted services exception** if Azure Backup/Monitor needed

**Scenario 2: Granting temporary blob access to a partner**

> "A partner needs read access to a specific blob for 2 hours without an Azure account."

- Generate a **user delegation SAS** (preferred) or service SAS
- Set `sp=r` (read only), `se` = 2 hours from now, `spr=https`
- Set start time 15 minutes in the past for clock skew
- Share the SAS URI with the partner

**Scenario 3: Revoking leaked SAS tokens**

> "A developer accidentally committed a SAS token to a public Git repo."

- If service SAS with stored access policy → **delete or rename** the stored access policy
- If ad hoc SAS (no policy) → **rotate the signing key** (invalidates ALL SAS signed with that key)
- If user delegation SAS → **revoke the user delegation key**

**Scenario 4: Migrating file server to Azure Files**

> "Contoso wants to move an on-prem file server to Azure Files with existing AD permissions."

- Enable **on-premises AD DS** authentication on the storage account
- Domain-join the storage account (creates computer object in AD)
- Sync identities with **Entra Connect**
- Assign **share-level RBAC** roles to Entra groups
- Mount with storage account key, set **NTFS ACLs** with icacls
- Users access via `\\storageaccount.file.core.windows.net\share`

**Scenario 5: Zero-downtime key rotation**

> "Security policy requires rotating storage keys every 60 days without app downtime."

- All apps currently use **key1**
- Switch all apps to **key2** (secondary)
- Regenerate **key1** via portal/CLI/PowerShell
- Switch all apps back to **new key1**
- Regenerate **key2**
- Set key expiration policy: `--key-exp-days 60`

---

## Exam Traps

> **TRAP 1:** IP rules only accept **public IPs** — private ranges (10.x, 172.16–31.x, 192.168.x) are rejected. Use VNet rules for private network access.

> **TRAP 2:** IP rules have **no effect** on requests from the **same Azure region** as the storage account — the traffic uses private IPs internally. Use VNet rules instead.

> **TRAP 3:** Only **service SAS** supports stored access policies. User delegation SAS and account SAS do **NOT**.

> **TRAP 4:** User delegation key max validity is **7 days** — not 30, not 90. SAS tokens using this key cannot exceed 7 days.

> **TRAP 5:** Rotating/regenerating an access key **invalidates all service SAS and account SAS** signed with that key. User delegation SAS is unaffected.

> **TRAP 6:** Maximum **5 stored access policies** per container/share/queue/table. Exceeding this returns 400 (Bad Request).

> **TRAP 7:** Recreating a deleted stored access policy with the **same name** restores access to all existing SAS tokens. Use a **different name** to truly revoke.

> **TRAP 8:** Disabling Shared Key on a storage account makes **service SAS + account SAS invalid**. Only user delegation SAS and Entra ID work.

> **TRAP 9:** Azure Files identity-based auth allows only **ONE identity source per storage account** — you cannot mix AD DS and Entra Domain Services.

> **TRAP 10:** After setting stored access policy, changes take up to **30 seconds** to propagate — requests during this window may return **403 Forbidden**.

---

## Rapid-Fire Q&A

**Q: What must you set before storage firewall rules take effect?**
A: Default action must be set to **Deny**

**Q: Can you use private IP addresses in storage firewall IP rules?**
A: No — only public internet IPs (no RFC 1918 ranges)

**Q: Which SAS type is most secure?**
A: **User delegation SAS** (signed with Entra credentials, not account key)

**Q: Which SAS type supports stored access policies?**
A: **Service SAS** only (not user delegation or account SAS)

**Q: What is the max validity for a user delegation key?**
A: **7 days**

**Q: How many stored access policies per container?**
A: **5**

**Q: What happens to service SAS when you rotate the signing key?**
A: All service SAS signed with that key are **invalidated**

**Q: How do you revoke a service SAS without rotating keys?**
A: Delete or rename its **stored access policy**

**Q: What are the two access keys for?**
A: **Zero-downtime rotation** — use one while regenerating the other

**Q: What happens when Shared Key access is disabled?**
A: Service SAS and account SAS become **invalid**; only user delegation SAS and Entra auth work

**Q: How many identity sources can Azure Files use per storage account?**
A: **1** (choose AD DS, Entra Domain Services, or Entra Kerberos)

**Q: What are the two layers of Azure Files permissions?**
A: **Share-level RBAC** (gatekeeper) + **NTFS ACLs** (granular); most restrictive wins

**Q: Which Azure Files RBAC role can modify ACLs?**
A: **Storage File Data SMB Share Elevated Contributor**

**Q: What tool do you use for initial NTFS ACL setup on Azure Files?**
A: Mount with **storage account key** via `net use`, then use **icacls** or File Explorer

**Q: What is the recommended SAS start time offset for clock skew?**
A: Set start time **15 minutes in the past**

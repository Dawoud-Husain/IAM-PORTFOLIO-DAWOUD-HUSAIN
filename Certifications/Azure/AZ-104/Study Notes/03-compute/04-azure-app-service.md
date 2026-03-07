# 3.4 — Create and Configure Azure App Service

**Domain Weight: 20–25% (HIGH PRIORITY)**

---

## Core Concepts

### Provision an App Service Plan

An App Service plan defines the compute resources (VM instances) that run your web apps. Every App Service app must belong to a plan, and multiple apps can share one plan. The tier you choose determines available features, scaling limits, and pricing.

**App Service Plan Tiers:**

| Tier | SKU | Use Case | Key Features |
| --- | --- | --- | --- |
| **Free** | F1 | Dev/test | Shared infra, no custom domains, no SSL, 1 GB disk, 60 min/day CPU |
| **Shared** | D1 | Dev/test | Shared infra, custom domains, no SSL, 1 GB disk, 240 min/day CPU |
| **Basic** | B1–B3 | Low-traffic | Dedicated VMs, manual scale only, no slots, no autoscale |
| **Standard** | S1–S3 | Production | Autoscale, 5 deployment slots, VNet integration, backup |
| **Premium v2/v3** | P1v2–P3v3 | High perf | 20 deployment slots, more memory/CPU, automatic scaling |
| **Premium v4** | P0v4–P5v4 | Latest gen | Newest hardware, zone redundancy |
| **Isolated v2** | I1v2–I6v2 | High security | Runs in App Service Environment (ASE), dedicated VNet |

**Key Facts:**

- Apps in same plan **share the same VM instances** — all scale together
- Moving apps between plans requires plans to be in the **same resource group AND region**
- Free/Shared tiers run on shared VMs → no SLA
- Basic+ tiers use dedicated VMs → SLA applies
- Linux and Windows apps **cannot be in the same plan** (except Free/Shared tier)
- Linux apps cannot be in Shared tier

---

### Configure Scaling for an App Service Plan

Scaling controls how many instances run your app and what size those instances are. There are two approaches: scale up (change tier/VM size) and scale out (add more instances).

**Scale Up vs. Scale Out:**

| Action | What Changes | When to Use |
| --- | --- | --- |
| **Scale up** | VM size / tier (more CPU, RAM) | App needs more power per instance |
| **Scale out** | Number of instances | App needs to handle more concurrent requests |

**Scale-Out Instance Limits:**

| Tier | Max Instances |
| --- | --- |
| Free / Shared | 1 (no scale out) |
| Basic | 3 (manual only) |
| Standard | 10 (autoscale) |
| Premium v2/v3 | 30 (autoscale + automatic) |
| Isolated v2 | 100 |

**Autoscale (Standard+):**

- Based on **metrics** (CPU %, memory, HTTP queue length, custom) or **schedule** (time/date)
- Define rules with thresholds, cooldown periods, instance count ranges
- Set **minimum**, **maximum**, and **default** instance count
- Cooldown period prevents rapid scale flapping (default 5 min)
- Multiple rules: scale-out triggers if **ANY** rule met; scale-in triggers only if **ALL** rules met

**Automatic Scaling (Premium v2/v3/v4):**

- Pre-warms instances based on **HTTP traffic** patterns
- No custom rules needed — platform-managed
- Set **maximum burst** limit; platform handles scaling decisions
- Avoids cold start — instances are ready before traffic arrives
- Different from autoscale: no scale rules to configure

**Mnemonic — Scale Limits: "BSP-I = 3-10-30-100"**

- **B**asic = 3
- **S**tandard = 10
- **P**remium = 30
- **I**solated = 100

---

### Create an App Service

An App Service is the actual web app (or API app, mobile backend) that runs inside a plan. You specify runtime stack, OS, region, and the plan during creation.

**Required Settings at Creation:**

- **App name** — globally unique, becomes `<name>.azurewebsites.net`
- **Publish** — Code or Docker Container
- **Runtime stack** — .NET, Java, Node.js, Python, PHP, Ruby (Linux only for some)
- **OS** — Windows or Linux
- **Region** — determines available plans
- **App Service plan** — select existing or create new

**Post-Creation Configuration:**

- **Application settings** — environment variables (key-value pairs), connection strings
- **General settings** — platform (32/64-bit), always on, ARR affinity, HTTPS only
- **Always On** — keeps app loaded even when idle (Basic+ tier, recommended for production)
- **ARR Affinity** — session affinity (sticky sessions); disable for stateless apps
- **HTTPS Only** — redirects all HTTP to HTTPS

---

### Configure Certificates and TLS for an App Service

App Service supports TLS/SSL to secure traffic between clients and your app. You can use free managed certificates or bring your own certificates for custom domains.

**Certificate Sources:**

| Source | Cost | Wildcard | Features |
| --- | --- | --- | --- |
| **App Service Managed Certificate** | Free | NO | Auto-renew, only SNI SSL, no export |
| **App Service Certificate** | Paid | YES | Stored in Key Vault, auto-renew, exportable |
| **Upload PFX / PEM** | Varies | YES | Upload existing cert (.pfx or .pem) |
| **Import from Key Vault** | Varies | YES | Syncs from Azure Key Vault |

**TLS/SSL Binding Types:**

| Binding | How It Works | IP Required |
| --- | --- | --- |
| **SNI SSL** | Uses hostname (SNI header) to route | NO — shared IP |
| **IP SSL** | Dedicated IP address per cert | YES — dedicated IP |

**Key Facts:**

- Free managed cert: SNI SSL only, no wildcard, no naked/root domains, no export
- Minimum tier for custom TLS: **Basic**
- Free/Shared tier: cannot use custom SSL bindings
- TLS minimum version configurable: 1.0, 1.1, 1.2 (default: 1.2)
- **HTTPS Only** setting forces HTTP→HTTPS redirect
- Private certificates require `.pfx` format (or `.pem` for Linux)
- Certificate must include: private key, x509 format, all intermediate certs in chain

---

### Map an Existing Custom DNS Name to an App Service

Custom domain mapping lets you use your own domain (e.g., `www.contoso.com`) instead of `<app>.azurewebsites.net`. Domain verification is done via DNS records.

**DNS Record Types:**

| Record | Maps | Use For |
| --- | --- | --- |
| **CNAME** | Subdomain → `<app>.azurewebsites.net` | `www.contoso.com` |
| **A record** | Root domain → App's IP address | `contoso.com` (apex) |
| **TXT** | Verification → `<app>.azurewebsites.net` | Domain ownership proof |

**Steps to Map Custom Domain:**

1. Get App Service IP (for A record) or default hostname (for CNAME)
2. Create DNS records at your registrar (CNAME or A + TXT)
3. In App Service → Custom domains → Add custom domain
4. Validate domain ownership
5. Add TLS/SSL binding for the custom domain

**Key Facts:**

- Custom domains require **Basic tier or higher**
- Free managed certificate: subdomains only (not root/naked domains)
- Root domain requires **A record** (CNAME cannot point to zone apex)
- ASUID verification TXT record: `asuid.<subdomain>` → App's verification ID
- Multiple custom domains can point to the same app
- Each slot can have its own custom domains

---

### Configure Backup for an App Service

App Service provides automatic and custom (on-demand/scheduled) backup capabilities for app files, configuration, and linked databases.

**Automatic vs. Custom Backups:**

| Feature | Automatic | Custom |
| --- | --- | --- |
| Tier required | Basic+ | Basic+ |
| Configuration | None | Yes (storage account needed) |
| Max size | 30 GB | 10 GB (4 GB for linked DB) |
| Frequency | Hourly (not configurable) | Configurable (min every 2 hours) |
| Retention | 30 days (not configurable) | 0–30 days or indefinite |
| Downloadable | No | Yes (stored as blobs) |
| Partial backups | No | Yes (`_backup.filter` file) |
| Linked database | No | Yes (until 3/31/2028) |
| Storage account | Not needed | Required |

**What IS Backed Up:**

- App content (files under `%HOME%`)
- App configuration, Application Insights, health check settings, native log settings

**What IS NOT Restored:**

- Network features (private endpoints, VNet integration, hybrid connections)
- Custom domains, TLS/SSL bindings
- Authentication, managed identities
- Scale-out settings, deployment slots, backup config itself

**Custom Backup Requirements:**

- Storage account in same subscription
- Blob container for storing backups
- Basic tier: production slot only
- Standard+: can backup deployment slots too
- Partial backups: create `_backup.filter` in `%HOME%\site\wwwroot`
- VNet backup: requires VNet integration + storage allows access from that VNet

---

### Configure Networking Settings for an App Service

App Service offers several networking features to control inbound and outbound traffic. By default, apps are publicly accessible and can only reach internet endpoints.

**Inbound Features (traffic TO your app):**

| Feature | Purpose | Tier |
| --- | --- | --- |
| **Access Restrictions** | Allow/deny by IP, VNet, service tag | All tiers |
| **Private Endpoints** | Private IP from VNet for inbound traffic | Basic+ |
| **Service Endpoints** | Restrict inbound to specific subnets | All tiers |

**Outbound Features (traffic FROM your app):**

| Feature | Purpose | Tier |
| --- | --- | --- |
| **VNet Integration** | Route outbound traffic through a VNet | Basic+ |
| **Hybrid Connections** | Outbound TCP to on-prem endpoints | Standard+ |

**VNet Integration Key Facts:**

- Enables **outbound** access to VNet resources (NOT inbound)
- App and VNet must be in **same region**
- Requires a **dedicated subnet** (delegated to `Microsoft.Web/serverFarms`)
- Subnet sizing: `/26` recommended (64 addresses); minimum `/28`
- Up to **2 VNet integrations** per App Service plan
- Supports: NSGs on integration subnet, route tables (UDRs), NAT gateway
- Does NOT support: drive mounting, AD domain join, NetBIOS
- For inbound private access → use **Private Endpoints** instead

**Hybrid Connections:**

- Outbound TCP tunnel to on-prem host:port via Azure Relay
- Requires **Hybrid Connection Manager** agent on Windows Server 2012+
- Does NOT require VPN or ExpressRoute
- Port 443 outbound to Azure Relay required
- Per-endpoint security (single host:port per connection)

**Private Endpoints:**

- Private IP from VNet for **inbound** traffic only
- Up to **100 private endpoints** per slot
- Cannot share endpoints between slots
- Available on Basic+ tiers
- Access restrictions do NOT apply to private endpoint traffic
- VNet integration subnet cannot be same subnet as private endpoint

---

### Configure Deployment Slots for an App Service

Deployment slots are live apps with their own hostnames, used for staging, testing, and zero-downtime deployments. You swap a staging slot into production to deploy changes.

**Slot Availability:**

| Tier | Max Slots |
| --- | --- |
| Free / Shared / Basic | 0 (production only) |
| Standard | 5 |
| Premium | 20 |
| Isolated | 20 |

**Settings That Swap (follow the app):**

- App settings and connection strings (unless marked "slot setting")
- Handler mappings, public certificates
- WebJobs content, WebJobs schedulers
- Hybrid connections, VNet integration
- Managed identities, endpoints with suffix `<appname>/slots/<slotname>`

**Settings That Do NOT Swap (stay with the slot):**

- Publishing endpoints, custom domain names
- Non-public certificates, TLS/SSL bindings
- Scale settings, diagnostic settings
- CORS, IP restrictions
- Always On, protocol settings
- Settings marked as **"deployment slot setting"**

**Swap Types:**

| Type | Behavior |
| --- | --- |
| **Swap** | Instant swap of source ↔ target |
| **Swap with preview** (multi-phase) | Apply target config to source first → verify → complete swap |
| **Auto swap** | Auto-swap to production after code push to slot (not available on Linux) |

**Swap Process (what happens internally):**

1. Target slot settings applied to source slot (triggers restart)
2. Source slot instances warm up with target config
3. Swap completes → source becomes target and vice versa
4. Previous production app now in source slot (rollback available)

**Key Facts:**

- Slot settings override: mark a setting as **"deployment slot setting"** to keep it sticky
- Auto swap: configure on the **source** slot, set target to production
- Each slot is a separate app with its own hostname: `<appname>-<slotname>.azurewebsites.net`
- Traffic routing: can route a percentage of traffic to a slot for A/B testing
- Slots share the same App Service plan (same VMs) — no extra cost

---

## What The Exam Tests

- App Service plan tier features: which tier supports slots, scaling, VNet integration, backup
- Scale-out instance limits per tier (3 / 10 / 30 / 100)
- Autoscale vs. automatic scaling differences
- Deployment slot swap behavior: what swaps vs. what stays
- Auto swap availability (not on Linux)
- Custom backup requirements: storage account, size limits (10 GB / 4 GB DB)
- What is NOT restored from backup (custom domains, TLS, auth, network features)
- VNet integration direction (outbound only, NOT inbound)
- Private endpoints direction (inbound only)
- Free managed certificate limitations (no wildcard, no root domain, SNI only)
- SNI SSL vs. IP SSL binding types
- DNS record types for custom domains (CNAME for subdomain, A for root)
- Hybrid Connections: on-prem TCP access without VPN
- Slot setting sticky behavior: deployment slot setting checkbox

---

## Important Limits / Numbers

| Item | Value |
| --- | --- |
| Basic tier max instances | **3** (manual only) |
| Standard tier max instances | **10** (autoscale) |
| Premium tier max instances | **30** (autoscale + automatic) |
| Isolated tier max instances | **100** |
| Standard tier deployment slots | **5** |
| Premium / Isolated deployment slots | **20** |
| Custom backup max size | **10 GB** (4 GB for linked DB) |
| Automatic backup max size | **30 GB** |
| Automatic backup retention | **30 days** |
| Min scheduled backup frequency | **Every 2 hours** |
| Max private endpoints per slot | **100** |
| Max VNet integrations per plan | **2** |
| Recommended VNet integration subnet | **/26** (64 addresses) |
| Minimum VNet integration subnet | **/28** |
| Autoscale cooldown default | **5 minutes** |
| Free tier CPU limit | **60 min/day** |
| Free managed cert wildcard support | **NO** |

---

## CLI + PowerShell Commands

### Azure CLI

```bash
# Create App Service plan
az appservice plan create \
  --name myPlan \
  --resource-group myRG \
  --sku S1 \
  --location eastus

# Create web app
az webapp create \
  --name myApp \
  --resource-group myRG \
  --plan myPlan \
  --runtime "DOTNET:8.0"

# Scale up (change tier)
az appservice plan update \
  --name myPlan \
  --resource-group myRG \
  --sku P1v3

# Scale out (set instance count)
az appservice plan update \
  --name myPlan \
  --resource-group myRG \
  --number-of-workers 3

# Configure autoscale
az monitor autoscale create \
  --resource-group myRG \
  --resource myPlan \
  --resource-type Microsoft.Web/serverfarms \
  --min-count 1 --max-count 10 --count 2

# Create deployment slot
az webapp deployment slot create \
  --name myApp \
  --resource-group myRG \
  --slot staging

# Swap slots
az webapp deployment slot swap \
  --name myApp \
  --resource-group myRG \
  --slot staging \
  --target-slot production

# Configure custom domain
az webapp config hostname add \
  --webapp-name myApp \
  --resource-group myRG \
  --hostname www.contoso.com

# Upload TLS certificate
az webapp config ssl upload \
  --name myApp \
  --resource-group myRG \
  --certificate-file mycert.pfx \
  --certificate-password "P@ssw0rd"

# Bind SSL certificate
az webapp config ssl bind \
  --name myApp \
  --resource-group myRG \
  --certificate-thumbprint <thumbprint> \
  --ssl-type SNI

# Create backup
az webapp config backup create \
  --webapp-name myApp \
  --resource-group myRG \
  --container-url <sas-url>

# Configure VNet integration
az webapp vnet-integration add \
  --name myApp \
  --resource-group myRG \
  --vnet myVNet \
  --subnet mySubnet

# List automatic backups (snapshots)
az webapp config snapshot list \
  --name myApp \
  --resource-group myRG
```

### PowerShell

```powershell
# Create App Service plan
New-AzAppServicePlan -Name "myPlan" `
  -ResourceGroupName "myRG" `
  -Location "EastUS" `
  -Tier "Standard" `
  -NumberOfWorkers 2

# Create web app
New-AzWebApp -Name "myApp" `
  -ResourceGroupName "myRG" `
  -AppServicePlan "myPlan" `
  -Location "EastUS"

# Scale up
Set-AzAppServicePlan -Name "myPlan" `
  -ResourceGroupName "myRG" `
  -Tier "PremiumV3"

# Create deployment slot
New-AzWebAppSlot -Name "myApp" `
  -ResourceGroupName "myRG" `
  -Slot "staging"

# Swap slots
Switch-AzWebAppSlot -Name "myApp" `
  -ResourceGroupName "myRG" `
  -SourceSlotName "staging" `
  -DestinationSlotName "production"

# Set custom domain
Set-AzWebApp -Name "myApp" `
  -ResourceGroupName "myRG" `
  -HostNames @("www.contoso.com","myApp.azurewebsites.net")
```

---

## Confusion Matrix

| Feature | Scale Up | Scale Out |
| --- | --- | --- |
| What changes | VM size / tier | Number of instances |
| Direction | Vertical | Horizontal |
| Requires autoscale | No | Standard+ for auto |
| Downtime | Brief restart | No downtime |

| Feature | Autoscale | Automatic Scaling |
| --- | --- | --- |
| Tier | Standard+ | Premium v2/v3/v4 |
| Configuration | Custom rules (metric/schedule) | Set max burst only |
| Trigger | CPU %, memory, queue, custom | HTTP traffic patterns |
| Pre-warming | No | Yes |
| Cold start risk | Yes | No |

| Feature | VNet Integration | Private Endpoints |
| --- | --- | --- |
| Direction | **Outbound** (app → VNet) | **Inbound** (client → app) |
| Purpose | Access VNet/on-prem resources | Expose app on private IP |
| Tier required | Basic+ | Basic+ |
| Same subnet allowed | No (cannot overlap) | No (cannot overlap) |

| Feature | VNet Integration | Hybrid Connections |
| --- | --- | --- |
| Direction | Outbound | Outbound |
| Connects to | Azure VNet resources | On-prem TCP host:port |
| Requires VPN | No (but needs VNet) | No |
| Agent needed | No | Yes (Hybrid Connection Manager) |
| Tier | Basic+ | Standard+ |

| Feature | SNI SSL | IP SSL |
| --- | --- | --- |
| IP address | Shared | Dedicated |
| Multiple certs on one IP | Yes | No |
| Browser support | Modern browsers | All browsers |
| Cost | Lower | Higher (dedicated IP) |

| Backup Feature | Automatic | Custom |
| --- | --- | --- |
| Storage account | Not needed | Required |
| Max size | 30 GB | 10 GB |
| Downloadable | No | Yes |
| Linked database | No | Yes (until 2028) |
| Partial backup | No | Yes |

---

## Decision Rules

| If... | Then... |
| --- | --- |
| Need deployment slots | Use **Standard** tier or higher |
| Need autoscale | Use **Standard** tier or higher |
| Need automatic scaling (pre-warming) | Use **Premium v2/v3/v4** |
| Need VNet integration | Use **Basic** tier or higher |
| Need to access on-prem without VPN | Use **Hybrid Connections** (Standard+) |
| Need to restrict inbound to private network | Use **Private Endpoints** |
| Need free SSL for custom subdomain | Use **App Service Managed Certificate** (SNI only) |
| Need wildcard or root domain SSL | Use **App Service Certificate** or upload your own |
| Need to map root/apex domain | Use **A record** (CNAME cannot point to zone apex) |
| Need to map subdomain | Use **CNAME record** |
| Need zero-downtime deployment | Use **deployment slot swap** |
| Need to test with partial traffic | Use **slot traffic routing** (percentage-based) |
| Need custom backup with database | Use **Custom backup** (requires storage account, 10 GB limit) |
| Need to backup to firewall-protected storage | Enable **VNet integration** + backup over VNet |
| Autoscale: scale-out rule triggered | Scale out if **ANY** rule is met |
| Autoscale: scale-in rule triggered | Scale in only if **ALL** rules are met |

---

## 5 Common Scenario Patterns

**Scenario 1: Zero-downtime production deployment**

> "Contoso wants to deploy a new version of their web app without any downtime for users."

- Create a **staging deployment slot** (Standard+ tier)
- Deploy new code to the staging slot
- Test and validate in staging
- **Swap** staging with production → instant, zero-downtime cutover
- If issues arise, swap back (previous production is now in staging)

**Scenario 2: Scaling for variable traffic**

> "An e-commerce app experiences high traffic during sales events but minimal traffic otherwise."

- Use **Standard or Premium** tier with autoscale
- Create scale-out rule: **CPU > 70% → add 2 instances** (max 10)
- Create scale-in rule: **CPU < 30% → remove 1 instance** (min 2)
- Set cooldown period to **5 minutes** to prevent flapping
- Or use **Premium v2/v3** with automatic scaling for HTTP-based pre-warming

**Scenario 3: Securing an internal web app**

> "A line-of-business app should only be accessible from the corporate VNet, not the public internet."

- Create **Private Endpoint** for the app (inbound private IP in VNet)
- Disable public network access via access restrictions
- For outbound calls to on-prem database: enable **VNet integration**
- For on-prem TCP access without VPN: use **Hybrid Connections**

**Scenario 4: Custom domain with SSL**

> "Contoso wants to use www.contoso.com with HTTPS for their App Service."

- Ensure app is on **Basic+ tier**
- At registrar: create **CNAME** record: `www` → `myapp.azurewebsites.net`
- Create **TXT** record for domain verification: `asuid.www` → app verification ID
- Add custom domain in App Service → Custom domains
- Use **free managed certificate** for SNI SSL (auto-renewed, subdomain only)

**Scenario 5: Backup and disaster recovery**

> "An admin needs to set up scheduled backups for a production app with a linked SQL database."

- Ensure app is on **Standard+ tier** (Basic supports production slot only)
- Create a **storage account** with a blob container
- Configure custom scheduled backup (minimum **every 2 hours**)
- Include linked Azure SQL database (connection string must exist in app config)
- Total backup must be **under 10 GB** (database portion under 4 GB)
- For VNet-protected storage: enable VNet integration + backup over VNet option

---

## Exam Traps

> **TRAP 1:** Free managed certificates do **NOT** support wildcard domains or root/naked domains — only subdomains like `www.contoso.com`. For root domains, you need a paid certificate.

> **TRAP 2:** Deployment slots are **NOT** available on Free, Shared, or Basic tiers. You need **Standard** (5 slots) or **Premium** (20 slots).

> **TRAP 3:** VNet integration is for **outbound** traffic only. For **inbound** private access, you need **Private Endpoints**. These are different features.

> **TRAP 4:** Autoscale scale-out triggers if **ANY** rule is met, but scale-in triggers only if **ALL** rules are met. This asymmetry is a common exam question.

> **TRAP 5:** Settings marked as **"deployment slot setting"** do NOT swap — they stay with the slot. By default, most settings DO swap unless explicitly marked.

> **TRAP 6:** Auto swap is **NOT available on Linux** App Service plans. It only works on Windows.

> **TRAP 7:** Custom backup max size is **10 GB** (with 4 GB DB limit). Automatic backup max is **30 GB** but is NOT downloadable and NOT configurable.

> **TRAP 8:** A **CNAME record cannot be used for root/apex domains** (e.g., `contoso.com`). You must use an **A record** for the root domain.

> **TRAP 9:** The VNet integration subnet and private endpoint subnet **cannot be the same subnet**. They must use different subnets.

> **TRAP 10:** Hybrid Connections do **NOT** require a VPN or ExpressRoute — they tunnel over port 443 via Azure Relay. But they do require the **Hybrid Connection Manager** agent installed on a Windows Server.

---

## Rapid-Fire Q&A

**Q: What tier is the minimum for deployment slots?**
A: Standard (5 slots)

**Q: What are the scale-out limits for Basic, Standard, Premium, Isolated?**
A: 3, 10, 30, 100

**Q: What is the difference between autoscale and automatic scaling?**
A: Autoscale (Standard+) uses custom metric/schedule rules. Automatic scaling (Premium v2/v3/v4) pre-warms instances based on HTTP traffic — no rules needed.

**Q: What DNS record type do you use for a root/apex domain?**
A: A record (CNAME cannot point to zone apex)

**Q: Can the free managed certificate be used for wildcard domains?**
A: No — subdomain only, SNI SSL only, no root domains, no export

**Q: What is VNet integration used for — inbound or outbound?**
A: Outbound only (app → VNet). Use Private Endpoints for inbound.

**Q: What does a deployment slot swap do with settings marked "slot setting"?**
A: They do NOT swap — they stay with the slot (sticky)

**Q: What storage is required for custom backups?**
A: Azure Storage account with a blob container in the same subscription

**Q: What is the custom backup size limit?**
A: 10 GB total (4 GB for linked database)

**Q: Is auto swap available on Linux?**
A: No — Windows only

**Q: What agent is needed for Hybrid Connections?**
A: Hybrid Connection Manager on Windows Server 2012+

**Q: What binding type does the free managed certificate use?**
A: SNI SSL only (not IP SSL)

**Q: How does autoscale handle multiple scale-out rules?**
A: Scale out if ANY rule is met; scale in only if ALL rules are met

**Q: What tier is minimum for VNet integration?**
A: Basic

**Q: What settings are NOT restored from a backup?**
A: Custom domains, TLS/SSL, authentication, managed identities, network features, scale settings, deployment slots

**Q: What subnet size is recommended for VNet integration?**
A: /26 (64 addresses); minimum /28

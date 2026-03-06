# 3.1 — Automate Deployment of Resources by Using ARM Templates or Bicep Files

**Domain Weight: 20–25% (HIGH PRIORITY)**

---

## Core Concepts

### Interpret an Azure Resource Manager Template or a Bicep File

ARM templates are JSON files that declaratively define Azure infrastructure. Bicep is a domain-specific language (DSL) that transpiles to ARM JSON with cleaner, more readable syntax. Both are processed by Azure Resource Manager, which orchestrates deployments idempotently — if a resource already exists with the same config, it is skipped.

**ARM Template JSON Structure (Top-Level Sections):**

| Section | Required | Description | Limits |
|---|---|---|---|
| `$schema` | YES | Schema URL — determines deployment scope | — |
| `contentVersion` | YES | Template version (e.g., `"1.0.0.0"`) — for documentation only | — |
| `parameters` | No | Input values at deployment time | **256 max** |
| `variables` | No | Reusable values/expressions | **256 max** |
| `functions` | No | User-defined functions (require namespace) | — |
| `resources` | YES | Resources to deploy or update | **800 max** |
| `outputs` | No | Values returned after deployment | **64 max** |

**Schema URLs by Deployment Scope:**

| Scope | Schema URL (end portion) |
|---|---|
| Resource Group | `.../2019-04-01/deploymentTemplate.json#` |
| Subscription | `.../2018-05-01/subscriptionDeploymentTemplate.json#` |
| Management Group | `.../2019-08-01/managementGroupDeploymentTemplate.json#` |
| Tenant | `.../2019-08-01/tenantDeploymentTemplate.json#` |

**Mnemonic — ARM Required Sections: "SCR" (Schema, ContentVersion, Resources)**

**Parameter Properties:**

- `type` (required): `string`, `securestring`, `int`, `bool`, `object`, `secureObject`, `array`
- `defaultValue`, `allowedValues`, `minValue`, `maxValue`, `minLength`, `maxLength`, `metadata.description`
- `securestring` / `secureObject` values NOT saved in deployment history
- CANNOT use `reference()` or `list*()` in parameters (require runtime state)

**Key ARM Template Functions:**

| Function | Purpose |
|---|---|
| `resourceGroup()` | Returns current RG object (id, name, location) — RG scope only |
| `subscription()` | Returns subscription object — RG/sub scope only |
| `resourceId(type, name)` | Returns unique resource ID string |
| `reference(name)` | Returns runtime state; creates **implicit dependency** |
| `listKeys(name, apiVersion)` | Lists keys; creates **implicit dependency** |
| `concat(a, b)` | Combines strings or arrays |
| `uniqueString(base)` | Deterministic 13-char hash |
| `parameters('name')` | Gets parameter value |
| `variables('name')` | Gets variable value |

- Expressions wrapped in `"[expression]"` brackets
- Functions are **case-insensitive**
- Escape literal `[` with `[[`

**Bicep File Structure:**

| Element | Keyword | Example |
|---|---|---|
| Target scope | `targetScope` | `targetScope = 'subscription'` |
| Parameter | `param` | `param location string = resourceGroup().location` |
| Variable | `var` | `var name = 'stg${uniqueString(resourceGroup().id)}'` |
| Resource | `resource` | `resource stg 'Microsoft.Storage/storageAccounts@2025-01-01' = { ... }` |
| Module | `module` | `module web './webApp.bicep' = { name: 'webDeploy' params: { ... } }` |
| Output | `output` | `output endpoint string = stg.properties.primaryEndpoints.blob` |
| Existing ref | `existing` | `resource stg '...' existing = { name: 'existingName' }` |

- Default target scope = `resourceGroup` (no need to set)
- String interpolation: `'${prefix}${suffix}'` replaces `concat()`
- Comments: `//` single-line, `/* */` multi-line
- Dependencies are **automatic** (implicit) — explicit `dependsOn` rarely needed

**ARM JSON vs Bicep Quick Comparison:**

| Feature | ARM JSON | Bicep |
|---|---|---|
| Format | JSON | Custom DSL |
| String concat | `"[concat('a','b')]"` | `'${a}${b}'` |
| Conditions | `"condition": "[bool]"` | `if (condition)` |
| Loops | `"copy": { ... }` | `[for item in list: { ... }]` |
| Dependencies | Explicit `dependsOn` or `reference()` | Automatic (implicit) |
| Modules | Linked/nested templates | `module` keyword |
| Secure params | `"type": "securestring"` | `@secure() param x string` |
| Allowed values | `"allowedValues": [...]` | `@allowed([...])` or union types |

---

### Modify an Existing Azure Resource Manager Template

Modifying ARM templates involves adding/removing resources, changing parameters and variables, updating dependencies, and using conditions and loops. Understanding the copy element for creating multiple instances and the difference between nested and linked templates is critical.

**Adding/Removing Resources:**

- Add a resource object to the `resources` array with `type`, `apiVersion`, `name`
- When removing, also remove any `dependsOn` references to that resource

**Dependencies (dependsOn):**

- `dependsOn` = JSON array of resource names or IDs
- `reference()` and `list*()` create **implicit dependencies** — do NOT add redundant `dependsOn`
- Child resources do NOT automatically depend on parent — must set explicitly
- Unnecessary dependencies **slow deployment** by preventing parallelism
- Circular dependencies → validation error

**Conditional Deployments:**

- `"condition": "[equals(parameters('env'), 'prod')]"` — must evaluate to true/false
- Condition does **NOT cascade** to child resources — apply to each resource separately
- Complete mode + condition=false + API 2019-05-10+ → resource IS **deleted**

**Copy Loops (Multiple Instances):**

```json
"copy": {
  "name": "storageCopy",
  "count": 3,
  "mode": "parallel"
}
```

- Default mode = **parallel** (use `serial` + `batchSize` for sequential)
- `copyIndex()` returns current index (0-based)
- Max count = **800**
- Works on: resources, properties, variables, outputs

**Nested vs Linked Templates:**

| Feature | Nested | Linked |
|---|---|---|
| Location | Embedded in main template | Separate file at HTTP/HTTPS URL |
| Scope | `outer` (default) or `inner` | Always `inner` |
| Reusability | Low (copy-paste) | High (URL-referenced) |
| Deploy mode | Incremental only | Incremental only |

- `outer` scope = parent's parameters/variables accessible; **secure values exposed** in history
- `inner` scope = isolated; secure values protected
- Cannot use BOTH `parameters` and `parametersLink` in linked templates

---

### Modify an Existing Bicep File

Bicep provides cleaner syntax with decorators, conditional deployments using `if`, loops using `for`, the `existing` keyword for referencing deployed resources, and modules for composability. Dependencies are handled automatically in most cases.

**Bicep Decorators:**

| Decorator | Applies To | Purpose |
|---|---|---|
| `@description('...')` | param, var, resource, output | Documentation |
| `@secure()` | param | Mark as sensitive (not logged) |
| `@allowed([...])` | param only | Restrict to specific values |
| `@minLength(n)` / `@maxLength(n)` | param (string/array) | Length constraints |
| `@minValue(n)` / `@maxValue(n)` | param (int) | Value constraints |
| `@batchSize(n)` | resource, module | Serial deployment batch size |

**Conditional Deployment:**

```bicep
resource sa 'Microsoft.Storage/storageAccounts@2025-01-01' = if (deployStorage) {
  name: 'mystorage'
  ...
}
```

- `if` MUST be on the **same line** as `=` — next line = compile error

**Loops:**

```bicep
resource stg 'Microsoft.Storage/storageAccounts@2025-01-01' = [for name in storageNames: {
  name: '${name}${uniqueString(resourceGroup().id)}'
  location: resourceGroup().location
  ...
}]
```

- Variants: integer range `range(0, count)`, array items, array + index `(item, i)`, filtered `if (condition)`
- Max **800** iterations
- Cannot loop with **nested child resources** — use top-level with `parent:` property
- `@batchSize(1)` for fully sequential deployment

**`existing` Keyword:**

```bicep
resource stg 'Microsoft.Storage/storageAccounts@2025-01-01' existing = {
  name: 'existingStorageName'
}
output endpoint string = stg.properties.primaryEndpoints.blob
```

- References an already-deployed resource (NOT redeployed)
- Only `name` (+ optional `scope`) needed
- If resource doesn't exist → deployment **fails** with NotFound

**Modules:**

- `module sym './storage.bicep' = { name: 'storageDeploy' params: { ... } }`
- Can reference: local files, Bicep registries (`br:`), template specs (`ts:`)
- Deployed in **parallel** by default
- Access outputs: `<module>.outputs.<property>`
- Set `scope:` to deploy to different RG/subscription

---

### Deploy Resources by Using an ARM Template or a Bicep File

Deployment is the process of submitting a template to Azure Resource Manager. You choose a deployment method (portal, CLI, PowerShell), a scope (resource group, subscription, management group, tenant), and a mode (Incremental or Complete). The what-if operation previews changes without applying them.

**Deployment Commands by Scope:**

| Scope | Azure CLI | PowerShell |
|---|---|---|
| **Resource Group** | `az deployment group create -g <rg>` | `New-AzResourceGroupDeployment -ResourceGroupName <rg>` |
| **Subscription** | `az deployment sub create -l <loc>` | `New-AzSubscriptionDeployment -Location <loc>` |
| **Management Group** | `az deployment mg create -l <loc>` | `New-AzManagementGroupDeployment -Location <loc>` |
| **Tenant** | `az deployment tenant create -l <loc>` | `New-AzTenantDeployment -Location <loc>` |

- Sub/MG/Tenant require `--location` (metadata storage location)
- Template file: `--template-file` / `-TemplateFile`; URI: `--template-uri` / `-TemplateUri`
- Parameters: `--parameters @params.json` / `-TemplateParameterFile params.json`

**Deployment Modes — Incremental vs Complete:**

| Feature | Incremental (Default) | Complete |
|---|---|---|
| Resources in template | Created/updated | Created/updated |
| Resources NOT in template | **Left unchanged** | **DELETED** |
| Supported scopes | All | **Resource group only** |
| Portal support | Yes | **No** |
| Linked/nested templates | Yes | Root-level only (nested = Incremental) |

- **Incremental does NOT incrementally add properties** — ALL properties are reapplied; omitted properties reset to defaults
- Always run **what-if** before Complete mode

**What-If Operation:**

- Previews changes without deploying
- CLI: `az deployment group what-if`
- PowerShell: `New-AzResourceGroupDeployment -WhatIf`
- Confirm + deploy: `--confirm-with-what-if` (CLI) / `-Confirm` (PS)
- Cannot resolve `reference()` — always shows those properties as changed (noise)

**7 Change Types:** Create, Delete, Ignore, NoChange, NoEffect, Modify, Deploy

**Rollback on Error:**

- Auto-redeploy last successful or specific named deployment on failure
- CLI: `--rollback-on-error` / PowerShell: `-RollbackToLastDeployment`
- Rollback always uses **COMPLETE MODE** (even if original was Incremental)
- Only works for **resource group** scope deployments
- Parameters **cannot be changed** during rollback

**Template Specs:**

- Store ARM templates as Azure resources, secured by RBAC
- Create: `az ts create` / `New-AzTemplateSpec`
- Deploy: `--template-spec $id` / `-TemplateSpecId $id`

---

### Export a Deployment as an ARM Template or Convert to Bicep

You can export existing Azure resources as ARM templates for reuse, and convert between ARM JSON and Bicep formats. There are two export sources: from the current resource state or from deployment history, each with different characteristics.

**Two Export Sources:**

| Feature | From Resource/Resource Group | From Deployment History |
|---|---|---|
| Content | Current state snapshot (verbose) | Original template (sparse, clean) |
| Manual changes | Included | Not included |
| Parameters | Hard-coded; few params | Includes params for reuse |
| Readiness | Needs cleanup | Ready to redeploy |
| Format | ARM JSON or Bicep (portal) | ARM JSON only |

**Export Commands:**

```bash
# CLI — Export from resource group
az group export --name myRG > template.json

# CLI — Export from deployment history
az deployment group export --resource-group myRG --name myDeployment > template.json

# PowerShell — Export from resource group
Export-AzResourceGroup -ResourceGroupName myRG

# PowerShell — Export from deployment history
Save-AzResourceGroupDeploymentTemplate -ResourceGroupName myRG -DeploymentName myDeployment
```

**Export Limitations:**

- Export is **not guaranteed to succeed** — needs cleanup for production
- **200 resource limit** per resource group export
- **Password parameters may be missing**
- Azure Data Factory **cannot be exported**

**Convert ARM JSON ↔ Bicep:**

| Direction | CLI Command | Bicep CLI |
|---|---|---|
| Bicep → ARM JSON | `az bicep build --file main.bicep` | `bicep build main.bicep` |
| ARM JSON → Bicep | `az bicep decompile --file main.json` | `bicep decompile main.json` |

- Decompile is **best-effort** — may need manual fixes
- `--force` to overwrite existing file

**Mnemonic — "BD": Build = Bicep→JSON (Down), Decompile = JSON→Bicep (Up)**

**Key Bicep CLI Commands:**

| Command | Purpose |
|---|---|
| `az bicep build` | Bicep → ARM JSON |
| `az bicep decompile` | ARM JSON → Bicep |
| `az bicep install` | Install Bicep CLI |
| `az bicep upgrade` | Upgrade to latest |
| `az bicep version` | Show version |
| `az bicep lint` | Check linter rules |
| `az bicep format` | Format Bicep file |
| `az bicep generate-params` | Generate params file |
| `az bicep publish` | Publish module to registry |

---

## What The Exam Tests

- ARM template required sections (`$schema`, `contentVersion`, `resources`)
- Schema URL determines deployment scope (RG vs subscription vs MG vs tenant)
- Parameter types including `securestring` (not saved in history)
- `reference()` and `list*()` create implicit dependencies — no redundant `dependsOn`
- Incremental vs Complete mode differences (Complete deletes resources not in template)
- Complete mode only at resource group scope, not via portal
- What-if previews changes; cannot resolve `reference()` (shows noise)
- Rollback uses Complete mode even if original was Incremental
- Export from resource = current state; export from history = original template
- `az bicep build` = Bicep→JSON; `az bicep decompile` = JSON→Bicep
- Bicep `if` must be on same line as `=`
- Bicep `existing` keyword fails if resource doesn't exist
- Copy loop max = 800; parameters max = 256; resources max = 800; outputs max = 64

---

## Important Limits / Numbers

| Item | Value |
|---|---|
| Max parameters per template | **256** |
| Max variables per template | **256** |
| Max resources per template | **800** |
| Max outputs per template | **64** |
| Template file size limit | **4 MB** |
| Parameter file size limit | **4 MB** |
| Copy loop max count | **800** |
| Deployment history per resource group | **800** (auto-deletes to 600) |
| Deployment name max length | **64 characters** |
| Max resources in exported template | **200** |
| What-if nested template limit | **500** |
| What-if cross-RG limit | **800 resource groups** |
| What-if nested template timeout | **5 minutes** |

---

## CLI + PowerShell Commands

### Deploy Templates

```bash
# CLI — Deploy ARM template to resource group
az deployment group create \
  --resource-group myRG \
  --template-file main.json \
  --parameters @params.json

# CLI — Deploy Bicep file
az deployment group create \
  --resource-group myRG \
  --template-file main.bicep \
  --parameters storageName='mystg123'

# CLI — Deploy with Complete mode
az deployment group create \
  --resource-group myRG \
  --template-file main.json \
  --mode Complete

# CLI — What-if preview
az deployment group what-if \
  --resource-group myRG \
  --template-file main.bicep

# CLI — Deploy with what-if confirmation
az deployment group create \
  --resource-group myRG \
  --template-file main.bicep \
  --confirm-with-what-if

# CLI — Rollback to last successful
az deployment group create \
  --resource-group myRG \
  --template-file main.json \
  --rollback-on-error

# CLI — Deploy to subscription scope
az deployment sub create \
  --location eastus \
  --template-file sub.bicep
```

### PowerShell — Deploy Templates

```powershell
# Deploy ARM template
New-AzResourceGroupDeployment -ResourceGroupName "myRG" `
  -TemplateFile "main.json" `
  -TemplateParameterFile "params.json"

# Deploy Bicep file
New-AzResourceGroupDeployment -ResourceGroupName "myRG" `
  -TemplateFile "main.bicep"

# Deploy with Complete mode
New-AzResourceGroupDeployment -ResourceGroupName "myRG" `
  -TemplateFile "main.json" -Mode Complete

# What-if
New-AzResourceGroupDeployment -ResourceGroupName "myRG" `
  -TemplateFile "main.bicep" -WhatIf

# Rollback
New-AzResourceGroupDeployment -ResourceGroupName "myRG" `
  -TemplateFile "main.json" -RollbackToLastDeployment
```

### Export and Convert

```bash
# Export from resource group
az group export --name myRG > template.json

# Export from deployment history
az deployment group export --resource-group myRG --name myDeployment > template.json

# Bicep → ARM JSON
az bicep build --file main.bicep

# ARM JSON → Bicep
az bicep decompile --file main.json

# Install / Upgrade Bicep
az bicep install
az bicep upgrade
az bicep version
```

```powershell
# Export from resource group
Export-AzResourceGroup -ResourceGroupName "myRG"

# Export from deployment history
Save-AzResourceGroupDeploymentTemplate -ResourceGroupName "myRG" `
  -DeploymentName "myDeployment"
```

---

## Confusion Matrix

| Feature | ARM JSON | Bicep |
|---|---|---|
| File extension | `.json` | `.bicep` |
| Syntax | Verbose JSON | Clean DSL |
| String building | `concat()` function | `'${interpolation}'` |
| Conditions | `"condition": "[bool]"` | `if (bool)` on same line as `=` |
| Loops | `"copy": { ... }` | `[for item in list: { ... }]` |
| Dependencies | Explicit `dependsOn` or implicit via `reference()` | Automatic (implicit) |
| Modules | Linked/nested templates (URL) | `module` keyword (file/registry) |
| Secure params | `"type": "securestring"` | `@secure() param x string` |
| Constraints | `allowedValues`, `minLength`, etc. in param object | Decorators: `@allowed`, `@minLength`, etc. |
| Existing resources | `"existing": true` (v2.0) | `existing` keyword |
| Deployment | Direct | Transpiled to ARM JSON first |

| Feature | Incremental Mode | Complete Mode |
|---|---|---|
| Default? | YES | No |
| Resources not in template | Left unchanged | **DELETED** |
| Supported scopes | All (RG, Sub, MG, Tenant) | **Resource group only** |
| Portal support | Yes | **No** |
| Nested/linked templates | Both modes | Always Incremental |
| Recommended | Yes | No (use Deployment Stacks) |

| Feature | Export from Resource | Export from History |
|---|---|---|
| Content | Current state (verbose) | Original template (clean) |
| Manual changes | Included | Not included |
| Parameters | Hard-coded | Parameterized |
| Ready to reuse | Needs cleanup | Yes |
| Format | ARM JSON or Bicep | ARM JSON only |

| Direction | Command | Mnemonic |
|---|---|---|
| Bicep → JSON | `az bicep build` | **B**uild goes **down** to JSON |
| JSON → Bicep | `az bicep decompile` | **D**ecompile goes **up** to Bicep |

---

## Decision Rules

| If... | Then... |
|---|---|
| Need cleaner, more readable templates | Use **Bicep** (transpiles to ARM JSON) |
| Need to deploy same infra repeatedly across environments | Use **ARM templates or Bicep** with parameter files |
| Need to preview deployment changes safely | Use **what-if** operation before deploying |
| Need to delete resources not in template | Use **Complete mode** (resource group only, not via portal) |
| Want to add resources without affecting existing | Use **Incremental mode** (default) |
| Need to reference an existing resource in Bicep | Use **`existing`** keyword |
| Need to create multiple similar resources | Use **copy loops** (ARM) or **for loops** (Bicep) |
| Need to share templates securely across teams | Use **Template Specs** (Azure resources with RBAC) |
| Need to convert existing resources to templates | **Export** from resource group or deployment history |
| Need to convert ARM JSON to Bicep | Use `az bicep decompile` (best-effort, may need fixes) |
| Rollback failed deployment | Use `--rollback-on-error` (WARNING: uses Complete mode) |

---

## 5 Common Scenario Patterns

**Scenario 1: Deploying infrastructure to multiple environments**

> "Contoso needs to deploy the same resources to Dev, Staging, and Production with different sizes and SKUs."

- Create one ARM/Bicep template with **parameters** for environment-specific values (SKU, size, name prefix)
- Create separate **parameter files** for each environment (dev.params.json, prod.params.json)
- Deploy: `az deployment group create --template-file main.bicep --parameters @dev.params.json`

**Scenario 2: Previewing changes before deployment**

> "An admin wants to verify what will change before deploying an updated template to production."

- Run **what-if**: `az deployment group what-if --resource-group prodRG --template-file main.bicep`
- Review the 7 change types (Create, Delete, Modify, NoChange, etc.)
- Note: properties using `reference()` always show as changed (noise — expected)
- Deploy with confirmation: `--confirm-with-what-if`

**Scenario 3: Ensuring no orphaned resources remain**

> "A team wants to guarantee that only resources defined in the template exist in the resource group."

- Deploy in **Complete mode**: `az deployment group create --mode Complete`
- Resources in RG but NOT in template will be **deleted**
- Always preview with what-if first
- Complete mode only works at resource group scope, not via portal

**Scenario 4: Exporting existing infrastructure to a template**

> "An admin manually created resources and now wants to capture them as a reusable template."

- Export from resource group: `az group export --name myRG > template.json`
- The export is verbose and needs cleanup (passwords may be missing, hard-coded values)
- Optionally decompile to Bicep: `az bicep decompile --file template.json`
- Better alternative if deployment exists: export from **deployment history** (cleaner, parameterized)

**Scenario 5: Creating multiple storage accounts with a loop**

> "Deploy 3 storage accounts with different names using a single template."

- ARM JSON: Use `"copy"` element with `"count": 3` and `copyIndex()` for naming
- Bicep:
```bicep
param names array = ['contoso', 'fabrikam', 'coho']
resource stg 'Microsoft.Storage/storageAccounts@2025-01-01' = [for name in names: {
  name: '${name}${uniqueString(resourceGroup().id)}'
  location: resourceGroup().location
  sku: { name: 'Standard_LRS' }
  kind: 'StorageV2'
}]
```
- Max 800 iterations per loop

---

## Exam Traps

> **TRAP 1:** **Incremental mode does NOT incrementally add properties** — ALL properties are reapplied. Omitting a property resets it to its default value. The resource definition must contain the FULL desired state.

> **TRAP 2:** **Complete mode DELETES resources** not in the template. It only works at **resource group** scope and is **NOT available** via the Azure portal. Nested/linked templates always deploy in Incremental mode even within a Complete deployment.

> **TRAP 3:** **Rollback always uses COMPLETE MODE** — even if the original deployment used Incremental. This can cause unexpected resource deletions. Only works for resource group scope.

> **TRAP 4:** `reference()` and `list*()` **CANNOT be used in the parameters section** — they require runtime state. They CAN be used in variables, resources, and outputs.

> **TRAP 5:** Using `reference()` or `list*()` creates an **implicit dependency** — adding explicit `dependsOn` is redundant and a best practice violation.

> **TRAP 6:** `$schema` URL **determines the deployment scope** — using the wrong schema for subscription/MG/tenant deployment causes errors.

> **TRAP 7:** **What-if cannot resolve `reference()`** — properties using it always show as "changed" (false positive/noise). This is expected behavior.

> **TRAP 8:** In Bicep, `if` for conditional deployment MUST be on the **same line as `=`** — placing it on the next line causes a compile error.

> **TRAP 9:** Bicep `existing` keyword **fails with NotFound** if the resource doesn't exist — it's a hard reference, not a conditional check.

> **TRAP 10:** **`az bicep build`** = Bicep→JSON (build down). **`az bicep decompile`** = JSON→Bicep (decompile up). Decompile is **best-effort** and may need manual fixes.

> **TRAP 11:** Export from resource group has a **200 resource limit**, **password parameters may be missing**, and Data Factory **cannot be exported**.

> **TRAP 12:** **Deployment history limit = 800** per resource group — Azure auto-deletes oldest entries down to 600 when approaching the limit. Deleting history does NOT affect deployed resources.

> **TRAP 13:** Conditional deployment (`condition: false`) does **NOT cascade** to child resources — you must add the condition to each resource individually. In Complete mode with API 2019-05-10+, resources with condition=false ARE deleted.

---

## Rapid-Fire Q&A

**Q: What are the 3 required sections in an ARM template?**
A: `$schema`, `contentVersion`, `resources`

**Q: What determines the deployment scope of an ARM template?**
A: The `$schema` URL

**Q: What is the max number of parameters in a template?**
A: 256

**Q: What is the max number of resources in a template?**
A: 800

**Q: Can `reference()` be used in the parameters section?**
A: No — it requires runtime state

**Q: What is the default deployment mode?**
A: Incremental

**Q: What does Complete mode do with resources not in the template?**
A: Deletes them

**Q: At which scopes does Complete mode work?**
A: Resource group only (not subscription, MG, or tenant)

**Q: Can you use Complete mode via the Azure portal?**
A: No

**Q: What mode does rollback use?**
A: Always Complete mode (even if original was Incremental)

**Q: What does `az bicep build` do?**
A: Converts Bicep to ARM JSON

**Q: What does `az bicep decompile` do?**
A: Converts ARM JSON to Bicep (best-effort)

**Q: Is decompile from JSON to Bicep guaranteed to work perfectly?**
A: No — it's best-effort and may need manual fixes

**Q: What does the what-if operation do?**
A: Previews changes without deploying; validates template

**Q: Why does what-if show properties as changed even when they haven't?**
A: It cannot resolve `reference()` — expected noise

**Q: In Bicep, where must `if` be placed for conditional deployment?**
A: On the same line as `=` (not on the next line)

**Q: What happens if Bicep `existing` references a nonexistent resource?**
A: Deployment fails with NotFound

**Q: What is the deployment history limit per resource group?**
A: 800 (auto-deletes to 600 when approaching limit)

**Q: What is the max copy loop count?**
A: 800

**Q: What creates an implicit dependency in ARM templates?**
A: Using `reference()` or `list*()` functions

**Q: What is the export resource limit per resource group?**
A: 200 resources

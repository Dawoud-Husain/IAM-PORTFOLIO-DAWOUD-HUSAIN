# 1.3 — Manage Azure Subscriptions and Governance

**Domain Weight: 20–25% (HIGH PRIORITY)**

---

## Core Concepts

### Implement and Manage Azure Policy

Azure Policy is a governance service that evaluates Azure resources against business rules written in JSON. Policies enforce standards and assess compliance at scale — they don't control WHO can do things (that's RBAC), but WHAT state resources must be in. You create policy definitions (rules), group them into initiatives, and assign them to scopes.

**Key Objects:**

- **Policy Definition** — a single rule (e.g., "allowed locations," "require tag")
- **Initiative Definition** (Policy Set) — a collection of policy definitions grouped under one goal
- **Assignment** — attaches a definition or initiative to a scope (MG, subscription, RG, or resource)
- **Exemption** — excludes specific resources from an assignment

**Policy Effects (Evaluation Order):**

| Order | Effect | What It Does |
| --- | --- | --- |
| 1 | **Disabled** | Turns off the policy entirely |
| 2 | **Append** | Adds fields to the request (e.g., add IP rules to storage) |
| 2 | **Modify** | Adds/updates/removes tags or properties on existing resources |
| 3 | **Deny** | Blocks the create/update request |
| 4 | **Audit** | Logs a warning in the activity log (non-blocking) |
| 5 | **Manual** | Requires attestation for compliance |
| 6 | **AuditIfNotExists** | Audits if a related resource doesn't exist |
| 7 | **DenyAction** | Blocks specific actions (e.g., delete) |
| Post | **DeployIfNotExists** | Deploys a template if a related resource is missing |

**Mnemonic — Effect Evaluation Order: "DAM-DAD"**
- **D**isabled → **A**ppend/Modify → **D**eny → **A**udit → **D**eployIfNotExists (after RP)

**Key Behaviors:**

- Policies evaluate every **24 hours** automatically + on create/update + on assignment change
- `modify` and `deployIfNotExists` require a **managed identity** on the assignment
- Best practice: start with **audit** effect before switching to **deny**
- Definitions at management group → assignable at any child scope
- Policy can be assigned at MG level but only **resources at subscription/RG level** are evaluated

**Remediation:**

- Only `modify` and `deployIfNotExists` support remediation tasks
- Existing non-compliant resources need a manual **remediation task** — only new/updated resources auto-remediate
- Remediation task can handle up to **50,000 resources**

**Common Built-in Policies:**

- Allowed Locations (deny)
- Allowed VM SKUs (deny)
- Require tag on resources (deny)
- Inherit tag from resource group (modify)
- Not allowed resource types (deny)

---

### Configure Resource Locks

Resource locks prevent accidental deletion or modification of critical resources. Locks apply across ALL users and roles — even Owners are restricted by locks. They inherit from parent to child scopes, and the most restrictive lock wins.

**Lock Types:**

| Lock Type (Portal) | Lock Type (CLI) | Can Read | Can Modify | Can Delete |
| --- | --- | --- | --- | --- |
| **Delete** | CanNotDelete | YES | YES | NO |
| **Read-only** | ReadOnly | YES | NO | NO |

**Key Behaviors:**

- Locks **inherit** from parent scope (subscription → RG → resource)
- Most restrictive lock in the chain takes precedence
- A **Delete lock on any resource** in a RG blocks the entire RG deletion
- **ReadOnly** on a storage account blocks listing keys (that's a POST action, not GET)
- Locks do **NOT** block subscription cancellation — Azure deactivates resources instead
- To create/delete locks you need `Microsoft.Authorization/locks/*` permissions (Owner or User Access Administrator)
- Max **20 locks** per unique scope

---

### Apply and Manage Tags on Resources

Tags are key-value metadata pairs you apply to resources, resource groups, and subscriptions to logically organize them for billing, management, and automation. Tags do NOT inherit — a tag on a resource group is NOT automatically applied to its resources. Use Azure Policy to enforce tag inheritance.

**Tag Limits:**

| Item | Limit |
| --- | --- |
| Tags per resource/RG/subscription | **50** |
| Tag name length | **512 characters** (128 for storage accounts) |
| Tag value length | **256 characters** |
| Can tag management groups? | **NO** |

**Key Behaviors:**

- Tag names are **case-insensitive** for operations; tag values are **case-sensitive**
- Tags do **NOT** inherit from RG/subscription to resources
- Use Azure Policy `modify` effect with "Inherit a tag from the resource group" to enforce inheritance
- Tags are stored as **plain text** — never put sensitive values
- Tag names cannot contain: `<`, `>`, `%`, `&`, `\`, `?`, `/`
- **Tag Contributor** role can tag resources without needing access to the resource itself

**Required Access:**

- Write access to `Microsoft.Resources/tags` (Tag Contributor role) — OR —
- Write access to the resource itself (Contributor role)

---

### Manage Resource Groups

A resource group is a logical container for Azure resources that share a lifecycle. Every resource must belong to exactly one resource group. The RG stores metadata about its resources — the RG location determines where that metadata is stored (important for compliance).

**Key Facts:**

- Resources in an RG can be in **different regions** from the RG itself
- RG location = metadata storage location (compliance consideration)
- A resource can only be in **one RG** at a time
- Deleting a RG **deletes all resources** inside it — irreversible
- **Resources are NOT limited per RG** — but limited to **800 per resource type** per RG (some exceptions)
- Max **980 resource groups** per subscription
- Moving resources between RGs **locks both source and target** during the move (up to 4 hours)
- Moving a resource **changes its resource ID**
- Role assignments **do NOT move** with resources — must be re-created

---

### Manage Subscriptions

An Azure subscription is a billing and access boundary. Subscriptions are organized under management groups and contain resource groups. Managing subscriptions includes moving them between management groups, transferring ownership, and understanding billing scopes.

**Key Facts:**

- Each subscription is associated with **one Microsoft Entra tenant**
- Unlimited subscriptions per Entra tenant
- To move resources between subscriptions: both must be in the **same Entra tenant**
- If tenants differ → transfer subscription ownership first or associate subscriptions to the same tenant
- The destination subscription must be **registered for the resource provider** of the resource being moved
- Moving resources requires `moveResources/action` on source + `write` on destination

---

### Manage Costs by Using Alerts, Budgets, and Azure Advisor Recommendations

Azure Cost Management provides tools to analyze, manage, and optimize cloud spending. Budgets set spending thresholds with notifications, cost alerts keep you informed, and Azure Advisor provides actionable recommendations to reduce waste.

**Budgets:**

- Set at subscription or resource group scope
- Define spending thresholds (e.g., 50%, 75%, 90%, 100% of budget)
- Trigger **email notifications** or **action groups** when thresholds are crossed
- Can trigger automation (e.g., shut down VMs when budget exceeded)
- Budget alerts are based on **actual** or **forecasted** costs

**Cost Alert Types:**

| Alert Type | Trigger | Purpose |
| --- | --- | --- |
| **Budget alert** | Actual or forecasted cost exceeds threshold | Track spending against budget |
| **Credit alert** | EA monetary commitment consumed past threshold | Enterprise Agreement only |
| **Department spending quota alert** | Department spending exceeds a fixed threshold | EA only |
| **Anomaly alert** | Unexpected cost spike detected | Investigate unusual spending |

**Azure Advisor — Cost Recommendations:**

- Identifies **idle/underutilized** resources
- Recommends **right-sizing** VMs
- Flags unused App Service plans
- Suggests **Reserved Instances** or **Savings Plans**
- Recommends deallocating underused VMs
- Available at subscription and RG scope

---

### Configure Management Groups

Management groups provide a hierarchy above subscriptions for applying governance (policies, RBAC) at enterprise scale. All subscriptions within a management group inherit its policies and role assignments. Every Entra tenant has a single root management group.

**Key Facts:**

| Item | Limit |
| --- | --- |
| Management groups per tenant | **10,000** |
| Depth of MG tree | **6 levels** (excluding root and subscription levels) |
| Parent per MG | **1 only** |
| Children per MG | **Unlimited** |
| Subscriptions per MG | **Unlimited** |

**Root Management Group:**

- Automatically created for every Entra tenant
- Cannot be deleted or moved
- All MGs and subscriptions fold up to root
- Global Admin must **elevate access** to manage the root MG
- Best practice: limit policies at root — use higher impact at lower levels

**Behaviors:**

- New subscriptions default to root MG unless a default MG is configured
- Policies and RBAC assignments at MG scope **inherit to all child subscriptions**
- Moving a subscription to a new MG applies the new MG's policies
- Changes can take up to **30 minutes** to propagate (ARM cache)

---

## What The Exam Tests

- Azure Policy effects: which are blocking (deny) vs. logging (audit) vs. auto-fix (modify/deployIfNotExists)
- Policy assignments require managed identity for which effects (modify, deployIfNotExists)
- Difference between policy definition and initiative
- Lock types: CanNotDelete vs. ReadOnly — and what each prevents
- ReadOnly lock on storage = can't list keys
- Tags do NOT inherit from RG to resources
- Max 50 tags per resource/RG/subscription
- Resource group deletion = all resources deleted
- Management group depth limit (6 levels, not counting root or subscription)
- Management groups per tenant (10,000)
- Budget alert types and how to configure them
- Azure Advisor Cost tab recommendations
- Moving resources: same tenant requirement, resource provider registration, locks block moves

---

## Important Limits / Numbers

| Item | Value |
| --- | --- |
| Policy definitions per scope | **500** |
| Initiative definitions per scope | **200** |
| Initiative definitions per tenant | **2,500** |
| Policy/initiative assignments per scope | **200** |
| Policies per initiative | **1,000** |
| Parameters per policy definition | **20** |
| Parameters per initiative | **400** |
| Exclusions (notScopes) per assignment | **400** |
| Remediation task max resources | **50,000** |
| Policy evaluation cycle | **Every 24 hours** |
| Locks per unique scope | **20** |
| Tags per resource/RG/subscription | **50** |
| Tag name length | **512 chars** (128 for storage) |
| Tag value length | **256 chars** |
| Resource groups per subscription | **980** |
| Resources per resource type per RG | **800** (some exceptions) |
| Management groups per tenant | **10,000** |
| MG tree depth | **6 levels** (root + sub excluded) |
| ARM cache refresh for MG changes | **Up to 30 minutes** |

---

## CLI + PowerShell Commands

### Azure Policy

```bash
# Create policy assignment (CLI)
az policy assignment create \
  --name "audit-vm-managed-disks" \
  --display-name "Audit VMs without managed disks" \
  --policy "/providers/Microsoft.Authorization/policyDefinitions/<policyId>" \
  --scope "/subscriptions/<subId>"

# List policy assignments
az policy assignment list --scope "/subscriptions/<subId>"

# Create initiative (policy set) definition
az policy set-definition create \
  --name "myInitiative" \
  --definitions '[{"policyDefinitionId": "/providers/..."}]'

# Check compliance
az policy state list --resource-group "<rgName>"
```

```powershell
# Create policy assignment (PowerShell)
New-AzPolicyAssignment -Name "audit-vm-disks" `
  -DisplayName "Audit VMs without managed disks" `
  -PolicyDefinition (Get-AzPolicyDefinition -Id "<policyId>") `
  -Scope "/subscriptions/<subId>"

# List assignments
Get-AzPolicyAssignment -Scope "/subscriptions/<subId>"

# Create initiative definition
New-AzPolicySetDefinition -Name "myInitiative" `
  -PolicyDefinition '[{"policyDefinitionId": "..."}]'

# Trigger remediation
Start-AzPolicyRemediation -Name "remediateTask" `
  -PolicyAssignmentId "<assignmentId>"
```

### Resource Locks

```bash
# Create lock on resource group (CLI)
az lock create --name "LockRG" \
  --lock-type CanNotDelete \
  --resource-group "<rgName>"

# Create ReadOnly lock
az lock create --name "ReadOnlyLock" \
  --lock-type ReadOnly \
  --resource-group "<rgName>"

# List locks
az lock list --resource-group "<rgName>"

# Delete lock
az lock delete --name "LockRG" --resource-group "<rgName>"
```

```powershell
# Create lock (PowerShell)
New-AzResourceLock -LockName "LockRG" `
  -LockLevel CanNotDelete `
  -ResourceGroupName "<rgName>"

# List locks
Get-AzResourceLock -ResourceGroupName "<rgName>"

# Remove lock
Remove-AzResourceLock -LockId "<lockId>"
```

### Tags

```bash
# Apply tags (CLI)
az tag create --resource-id "<resourceId>" \
  --tags Environment=Prod CostCenter=12345

# Update tags (merge)
az tag update --resource-id "<resourceId>" \
  --tags Team=WebApps --operation Merge

# List tags
az tag list --resource-id "<resourceId>"
```

```powershell
# Apply tags (PowerShell)
$tags = @{"Environment"="Prod"; "CostCenter"="12345"}
New-AzTag -ResourceId "<resourceId>" -Tag $tags

# Update tags (merge)
Update-AzTag -ResourceId "<resourceId>" `
  -Tag @{"Team"="WebApps"} -Operation Merge

# Get tags
Get-AzTag -ResourceId "<resourceId>"
```

### Resource Groups & Management Groups

```bash
# Create resource group
az group create --name "MyRG" --location "eastus"

# Delete resource group
az group delete --name "MyRG"

# Move resource to another RG
az resource move --destination-group "DestRG" --ids "<resourceId>"

# Create management group
az account management-group create --name "MyMG"

# Move subscription into MG
az account management-group subscription add \
  --name "MyMG" --subscription "<subId>"
```

```powershell
# Create resource group
New-AzResourceGroup -Name "MyRG" -Location "eastus"

# Delete resource group
Remove-AzResourceGroup -Name "MyRG"

# Move resource
Move-AzResource -DestinationResourceGroupName "DestRG" `
  -ResourceId "<resourceId>"

# Create management group
New-AzManagementGroup -GroupName "MyMG"

# Move subscription
New-AzManagementGroupSubscription -GroupId "MyMG" `
  -SubscriptionId "<subId>"
```

---

## Confusion Matrix

| Feature | Azure Policy | Azure RBAC |
| --- | --- | --- |
| Controls | WHAT state resources are in | WHO can do WHAT |
| Focus | Resource properties & compliance | User actions & permissions |
| Enforcement | Evaluates on create/update + every 24h | Evaluates on every API call |
| Example | "All VMs must use managed disks" | "User A can create VMs" |
| Deny mechanism | Policy deny effect | No matching role assignment |

| Lock | CanNotDelete | ReadOnly |
| --- | --- | --- |
| Read resource | YES | YES |
| Modify resource | YES | NO |
| Delete resource | NO | NO |
| List storage keys | YES | NO (POST action blocked) |

| Tag Feature | Default Behavior | With Azure Policy |
| --- | --- | --- |
| Inherit from RG to resources | NO | YES (modify effect) |
| Inherit from subscription | NO | YES (modify effect) |
| Require tag on RG | NO | YES (deny effect) |
| Auto-apply default tag value | NO | YES (modify effect) |

| Hierarchy Level | Contains | Governance Applied |
| --- | --- | --- |
| Management Group | Subscriptions + child MGs | Policy + RBAC → inherited down |
| Subscription | Resource Groups | Policy + RBAC + Budgets |
| Resource Group | Resources | Policy + RBAC + Locks + Tags |
| Resource | Itself | Policy + RBAC + Locks + Tags |

---

## Decision Rules

| If... | Then... |
| --- | --- |
| Need to enforce resource standards across many subscriptions | Assign **Azure Policy** at **management group** scope |
| Want to audit first before enforcing | Use **audit** effect first, then switch to **deny** |
| Policy must auto-fix non-compliant resources | Use **modify** or **deployIfNotExists** effect + managed identity |
| Existing resources are non-compliant | Create a **remediation task** (auto-remediation only applies to new/updated) |
| Need to prevent accidental deletion of a production RG | Apply **CanNotDelete** lock at the RG level |
| Need to make a resource completely read-only | Apply **ReadOnly** lock |
| Tags must flow from RG to child resources | Use Azure Policy "Inherit a tag from the resource group" (modify effect) |
| Need to organize subscriptions for enterprise governance | Create **management group** hierarchy |
| Need to track spending and get alerts | Create a **budget** in Cost Management |
| Need recommendations to reduce waste | Check **Azure Advisor → Cost** tab |

---

## 5 Common Scenario Patterns

**Scenario 1: Enforce allowed regions**

> "Contoso must ensure all resources are deployed only to East US and West US."

- Create or use built-in policy: "Allowed locations" (deny effect)
- Assign at **management group** scope to cover all subscriptions
- Resources deployed outside these regions are **blocked**

**Scenario 2: Protect production resources from deletion**

> "An admin accidentally deleted a production SQL database. How to prevent this?"

- Apply a **CanNotDelete** lock on the resource group or the specific resource
- Even Owners cannot delete with the lock in place
- To delete: explicitly remove the lock first, then delete

**Scenario 3: Enforce tagging for cost tracking**

> "All resource groups must have a 'CostCenter' tag. Resources inside should inherit it."

- Policy 1: "Require a tag on resource groups" → **deny** effect → tag name = CostCenter
- Policy 2: "Inherit a tag from the resource group" → **modify** effect → tag name = CostCenter
- Assign both at subscription scope
- Group them into an **initiative** for easier management

**Scenario 4: Moving resources between subscriptions**

> "A team needs to move a set of VMs from Dev subscription to Prod subscription."

- Verify both subscriptions are in the **same Entra tenant**
- Verify the destination subscription is **registered** for `Microsoft.Compute`
- Check for **locks** — ReadOnly locks block moves
- Role assignments will **not move** — must be recreated
- Resource IDs **change** after the move

**Scenario 5: Setting up cost controls**

> "The finance team wants alerts when Azure spending exceeds $10,000/month."

- Cost Management → Budgets → Create → Set $10,000 monthly
- Add alert thresholds at 50%, 75%, 100%
- Configure email recipients or action groups
- Check Azure Advisor → Cost tab for right-sizing recommendations

---

## Exam Traps

> **TRAP 1:** Azure Policy evaluates resource **state/properties** — it does NOT control user permissions (that's RBAC). A policy cannot grant or deny a user's ability to perform an action.

> **TRAP 2:** `modify` and `deployIfNotExists` effects require a **managed identity** on the policy assignment. Without it, remediation fails.

> **TRAP 3:** Tags do **NOT** inherit from resource groups or subscriptions to resources by default. You MUST use Azure Policy to enforce inheritance.

> **TRAP 4:** A **ReadOnly** lock on a storage account **prevents listing keys** (listing keys is a POST action). Users can read data via existing keys but cannot retrieve new ones.

> **TRAP 5:** A **CanNotDelete** lock on ANY resource in a resource group **blocks deletion of the entire RG** — even if the RG itself is unlocked.

> **TRAP 6:** Management group tree supports **6 levels of depth** — this does NOT include the root level or the subscription level. Total visual levels = 8 (root + 6 + subscription).

> **TRAP 7:** Existing non-compliant resources are **NOT auto-remediated** by policy. You must create a **remediation task**. Only new/updated resources are auto-remediated by modify/deployIfNotExists.

> **TRAP 8:** Moving resources between subscriptions requires both to be in the **same Entra tenant**. Different tenants → transfer ownership first.

> **TRAP 9:** Max **50 tags** per resource. If you need more than 50, use a **JSON string** as the tag value to pack multiple values into one tag.

> **TRAP 10:** Azure Policy best practice is to start with **audit** effect, NOT deny. Immediately enforcing deny can break existing automation and deployments.

---

## Rapid-Fire Q&A

**Q: What are the three Azure Policy objects?**
A: Policy definition, initiative definition (policy set), and assignment

**Q: Which policy effects require a managed identity?**
A: `modify` and `deployIfNotExists`

**Q: How often does Azure Policy auto-evaluate compliance?**
A: Every 24 hours

**Q: What is an initiative?**
A: A collection of policy definitions grouped together for a single assignment

**Q: What are the two lock types?**
A: CanNotDelete (Delete) and ReadOnly

**Q: Which lock type blocks listing storage account keys?**
A: ReadOnly (listing keys is a POST action)

**Q: Do locks override RBAC?**
A: Yes — locks apply to ALL users and roles, including Owners

**Q: What permission do you need to create/delete locks?**
A: `Microsoft.Authorization/locks/*` — held by Owner and User Access Administrator

**Q: Do tags inherit from resource groups to resources?**
A: No — not by default. Use Azure Policy with modify effect to enforce inheritance

**Q: What is the max number of tags per resource?**
A: 50

**Q: Can you tag management groups?**
A: No

**Q: What is the max depth of a management group tree?**
A: 6 levels (not counting root or subscription levels)

**Q: How many management groups per tenant?**
A: 10,000

**Q: How many resource groups per subscription?**
A: 980

**Q: What must be true to move resources between subscriptions?**
A: Both subscriptions must be in the same Microsoft Entra tenant

**Q: What happens to role assignments when you move a resource?**
A: They do NOT move — they become orphaned and must be re-created

**Q: What is a budget alert?**
A: A notification triggered when actual or forecasted costs exceed a defined threshold

**Q: Where do you find Azure Advisor cost recommendations?**
A: Azure portal → Advisor → Cost tab

**Q: What is the best-practice starting effect for a new policy?**
A: Audit (observe first, then enforce with deny)

**Q: What PowerShell command creates a resource lock?**
A: `New-AzResourceLock`

**Q: What CLI command creates a policy assignment?**
A: `az policy assignment create`

**Q: What CLI command creates a management group?**
A: `az account management-group create`

**Q: What is the remediation task resource limit?**
A: 50,000 resources

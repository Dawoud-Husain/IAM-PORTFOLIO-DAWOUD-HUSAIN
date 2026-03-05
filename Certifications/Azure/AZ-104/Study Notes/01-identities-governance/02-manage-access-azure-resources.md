# 1.2 — Manage Access to Azure Resources

**Domain Weight: 20–25% (HIGH PRIORITY)**

---

## Core Concepts

### Manage Built-in Azure Roles

Azure Role-Based Access Control (RBAC) is the authorization system built on Azure Resource Manager that controls who can do what on Azure resources. Roles are collections of permissions (Actions, NotActions, DataActions, NotDataActions) that define what operations are allowed. Azure provides 100+ built-in roles, plus you can create custom roles.

**Role Assignment = Security Principal + Role Definition + Scope**

**5 Fundamental (Privileged) Built-in Roles:**

| Role | Permissions | Key Distinction |
| --- | --- | --- |
| **Owner** | Full access + can assign roles | Highest privilege |
| **Contributor** | Full access, **cannot** assign roles | No RBAC management |
| **Reader** | View only (all resources) | Read-only |
| **User Access Administrator** | Manage user access + assign roles | RBAC only, no resource management |
| **Role Based Access Control Administrator** | Assign roles in RBAC | Cannot manage via Azure Policy |

**Mnemonic — 5 Privileged Roles: "OCR-UR"**

- **O**wner = everything
- **C**ontributor = everything minus RBAC
- **R**eader = view only
- **U**ser Access Admin = RBAC management
- **R**BAC Admin = role assignments only

**Role Definition Structure:**

- `Actions` — control plane operations allowed (e.g., `Microsoft.Compute/virtualMachines/*`)
- `NotActions` — subtracted from Actions (NOT a deny rule)
- `DataActions` — data plane operations allowed (e.g., read blob data)
- `NotDataActions` — subtracted from DataActions
- `AssignableScopes` — where the role can be assigned

**Effective Permissions Formula:**

- `Actions - NotActions = Effective control plane permissions`
- `DataActions - NotDataActions = Effective data plane permissions`

**Control Plane vs. Data Plane:**

| Plane | What It Controls | Example |
| --- | --- | --- |
| **Control plane** | Azure resource management | Create/delete storage account |
| **Data plane** | Data within resources | Read/write blobs inside storage |

- Owner with `*` Actions can manage resources but **cannot** read blob data by default
- Need data roles (e.g., Storage Blob Data Reader) for data plane access

**Custom Roles:**

- Create when built-in roles don't meet needs
- Can use portal, PowerShell, CLI, or REST API
- Start by cloning a built-in role (easiest method)
- `AssignableScopes` must include at least 1 management group, subscription, or resource group
- Cannot set `AssignableScopes` to root (`"/"`) — only built-in roles have root scope
- Custom roles with `DataActions` **cannot** be assigned at management group scope

**Common Job Function Roles (Exam Favorites):**

| Role | What It Does |
| --- | --- |
| **Virtual Machine Contributor** | Manage VMs (not access, not VNet/storage) |
| **Storage Blob Data Reader** | Read blob containers and data |
| **Storage Blob Data Contributor** | Read/write/delete blobs |
| **Storage Blob Data Owner** | Full blob access + assign roles |
| **Network Contributor** | Manage networks (not access) |
| **Monitoring Reader** | Read monitoring data |
| **Backup Operator** | Manage backups (except removing, vault creation) |

---

### Assign Roles at Different Scopes

Azure RBAC uses a hierarchy of scopes — permissions assigned at a parent scope are inherited by all child scopes. The four scope levels from broadest to narrowest are: Management Group, Subscription, Resource Group, and Resource. Understanding inheritance is critical for the exam.

**Scope Hierarchy (Top → Bottom):**

```
Management Group
  └── Subscription
        └── Resource Group
              └── Resource
```

**Inheritance Rules:**

- Roles assigned at a **parent** scope automatically apply to **all child** scopes
- Reader at management group = can read everything in all subscriptions below
- Contributor at resource group = manage all resources in that RG only
- Cannot block inheritance (no way to "break" it at child scope)

**Scope Format (CLI/PowerShell):**

```
/providers/Microsoft.Management/managementGroups/{mgName}
/subscriptions/{subId}
/subscriptions/{subId}/resourceGroups/{rgName}
/subscriptions/{subId}/resourceGroups/{rgName}/providers/{provider}/{resourceType}/{resourceName}
```

**Security Principals (Who Can Be Assigned Roles):**

| Principal Type | Description |
| --- | --- |
| **User** | Individual Entra ID identity |
| **Group** | Set of users — all members inherit the role |
| **Service Principal** | Identity for applications/services |
| **Managed Identity** | Auto-managed identity for Azure services |

**Prerequisites to Assign Roles:**

- Need `Microsoft.Authorization/roleAssignments/write` permission
- Roles that grant this: **Owner**, **User Access Administrator**, **Role Based Access Control Administrator**
- Contributor **cannot** assign roles (most common exam trap for this sub-objective)

**Best Practice: Least Privilege**

- Assign narrowest role at narrowest scope
- Prefer resource group scope over subscription scope
- Prefer Reader over Contributor when only viewing is needed
- Use groups instead of individual users for easier management

---

### Interpret Access Assignments

Interpreting access means understanding effective permissions by evaluating all role assignments, deny assignments, and inheritance across scopes. The Azure portal provides "Check access" on the Access control (IAM) blade to view effective permissions for any security principal. The exam tests your ability to trace through layered assignments.

**How Azure RBAC Evaluates Access (Flow):**

1. User makes a request to Azure Resource Manager
2. Azure RBAC gathers all role assignments and deny assignments at the resource's scope
3. **Deny assignments checked first** — if denied, access is blocked
4. Role assignments evaluated — checks if user has a role with the action
5. If role includes `Actions` with wildcard (`*`), `NotActions` are subtracted
6. If conditions exist on role assignment, they are evaluated
7. If user has no matching role at requested scope → **access denied**

**Key Rule: RBAC is additive (allow-based)**

- If you have 2 role assignments, you get the **union** of all permissions
- Reader + Contributor at same scope = effectively Contributor (Reader permissions are a subset)
- There is no explicit "deny" in normal role assignments

**Deny Assignments:**

- Block actions **even if** a role assignment grants access
- **Cannot be created directly by users** — only by Azure (Blueprints, managed apps, deployment stacks)
- Take precedence over role assignments
- Can exclude specific principals
- Can prevent inheritance to child scopes

**Check Access (Portal):**

- Navigate to any resource → Access control (IAM) → **Check access** tab
- Search for a user/group/service principal
- Shows: role assignments at current scope + inherited from parent scopes
- Shows deny assignments if any exist

**Viewing Role Assignments (Portal):**

- Access control (IAM) → **Role assignments** tab
- Lists all assignments at current scope
- Shows: Role name, Principal, Scope (this resource / inherited)
- Can filter by type (User, Group, Service Principal, Managed Identity)

**NotActions vs. Deny Assignments:**

| Feature | NotActions | Deny Assignment |
| --- | --- | --- |
| Purpose | Subtract from wildcard Actions | Block even if role grants access |
| Is a deny rule? | **NO** | **YES** |
| Can be overridden? | Yes, by another role granting the action | No — always blocks |
| Created by | Role definition | Azure only (Blueprints, deployment stacks) |

---

## What The Exam Tests

- Difference between Owner, Contributor, and Reader (especially: Contributor **cannot** assign roles)
- Scope hierarchy and inheritance (management group → subscription → resource group → resource)
- Which roles can assign other roles (Owner, User Access Admin, RBAC Admin — NOT Contributor)
- Control plane vs. data plane (Owner ≠ blob data access)
- Custom role limits (5,000 per tenant, cannot assign at management group scope if has DataActions)
- Role assignment limits (4,000 per subscription, 500 per management group)
- NotActions is NOT a deny — it only subtracts from wildcard
- Deny assignments cannot be created directly by users
- How to check effective access for a user (Check access on IAM blade)
- Additive model: multiple role assignments = union of permissions

---

## Important Limits / Numbers

| Item | Value |
| --- | --- |
| Role assignments per subscription | **4,000** |
| Role assignments per management group | **500** |
| Custom roles per tenant | **5,000** |
| AssignableScopes per custom role | **2,000** |
| Deny assignments per subscription (system-managed) | **2,000** |
| Built-in roles (approximate) | **100+** |
| Scope levels | **4** (Mgmt Group, Sub, RG, Resource) |
| Custom role AssignableScopes mgmt groups | **1 only** per custom role |

---

## CLI + PowerShell Commands

### Azure CLI

```bash
# List all role assignments in a subscription
az role assignment list --output table

# List role assignments for a specific user
az role assignment list --assignee user@contoso.com --output table

# Assign a role at resource group scope
az role assignment create \
  --assignee user@contoso.com \
  --role "Contributor" \
  --resource-group "myRG"

# Assign a role at subscription scope
az role assignment create \
  --assignee user@contoso.com \
  --role "Reader" \
  --scope "/subscriptions/{sub-id}"

# Remove a role assignment
az role assignment delete \
  --assignee user@contoso.com \
  --role "Contributor" \
  --resource-group "myRG"

# List all built-in role definitions
az role definition list --output table

# Show a specific role definition
az role definition list --name "Contributor"

# Create a custom role from JSON file
az role definition create --role-definition @custom-role.json

# Delete a custom role
az role definition delete --name "My Custom Role"
```

### PowerShell (Az Module)

```powershell
# List all role assignments at a scope
Get-AzRoleAssignment -ResourceGroupName "myRG"

# List role assignments for a specific user (including group memberships)
Get-AzRoleAssignment -SignInName "user@contoso.com" -ExpandPrincipalGroups

# Assign a role
New-AzRoleAssignment `
  -SignInName "user@contoso.com" `
  -RoleDefinitionName "Contributor" `
  -ResourceGroupName "myRG"

# Assign at subscription scope
New-AzRoleAssignment `
  -SignInName "user@contoso.com" `
  -RoleDefinitionName "Reader" `
  -Scope "/subscriptions/{sub-id}"

# Remove a role assignment
Remove-AzRoleAssignment `
  -SignInName "user@contoso.com" `
  -RoleDefinitionName "Contributor" `
  -ResourceGroupName "myRG"

# List role definitions
Get-AzRoleDefinition | Where-Object { $_.IsCustom -eq $true }

# Get specific role definition
Get-AzRoleDefinition -Name "Contributor"

# Create custom role
New-AzRoleDefinition -InputFile "custom-role.json"

# List deny assignments
Get-AzDenyAssignment -Scope "/subscriptions/{sub-id}"
```

---

## Confusion Matrix

| Feature | Owner | Contributor | Reader |
| --- | --- | --- | --- |
| Manage resources | YES | YES | NO |
| Assign RBAC roles | YES | **NO** | NO |
| View resources | YES | YES | YES |
| Delete resources | YES | YES | NO |
| Access data plane | NO (need data role) | NO (need data role) | NO |

| Feature | User Access Admin | RBAC Admin |
| --- | --- | --- |
| Assign roles | YES | YES |
| Manage resources | NO | NO |
| Manage via Azure Policy | YES | **NO** |
| Assign Owner role | YES | YES |

| Feature | Actions/NotActions | DataActions/NotDataActions |
| --- | --- | --- |
| Plane | Control plane | Data plane |
| Example | Create VM, delete RG | Read blobs, write queues |
| Managed by | Azure Resource Manager | Resource provider or ARM |

| Feature | NotActions | Deny Assignments |
| --- | --- | --- |
| Is a deny? | NO — just subtracts from wildcard | YES — blocks even if allowed |
| Can be overridden? | Yes, by another role | No |
| User-created? | Yes (in custom roles) | No — Azure only |

| Feature | Built-in Roles | Custom Roles |
| --- | --- | --- |
| Created by | Microsoft | You |
| AssignableScopes | Root (`"/"`) | Specific scopes (not root) |
| Limit | 100+ (no cap) | 5,000 per tenant |
| DataActions at mgmt group scope | YES | **NO** |

---

## Decision Rules

| If... | Then... |
| --- | --- |
| User needs full control including RBAC | Assign **Owner** |
| User needs full control but NOT RBAC | Assign **Contributor** |
| User needs view-only access | Assign **Reader** |
| User needs to manage role assignments only | Assign **User Access Administrator** or **RBAC Admin** |
| User needs to read blob data | Assign **Storage Blob Data Reader** (not Reader!) |
| Need to limit a role to a specific resource | Assign at **resource scope** (narrowest) |
| Same role needed across many subscriptions | Assign at **management group scope** |
| Built-in roles don't fit requirements | Create a **custom role** |
| Custom role needs data plane actions | **Cannot** assign at management group scope |
| User has Contributor but can't assign roles | Expected — Contributor lacks `Microsoft.Authorization/*/Write` |
| 2 roles assigned to same user at same scope | User gets **union** of both (additive) |

---

## 5 Common Scenario Patterns

**Scenario 1: Contributor can't assign roles**

> "An admin with Contributor role at the subscription scope tries to assign Reader role to a user but gets a permission error."

- Root cause: Contributor does **not** have `Microsoft.Authorization/roleAssignments/write`
- Fix: Assign **Owner** or **User Access Administrator** role to the admin

**Scenario 2: Owner can't read blob data**

> "A subscription Owner tries to view blobs in a storage account using Microsoft Entra authentication but gets Access Denied."

- Root cause: Owner has control plane `*` but no **DataActions** for blobs
- Fix: Assign **Storage Blob Data Reader** (or higher data role) in addition to Owner
- The wildcard `*` in Actions does NOT grant data plane access

**Scenario 3: Inheriting permissions from parent scope**

> "A user is assigned Reader at the subscription level. Can they view resources in Resource Group A?"

- Answer: **YES** — role assignments at subscription scope inherit to all RGs and resources below
- If they also have Contributor at RG-B, they get Contributor at RG-B and Reader at RG-A

**Scenario 4: Custom role for specific task**

> "Contoso needs a role that allows users to restart VMs but nothing else."

- Create a custom role with: `Actions: ["Microsoft.Compute/virtualMachines/restart/action"]`
- Set `AssignableScopes` to the relevant subscription(s)
- Assign the custom role at the resource group or subscription scope

**Scenario 5: Determining effective access**

> "A user has Reader at subscription, Contributor at RG-A, and there's a deny assignment blocking delete on RG-A. What can the user do?"

- At RG-A: Contributor (inherited from direct assignment) + Reader (inherited from subscription)
- Effective = Contributor (additive model)
- **BUT**: deny assignment blocks delete → user can do everything except delete at RG-A
- Deny assignments always win over role assignments

---

## Exam Traps

> **TRAP 1:** **Contributor cannot assign roles.** Contributor has `NotActions` that exclude `Microsoft.Authorization/*/Write` and `Microsoft.Authorization/*/Delete`. The exam loves testing this.

> **TRAP 2:** **NotActions is NOT a deny rule.** If a user has one role with an action in NotActions and another role that grants that action, the user CAN perform it. NotActions only subtracts from the wildcard in the same role definition.

> **TRAP 3:** **Owner does NOT automatically get data plane access.** Owner's `*` wildcard only covers control plane Actions. To read blob data, you need a data role like Storage Blob Data Reader.

> **TRAP 4:** **Deny assignments cannot be created directly.** They are only created by Azure services (Blueprints, managed apps, deployment stacks). You cannot create a deny assignment via portal, CLI, or PowerShell.

> **TRAP 5:** **Custom roles with DataActions cannot be assigned at management group scope.** You can create them with a management group in AssignableScopes, but you must assign them at subscription scope or below.

> **TRAP 6:** **Role assignments are additive (union).** There is no "subtract" between role assignments. If you have Reader + Contributor, you effectively have Contributor. The only way to block is via deny assignments.

> **TRAP 7:** **4,000 role assignment limit per subscription.** Eligible role assignments (PIM) and future-scheduled assignments do NOT count toward this limit. Custom role limit is 5,000 per tenant.

> **TRAP 8:** **Reader role does NOT grant data plane read.** Reader can see a storage account exists (control plane) but cannot read the blobs inside it (data plane). Exam often tests this distinction.

> **TRAP 9:** **Custom roles can only have 1 management group in AssignableScopes.** You cannot list multiple management groups. But you can list multiple subscriptions.

> **TRAP 10:** **Scope inheritance cannot be blocked.** If a role is assigned at subscription scope, there is no way to prevent it from applying to child resource groups. The only workaround is a deny assignment (which only Azure can create).

---

## Rapid-Fire Q&A

**Q: Which role can manage all resources AND assign RBAC roles?**
A: Owner

**Q: Which role can manage all resources but CANNOT assign roles?**
A: Contributor

**Q: What permission is needed to assign roles?**
A: `Microsoft.Authorization/roleAssignments/write`

**Q: What are the 4 scope levels in Azure RBAC?**
A: Management Group → Subscription → Resource Group → Resource

**Q: Do permissions inherit from parent to child scopes?**
A: Yes, always — inheritance cannot be broken

**Q: How are effective permissions calculated for Actions?**
A: `Actions - NotActions = Effective permissions`

**Q: Is NotActions a deny rule?**
A: No — it only subtracts from the wildcard in the same role. Another role can still grant the action.

**Q: Can you create deny assignments directly?**
A: No — only Azure (Blueprints, managed apps, deployment stacks) can create them

**Q: What is the role assignment limit per subscription?**
A: 4,000

**Q: What is the custom role limit per tenant?**
A: 5,000

**Q: Can Owner read blob data by default?**
A: No — Owner only has control plane `*`. Need a data role like Storage Blob Data Reader.

**Q: Can custom roles with DataActions be assigned at management group scope?**
A: No — they must be assigned at subscription scope or below

**Q: What happens when a user has both Reader and Contributor at the same scope?**
A: They get Contributor (additive — union of both roles)

**Q: Where do you check a user's effective access in the portal?**
A: Resource → Access control (IAM) → Check access tab

**Q: What CLI command assigns a role?**
A: `az role assignment create`

**Q: What PowerShell cmdlet lists role assignments?**
A: `Get-AzRoleAssignment`

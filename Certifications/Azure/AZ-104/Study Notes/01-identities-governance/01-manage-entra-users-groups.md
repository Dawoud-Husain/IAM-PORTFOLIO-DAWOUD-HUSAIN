# 1.1 — Manage Microsoft Entra Users and Groups

**Domain Weight: 20–25% (HIGH PRIORITY)**

---

## Core Concepts

### Create Users and Groups

Microsoft Entra ID is Microsoft's cloud-based identity and access management service, replacing Azure Active Directory. Users and groups are the foundational objects — users represent individual identities, and groups bundle users for access control, licensing, and policy. Administrators create these objects via the Entra admin center, Azure CLI, PowerShell, or bulk CSV import.

**User Types:**

- **Member** — internal employee, full default directory permissions
- **Guest** — external B2B user, restricted directory permissions by default
- **Cloud-only** — exists only in Entra ID
- **Synced** — synchronized from on-premises AD via Entra Connect

**Creating a User (Portal):**

- Entra admin center → Users → All users → New user → Create new user
- Required fields: Display name, User principal name (UPN)
- Auto-generates temporary password; user must change at first sign-in
- Can assign up to 20 groups/roles at creation time
- Can assign only 1 administrative unit at creation time

**Bulk Create:**

- Portal: Users → Bulk operations → Bulk create (CSV template)
- CSV must include: `DisplayName`, `UserPrincipalName`, `PasswordProfile`
- Bulk ops can time out on large tenants → use PowerShell as workaround

**Group Types:**

| Type                    | Purpose                      | Licensing Support |
| ----------------------- | ---------------------------- | ----------------- |
| **Security**      | Access control, licensing    | YES               |
| **Microsoft 365** | Collaboration (email, Teams) | NO                |

**Membership Types:**

| Type                     | How Members Are Added      | License Needed |
| ------------------------ | -------------------------- | -------------- |
| **Assigned**       | Manually by admin          | Free           |
| **Dynamic User**   | Auto via attribute rules   | P1             |
| **Dynamic Device** | Auto via device attributes | P1             |

**Dynamic Group Rules (examples):**

- All members: `user.objectId -ne null`
- Only guests: `user.userType -eq "Guest"`
- By dept: `user.department -eq "Sales"`

**Role-Assignable Groups:**

- Requires P1/P2 license
- Requires Privileged Role Administrator role
- Membership type automatically set to **Assigned** (cannot be Dynamic)

**Mnemonic — Group Membership: "SAD"**

- **S**ecurity group = supports licensing
- **A**ssigned = manual
- **D**ynamic = automatic (P1 required)

---

### Manage User and Group Properties

Microsoft Entra user properties control identity details, job information, contact data, and settings used by downstream features like licensing and SSPR. Group properties determine membership type, whether the group can hold Entra roles, and how it integrates with Microsoft 365. These properties can be updated in bulk via CSV or PowerShell.

**Key User Properties:**

- `UserPrincipalName` — unique login ID (format: user@domain.com)
- `DisplayName` — shown in directory and email
- `UsageLocation` — **REQUIRED before assigning a license** (2-letter country code)
- `AccountEnabled` — true/false; disabling doesn't delete
- `UserType` — Member or Guest (can be converted)
- `JobTitle`, `Department`, `Manager` — organizational attributes
- `MailNickname` — required; used as email alias

**Soft Delete:**

- Deleted users/groups go to **Deleted items** for **30 days**
- Can be restored within 30 days; permanently deleted after
- Bulk restore supported

**Group Properties:**

- Group name **cannot start with a space**
- M365 groups get a **Group email address** option
- Toggle "Microsoft Entra roles can be assigned to the group" → YES to enable role assignment (P1/P2 required)

**Changing UserType:**

- Member → Guest or Guest → Member is possible via portal or PowerShell
- Should only be changed if organizational relationship actually changes

---

### Manage Licenses in Microsoft Entra ID

Group-based licensing in Microsoft Entra ID allows admins to assign product licenses to security groups, automatically applying and removing licenses as users join or leave the group. Since September 2024, license assignment via the Entra admin center UI has been removed — administrators must use the Microsoft 365 Admin Center (APIs and PowerShell remain available).

**Key Facts:**

- **As of September 1, 2024:** License assignment UI removed from Entra portal → use **M365 Admin Center** (admin.microsoft.com)
- API and PowerShell access **remain supported**
- Only **Security groups** support group-based licensing (NOT M365 groups)
- Dynamic groups (P1) can be used as license groups for automatic assignment

**License Assignment Flow:**

1. M365 Admin Center → Billing → Licenses
2. Select product → Assign → Choose group
3. Members automatically receive licenses; removed when they leave

**UsageLocation Rule:**

- Must be set on each user **before** a license can be assigned
- For group-based licensing: if unset, user inherits the **directory's location**
- Best practice: set UsageLocation during user creation (configure via Entra Connect for synced users)

**Licensing Requirements for Group-Based Licensing:**

- Requires Entra ID P1 (or Microsoft 365 Business Premium / Office 365 E3 or higher) for **every user who benefits**
- Must have enough licenses for all group members

**Direct vs. Group-Based Licensing:**

| Scenario                             | Behavior                                                         |
| ------------------------------------ | ---------------------------------------------------------------- |
| User in licensed group               | Cannot remove license directly from user — must change at group |
| User gets same license from 2 groups | License consumed**only once**                              |
| User has direct + group license      | Both coexist; direct license is separate                         |

**Mnemonic — License Rule: "SUG"**

- **S**ecurity groups only
- **U**sageLocation must be set first
- **G**roup changes auto-apply (join = assign, leave = remove)

---

### Manage External Users

Microsoft Entra B2B (Business-to-Business) collaboration allows external users to be invited into your tenant as **Guest** users. Guests authenticate with their own organization's credentials or a social/OTP identity, and access is governed by the inviting organization's policies. Guest accounts are created automatically upon invitation.

**Invite Guest User (Portal):**

- Entra admin center → Users → New user → **Invite external user**
- Enter email address → optional message → send invitation
- Guest account added to directory with UserType = **Guest**
- Invitation does **not expire**

**Guest UPN Format:**

- `emailaddress#EXT#@yourdomain.onmicrosoft.com`
- Example: `john_contoso.com#EXT#@fabrikam.onmicrosoft.com`

**Guest Default Permissions:**

- Cannot enumerate all users/groups in directory
- Can manage their own profile
- Cannot read all directory information
- Phone sign-in NOT supported for external users
- B2B accounts not supported in Teams **shared channels** (use B2B direct connect instead)

**External Collaboration Settings (Control who can invite):**

- Entra admin center → External Identities → External collaboration settings
- Options: All users can invite / Admins + member users / Admins only / No one
- Can allow/block specific domains

**Cross-Tenant Access Settings (Control what they can access):**

- Control inbound/outbound B2B collaboration per organization
- Can trust MFA claims from external Entra orgs

**Guest User States:**

| State              | Meaning                       |
| ------------------ | ----------------------------- |
| Pending acceptance | Invitation sent, not redeemed |
| Active             | Invitation redeemed           |

**Converting UserType:**

- Guest → Member possible via portal or PowerShell
- Only if organizational relationship changes (e.g., contractor becomes employee)

---

### Configure Self-Service Password Reset (SSPR)

SSPR lets users change or reset their own passwords without admin/helpdesk involvement. It is configured at the tenant level with options for scope (None/Selected/All), authentication methods, registration requirements, and on-premises writeback for hybrid environments. The exam focuses heavily on scope limitations, method requirements, and admin behavior differences.

**SSPR Scope Options:**

| Option             | Who Can Use SSPR                                 |
| ------------------ | ------------------------------------------------ |
| **None**     | No users                                         |
| **Selected** | 1 Entra security group (nested groups supported) |
| **All**      | All users in tenant                              |

> **EXAM TRAP:** "Selected" = **only 1 group** via the Entra admin center

**Authentication Methods (Available):**

- Mobile app notification / code (Microsoft Authenticator)
- Email (external address)
- Mobile phone (SMS/call)
- Office phone
- Security questions (minimum 3 registered, minimum 3 to reset)
- App password (legacy MFA)

**Method Requirements:**

- Users: admin configures **1 or 2 methods** required to reset
- **Admins always require 2 methods** (cannot be changed — hardcoded policy)
- Best practice: require 1 more method to register than needed to reset

**Registration Settings:**

- Require registration at next sign-in → YES (recommended)
- Reconfirmation interval: 90–180 days

**Password Writeback (Hybrid):**

- Required for synced/federated users to reset passwords in cloud and sync back to on-prem AD
- Requires: **Microsoft Entra Connect** (or cloud sync) + **Entra ID P1 license**
- Enable in: Entra Connect wizard → Optional features → Password writeback
- Enable in SSPR: Password reset → On-premises integration → Write back passwords
- Optional: Allow users to **unlock accounts without resetting password**
- Supports: Password hash sync, Pass-through auth, Federation (ADFS)

**Mnemonic — SSPR Admin Rule: "SARA"**

- **S**elected = 1 group only
- **A**dmins always need 2 methods
- **R**egistration required at sign-in (recommended)
- **A**uthentication writeback needs P1 + Entra Connect

---

## What The Exam Tests

- Which group type supports group-based licensing (Security, NOT M365)
- Where to manage licenses after Sept 2024 (M365 Admin Center)
- What must be set before assigning a license (UsageLocation)
- SSPR scope: "Selected" = how many groups (1)
- How many SSPR methods admins need (2, always)
- What's required for password writeback (Entra Connect + P1)
- Soft delete window (30 days)
- Dynamic group license requirement (P1)
- Guest UPN format (#EXT#)
- Role-assignable groups: membership type becomes Assigned (not Dynamic)
- Bulk operations: CSV file format required
- Group name cannot start with a space (portal blocks this)

---

## Important Limits / Numbers

| Item                                             | Value                                                 |
| ------------------------------------------------ | ----------------------------------------------------- |
| Soft delete window (users/groups)                | **30 days**                                     |
| Groups/roles assignable at user creation         | **Up to 20**                                    |
| Administrative units assignable at user creation | **1 only**                                      |
| SSPR groups via Entra admin center               | **1 group** (nested OK)                         |
| Admin SSPR methods required                      | **2 always**                                    |
| Dynamic group membership rules in builder        | **Up to 5 expressions** (use text box for more) |
| License UI removed from Entra portal             | **September 1, 2024**                           |
| Security questions minimum registered            | **3**                                           |
| Security questions minimum to reset              | **3**                                           |
| SSPR reconfirmation interval (recommended)       | **90–180 days**                                |

---

## CLI + PowerShell Commands

### Azure CLI

```bash
# Create user
az ad user create \
  --display-name "John Doe" \
  --password "TempPass123!" \
  --user-principal-name "john@contoso.com" \
  --force-change-password-next-login true \
  --mail-nickname "john"

# Create group
az ad group create \
  --display-name "Sales Team" \
  --mail-nickname "salesteam"

# Add member to group
az ad group member add \
  --group "Sales Team" \
  --member-id <user-object-id>

# List users
az ad user list --output table

# Delete user
az ad user delete --id john@contoso.com
```

### Microsoft Graph PowerShell (New — use these, NOT MSOnline)

```powershell
# Connect
Connect-MgGraph -Scopes "User.ReadWrite.All", "Group.ReadWrite.All"

# Create user
$passwordProfile = @{ Password = "TempPass123!" }
New-MgUser -DisplayName "John Doe" `
  -UserPrincipalName "john@contoso.com" `
  -MailNickname "john" `
  -PasswordProfile $passwordProfile `
  -AccountEnabled

# Create security group
New-MgGroup -DisplayName "Sales Team" `
  -MailNickname "salesteam" `
  -MailEnabled:$false `
  -SecurityEnabled

# Get users
Get-MgUser -Filter "UserType eq 'Member'"
Get-MgUser -Filter "accountEnabled eq false"

# Add user to group
New-MgGroupMember -GroupId <group-id> -DirectoryObjectId <user-id>

# Update group
Update-MgGroup -GroupId <group-id> -Description "Updated description"

# Delete group
Remove-MgGroup -GroupId <group-id>

# Restore deleted user
Restore-MgDirectoryDeletedItem -DirectoryObjectId <deleted-user-id>
```

> **NOTE:** `MSOnline` and `AzureAD` modules deprecated as of March 30, 2024. Use `Microsoft.Graph` PowerShell module.

---

## Confusion Matrix

| Feature                | Security Group | M365 Group |
| ---------------------- | -------------- | ---------- |
| Supports licensing     | YES            | NO         |
| Email enabled          | NO             | YES        |
| Used in Teams          | NO             | YES        |
| Can be dynamic         | YES (P1)       | YES (P1)   |
| Can assign Entra roles | YES (P1)       | NO         |

| Feature                       | Member User | Guest User                  |
| ----------------------------- | ----------- | --------------------------- |
| Default directory permissions | Full member | Restricted                  |
| Can enumerate all users       | YES         | NO                          |
| UPN format                    | user@domain | email#EXT#@domain           |
| Phone sign-in                 | YES         | NO                          |
| Teams shared channels         | YES         | NO (use B2B direct connect) |

| SSPR Scope | Who Gets SSPR       |
| ---------- | ------------------- |
| None       | Nobody              |
| Selected   | 1 group (nested ok) |
| All        | Entire tenant       |

| License Management                  | Portal/UI Available? |
| ----------------------------------- | -------------------- |
| Entra admin center (post Sept 2024) | NO (UI removed)      |
| M365 Admin Center                   | YES                  |
| PowerShell / API                    | YES                  |

---

## Decision Rules

| If...                                           | Then...                                                                |
| ----------------------------------------------- | ---------------------------------------------------------------------- |
| Need group-based licensing                      | Use**Security group** (not M365)                                 |
| User can't be assigned a license                | Check**UsageLocation** is set                                    |
| Want auto-assignment based on attributes        | Use**Dynamic User** group (P1 required)                          |
| Need to assign Entra roles to a group           | Enable**role-assignable** toggle (P1, membership = Assigned)     |
| Synced user needs cloud SSPR to sync to on-prem | Enable**password writeback** (Entra Connect + P1)                |
| SSPR for pilot group only                       | Set scope to**Selected**, pick 1 group                           |
| Admin lost access to account                    | Requires**2 SSPR methods** — no exceptions                      |
| Guest account showing "Pending acceptance"      | Resend the invitation email                                            |
| Deleted user needs restoration                  | Must be within**30 days**                                        |
| SSPR license fails                              | Check user has**Entra P1** (or M365 Business Premium / O365 E3+) |

---

## 5 Common Scenario Patterns

**Scenario 1: Automatic license assignment**

> "Contoso wants to automatically assign Microsoft 365 E3 licenses to all employees in the Sales department."

- Create a **Dynamic Security Group** with rule `user.department -eq "Sales"`
- Requires **P1 license**
- Assign M365 E3 license to the group via **M365 Admin Center**
- Set UsageLocation on all users before assignment

**Scenario 2: SSPR pilot deployment**

> "An admin wants to test SSPR with the IT Help Desk group before rolling it out company-wide."

- Entra admin center → Password reset → Properties → **Selected**
- Pick the **IT Help Desk** group (1 group limit)
- Nested groups inside that group are supported

**Scenario 3: Hybrid user password reset**

> "A user synced from on-premises AD resets their password via SSPR but it doesn't update on-prem."

- Root cause: **password writeback** not enabled
- Fix: Enable in Entra Connect + enable in SSPR → On-premises integration
- Requires: **Entra Connect** + **P1 license**

**Scenario 4: External vendor access**

> "Contoso needs to give a partner company (Fabrikam) access to a SharePoint site without creating new accounts."

- Use **B2B collaboration** — invite as guest
- Invite via Entra admin center → Users → Invite external user
- Guest UPN: `vendor@fabrikam.com#EXT#@contoso.onmicrosoft.com`

**Scenario 5: License assignment failing**

> "An admin attempts to assign an Office 365 license to a new user but gets an error."

- Check: Is **UsageLocation** set on the user? (Required before license assignment)
- Check: Are there enough licenses available?
- Check: Is the user in a group where service plans conflict?

---

## Exam Traps

> **TRAP 1:** "Selected" in SSPR = **1 group only** via the admin center. Selecting a group with nested groups works, but you can only pick 1 parent group.

> **TRAP 2:** Admins **always need 2 SSPR methods** — this is hardcoded. Even if policy says "1 method," admins require 2.

> **TRAP 3:** M365 (Office 365) groups **do NOT support group-based licensing** — only **Security groups** do.

> **TRAP 4:** You must set **UsageLocation** before assigning any license. Forgetting this is the #1 license assignment failure cause on the exam.

> **TRAP 5:** License management via **Entra admin center UI is gone since September 2024** — use M365 Admin Center. API/PowerShell still work.

> **TRAP 6:** Role-assignable groups **force membership type = Assigned** — you cannot make them Dynamic.

> **TRAP 7:** Soft delete is **30 days** — not 7, not 14, not 60. After 30 days, permanently deleted and **unrecoverable**.

> **TRAP 8:** Password writeback requires **Entra Connect** (or cloud sync) AND **P1 license** — both are required, not just one.

> **TRAP 9:** Dynamic group membership rules require **P1 for the org**, not per user — but you need enough P1 licenses for all benefiting users.

> **TRAP 10:** Group name **cannot start with a space** — the portal blocks creation if it does.

---

## Rapid-Fire Q&A

**Q: Which group type supports group-based licensing?**
A: Security groups only (not M365 groups)

**Q: What must be configured on a user before assigning a license?**
A: UsageLocation

**Q: How many groups can you select in SSPR "Selected" mode?**
A: 1 group (nested groups inside are supported)

**Q: How many SSPR methods do admins always need?**
A: 2 (hardcoded, cannot be changed)

**Q: What is the soft delete window for users?**
A: 30 days

**Q: What license is required for Dynamic groups?**
A: Entra ID P1 (or Microsoft 365 Business Premium / Office 365 E3+)

**Q: Where do you manage licenses via UI after September 2024?**
A: Microsoft 365 Admin Center (admin.microsoft.com)

**Q: What does the guest user UPN look like?**
A: `email#EXT#@yourtenant.onmicrosoft.com`

**Q: What is required for password writeback?**
A: Microsoft Entra Connect + P1 license

**Q: Can you make a role-assignable group dynamic?**
A: No — membership type is forced to Assigned

**Q: What PowerShell cmdlet creates a user in Microsoft Graph?**
A: `New-MgUser`

**Q: What CLI command creates an Entra user?**
A: `az ad user create`

**Q: Can a user be in more than one licensed group with the same license?**
A: Yes, but the license is consumed only once

**Q: What happens to a guest's access if you don't resend an expired invitation?**
A: Invitations don't expire — but the guest must redeem it; resend if status is "Pending acceptance"

**Q: Which PowerShell module replaces MSOnline and AzureAD?**
A: Microsoft Graph PowerShell (`Microsoft.Graph`)

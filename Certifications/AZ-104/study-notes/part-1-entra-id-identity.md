# AZ-104 Study Notes - Part 1: Entra ID & Identity

## Source
John Savill AZ-104 Administrator Associate Study Cram v2 - Part 1 of 8

---

## Topics Covered

### Microsoft Entra ID (formerly Azure AD)

#### Core Concepts

- [ ] ☁️ **Entra ID (formerly Azure AD)**
  - Microsoft's cloud-based identity provider
  - Speaks cloud protocols (OAuth 2.0, OpenID Connect, SAML, WS-Fed)
  - Uses HTTPS/TLS encryption for internet communication
  - Flat structure (no organizational units like AD DS)
  - Accessed via Microsoft Graph (REST-based API over HTTPS port 443)

- [ ] 🏢 **Entra Tenant**
  - Your organization's specific instance of Entra ID
  - Contains users, groups, devices, applications, policies
  - Default domain: [something].onmicrosoft.com
  - Can add custom verified domains
  - Does NOT live inside Azure subscription (it's a global instance)
  - Subscriptions trust a particular tenant

- [ ] 🌐 **Cloud Protocols**
  - **OAuth 2.0**: Authorization protocol
  - **OpenID Connect**: Authentication protocol
  - **SAML**: Security Assertion Markup Language
  - **WS-Fed**: Web Services Federation
  - All work over HTTPS/TLS (internet-friendly)

#### Active Directory Integration

- [ ] 💼 **Active Directory Domain Services (AD DS)**
  - On-premises identity service
  - Uses private network protocols (Kerberos, NTLM, LDAP)
  - Has organizational units (OUs) and hierarchical structure
  - Requires many different network ports
  - Often the source of truth for hybrid environments

- [ ] 🔄 **Entra Connect Sync**
  - Synchronizes AD DS to Entra ID
  - Engine runs on-premises (Windows Server)
  - Flow is always: AD DS → Entra ID (AD is source of truth)
  - Replicates users, groups, and other objects

- [ ] ☁️ **Entra Connect Cloud Sync**
  - Newer synchronization method
  - Engine runs in the cloud
  - Lightweight agents run on domain controllers
  - Same one-way flow: AD DS → Entra ID

#### Account Types

- [ ] 👤 **Cloud Accounts**
  - Created directly in Entra ID
  - No on-premises AD DS presence
  - Managed entirely in the cloud

- [ ] 🔀 **Hybrid Accounts**
  - Created in AD DS
  - Synchronized to Entra ID via Entra Connect
  - AD DS remains source of truth

- [ ] 🌍 **Guest Accounts**
  - External users from other organizations
  - Come from external Entra tenants, Google, Facebook, Microsoft accounts, or SAML providers
  - Have stub object in your tenant, primary identity lives externally
  - Default user type is "Guest" but can be changed to "Member"
  - Benefits: No separate password management, security managed by home organization

- [ ] 👥 **External Users**
  - Users from outside your organization
  - Can authenticate via various identity providers
  - One-time passcode option (email verification)
  - Reduces password sprawl and security risks

#### Account Provisioning Methods

- [ ] ➕ **Manual Creation**
  - Create individual users in portal
  - Assign properties and licenses

- [ ] 📊 **Bulk Operations**
  - Bulk create: Upload CSV template with user data
  - Bulk invite: Invite external users via CSV
  - Download CSV template, edit, upload to create multiple accounts

- [ ] 🏭 **HR System Provisioning**
  - HR systems can provision to Entra provisioning endpoints
  - Can create accounts in AD DS first (then sync to Entra)
  - Can create directly in Entra
  - Automated provisioning from source of truth

- [ ] 📜 **Script-based Creation**
  - Use PowerShell or other automation
  - Microsoft Graph API for programmatic access

#### Groups

- [ ] 👥 **Security Groups**
  - Most common group type
  - Used for access control, role assignment, licensing
  - Can assign to Azure resources and applications

- [ ] 📧 **Microsoft 365 Groups**
  - Used for collaboration tools
  - Includes shared calendars, SharePoint, Teams, etc.

- [ ] ✋ **Assigned Membership**
  - Manually select specific users or devices
  - Static membership

- [ ] 🤖 **Dynamic Membership**
  - Rule-based membership
  - Choose either users OR devices (not both in same group)
  - Rules based on attributes (department, job title, hire date, etc.)
  - Periodically evaluated and updated automatically
  - Example rules: Hired within last 30 days, specific job title, device type

- [ ] 🎯 **Group Usage**
  - Assign licenses to groups (not individual users)
  - Assign roles to groups
  - Assign application access to groups
  - Easier management than per-user assignments

#### Devices

- [ ] 📱 **Device Registration**
  - Device becomes known entity in Entra
  - Apply policies and management (Intune)
  - Suitable for personal/BYOD devices
  - Device shows up as object in tenant

- [ ] 💻 **Device Join (Entra Join)**
  - Full corporate control over device
  - User can authenticate with Entra credentials at login screen
  - Suitable for corporate-owned devices
  - Includes all registration capabilities plus authentication

- [ ] 🔐 **Authentication at Logon**
  - Joined devices can use Entra credentials (john@savtech.net)
  - No need for VPN to domain controller
  - Cloud-first approach for modern workplace

#### Licensing

- [ ] 🆓 **Entra ID Free**
  - Basic functionality
  - Included with Azure/Microsoft 365 subscriptions

- [ ] 📦 **Entra ID P1 (Premium 1)**
  - Conditional Access policies
  - HR-driven provisioning
  - Self-Service Password Reset with writeback to on-premises
  - Hybrid identity features
  - Often bundled with Microsoft 365 E3

- [ ] 🎁 **Entra ID P2 (Premium 2)**
  - Privileged Identity Management (PIM)
  - Identity Protection features
  - Access Reviews
  - Risk-based conditional access
  - Machine learning capabilities
  - Often bundled with Microsoft 365 E5

- [ ] 📊 **Identity Governance Add-on**
  - Lifecycle workflow capabilities
  - Enhanced entitlement management
  - Verified ID
  - Governance dashboard
  - Advanced access reviews and certifications

- [ ] 👨‍💼 **Per-User Licensing**
  - Different users can have different license levels
  - Privileged users may need P2 for PIM
  - Standard users may only need P1
  - Governance users may need additional add-on
  - Assign licenses to groups for easier management

#### Features & Capabilities

- [ ] 🔑 **Self-Service Password Reset (SSPR)**
  - Users can reset their own passwords
  - Reduces help desk burden
  - Cloud accounts: Direct password reset
  - Hybrid accounts: Requires P1 license for writeback to AD DS
  - Configure authentication methods (email, phone, security questions)
  - Can require 1 or 2 authentication methods

- [ ] 🎨 **Company Branding**
  - Customize login experience
  - Configure background images
  - Set custom messages with formatting (bold, underline, special characters)
  - Located in: User Experiences → Company Branding

- [ ] 🌐 **Custom Domains**
  - Default: [name].onmicrosoft.com
  - Add custom domain names
  - Must verify ownership via DNS record
  - Can set as primary domain
  - Used in user principal names

- [ ] 📈 **Microsoft Graph**
  - REST-based API for interacting with Entra ID
  - Works over HTTPS (port 443)
  - Standard way to interact with Microsoft 365, Entra ID, and other services
  - Replaces older APIs

- [ ] 🔗 **Trusted Applications**
  - Azure services trust Entra tenant
  - Microsoft 365 trusts Entra tenant
  - Third-party SaaS applications can trust tenant
  - Internet Access feature: Check access for any internet site
  - Private Access feature: Control access to on-premises TCP/UDP apps

- [ ] 🛡️ **Secure Service Edge (SSE)**
  - Internet Access: Control access to internet sites
  - Private Access: Extend access to on-premises resources
  - Integrated with Entra for authentication/authorization

#### Administrative Features

- [ ] 👑 **Global Administrator Role**
  - Most privileged role in Entra
  - Restrict who has this role
  - Can perform any administrative action

- [ ] 🎭 **Delegated Roles**
  - Grant specific sets of permissions
  - Examples: User Administrator, Security Administrator, etc.
  - Privileged roles indicated in portal
  - Assign to groups rather than individual users

- [ ] 📁 **Administrative Units**
  - Provide delegation at granular level
  - Used since Entra has flat structure (no OUs)
  - Allow scoped administrative permissions

---

## Key Takeaways

1. **Entra ID ≠ Azure AD DS** - Entra is cloud-based with cloud protocols; AD DS is on-premises with private network protocols
2. **Tenant is Independent** - Entra tenant does not live in Azure subscription; subscriptions trust tenants
3. **Synchronization is One-Way** - AD DS → Entra ID (AD is source of truth)
4. **Use Groups for Management** - Assign licenses, roles, and permissions to groups, not individual users
5. **Licensing Matters** - Different features require different license levels (Free, P1, P2, Governance)
6. **Guest Users are Powerful** - External collaboration without password management headaches
7. **Dynamic Groups Save Time** - Rule-based membership automatically maintained
8. **Device Join vs Register** - Join for corporate devices, Register for BYOD
9. **SSPR Reduces Help Desk Load** - But requires P1 for hybrid writeback
10. **Cloud Protocols** - OAuth 2.0, OpenID Connect, SAML work over internet (HTTPS)

---

## Exam Tips

- Know the difference between Entra Connect Sync (on-prem engine) and Cloud Sync (cloud engine)
- Understand when to use P1 vs P2 licensing
- Remember that guests don't need separate password management
- Dynamic groups can be users OR devices, not both
- SSPR writeback to on-premises requires P1 license
- Tenant exists independently of Azure subscriptions
- Microsoft Graph is the modern API for Entra interaction

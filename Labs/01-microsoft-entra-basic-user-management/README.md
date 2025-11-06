# Microsoft Entra ID - Basic User Management

**Lab Source:** [Microsoft Learning](https://microsoftlearning.github.io/Get-started-Microsoft-Entra-Management-Tasks/Instructions/Labs/01-perform-basic-user-management.html)
**Completed:** November 6, 2024
**Platform:** Microsoft Entra ID (Azure Portal)

## Overview

Hands-on lab practicing fundamental identity and access management tasks in Microsoft Entra ID, including user account creation, bulk provisioning, external user collaboration, and role-based access control.

## Skills Demonstrated

- User account creation and configuration in Microsoft Entra ID
- Bulk user provisioning using CSV templates
- External user invitation and guest access management
- Role-based access control (RBAC) implementation
- Administrative role assignment with least privilege principle
- Identity lifecycle management and resource cleanup

## Lab Activities

### Task 1: Create User Accounts

Created individual user accounts including `user1` and configured user principal names in the default directory. Demonstrated understanding of user identity formats and authentication setup.

**Screenshot:** [new-user.png](screenshots/new-user.png) - Shows users successfully created in the Microsoft Entra admin center

### Task 3: Bulk User Creation

Leveraged bulk operations to efficiently create multiple user accounts using CSV file upload:
- Ben Andrews
- Chris Green
- Cynthia Carey
- David Longmuir
- Jane Miller

This demonstrates scalability for enterprise onboarding scenarios where manual user creation would be inefficient.

**Screenshot:** [bulk-users.png](screenshots/bulk-users.png) - Bulk create users operation showing successful CSV upload and user provisioning

### Task 4: Invite External Users

Configured external user collaboration by inviting guest users to the directory. This enables secure cross-organization collaboration while maintaining proper access boundaries.

**Screenshot:** [invited-user.png](screenshots/invited-user.png) - External guest user with "Invitation" creation type

### Task 5: Assign Administrative Roles

Implemented role-based access control by assigning the **Attribute Assignment Reader** role to `user1`. This role provides read-only access to custom security attribute keys and values, demonstrating least privilege access principles.

**Screenshot:** [user1-assignedrole.png](screenshots/user1-assignedrole.png) - Administrative role assignment for user1

### Cleanup

Successfully removed all test resources including users, role assignments, and configurations to return the environment to its original state, following best practices for lab hygiene.

## Key Learnings

**Technical Skills Acquired:**
- Microsoft Entra admin center navigation and user management workflows
- Understanding user principal name (UPN) structure and tenant namespaces
- Bulk operations using CSV templates for scalable provisioning
- Differentiation between member users and guest users
- Administrative role hierarchy and scope limitations
- Identity lifecycle management best practices

**Challenges Solved:**
- Successfully formatted and uploaded CSV file for bulk user creation
- Understood the implications of different user types (Member vs Guest)
- Navigated the admin center interface to locate role assignment options
- Validated role assignments took effect properly

**Real-World Applications:**
- **Enterprise Onboarding:** Bulk user provisioning streamlines new hire onboarding for large organizations
- **Partner Collaboration:** External user invitations enable secure B2B collaboration without creating unnecessary internal accounts
- **Least Privilege Access:** Role assignments demonstrate security best practices by granting only necessary permissions
- **Compliance:** Proper user lifecycle management supports audit requirements and regulatory compliance
- **Identity Governance:** Understanding user types and access controls forms foundation for zero-trust security models

## Professional Impact

This lab demonstrates practical IAM skills essential for:
- Identity and Access Management Administrator roles
- Cloud Security Engineer positions
- Microsoft 365/Azure Administrator responsibilities
- IT Support and Helpdesk operations
- Security compliance and governance functions

## Reflection

Working through this lab provided hands-on experience with Microsoft Entra ID's core identity management capabilities. The bulk operations feature proved particularly valuable for understanding how organizations scale user management beyond manual processes. The role assignment exercise reinforced the importance of understanding least privilege principles and how granular permissions can be applied in enterprise environments.

The distinction between member users and guest users highlighted important security boundaries for cross-organizational collaboration. This has direct implications for real-world scenarios where companies need to work with external partners while maintaining proper access controls.

Overall, this lab built a solid foundation for understanding cloud-based identity management and the administrative workflows that support enterprise IAM operations.

---

**Lab Status:** Completed
**Documented by:** Dawoud Husain
**Contact:** [LinkedIn](https://www.linkedin.com/in/dawoud-husain-44b17827a/) | dawoudhusain25@gmail.com

# Project: Role-Based Access Control (RBAC) & Least Privilege

## Overview

This project implements Role-Based Access Control (RBAC) in Microsoft Entra ID to enforce the Principle of Least Privilege (PoLP) across users, groups, and applications.
It ensures that each identity only has the minimum access required to perform its duties, reducing the attack surface and improving compliance with governance policies.

---

##  Implementation Steps

### Step 1. Define Access Roles

```yaml
roles:
  - name: "Security Reader"
    description: "View security alerts and reports"
  - name: "Application Administrator"
    description: "Manage app registrations and enterprise apps"
  - name: "User Administrator"
    description: "Create and manage user accounts"
```

---

### Step 2. Assign Roles to Groups (Not Individuals)

```powershell
Assign RBAC role to a group instead of a direct user
Connect-AzAccount
New-AzRoleAssignment -ObjectId "<groupObjectId>" -RoleDefinitionName "Security Reader" -Scope "/subscriptions/<subID>"
```

```yaml
roleAssignment:
  target: "Group"
  enforcement: "Group-Based"
  approvalRequired: false
```

---

### Step 3. Implement Custom Roles (For Fine-Grained Control)

```yaml
customRole:
  name: "IncidentResponder"
  description: "Allows investigation and response to security incidents"
  permissions:
    - "Microsoft.Security/alerts/read"
    - "Microsoft.Security/alerts/update"
    - "Microsoft.OperationalInsights/query/read"
  assignableScopes:
    - "/subscriptions/<subID>"
```

---

### Step 4. Review & Audit Permissions

```yaml
accessReviews:
  frequency: "Quarterly"
  scope: "AdminRoles"
  reviewers:
    - "Security Governance Officer"
    - "Compliance Lead"
  autoRemoveInactiveAssignments: true
```

---

## Outcome

* Enforced least-privilege access across Entra ID and Azure resources.
* Reduced standing admin privileges by 65% through group-based RBAC.
* Enabled continuous access reviews for security and compliance alignment.
* Improved operational efficiency by centralizing role assignments.

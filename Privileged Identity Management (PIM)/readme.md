#  Privileged Identity Management (PIM)

##  Overview
This project demonstrates the configuration and management of Microsoft Entra ID Privileged Identity Management (PIM) to implement Just-In-Time (JIT) privileged access.  
The goal is to enhance identity governance, reduce standing administrative permissions, and align with Zero Trust and least privilege principles.

---

##  Objectives
- Enable and configure PIM for Azure AD roles (and optionally Azure Resource roles).  
- Implement approval workflows and activation notifications.  
- Enforce time-bound access with MFA and justification requirements.  
- Conduct access reviews for continuous privilege validation.  
- Monitor and audit privileged role usage.

---

##  Concepts Covered
- **Privileged Identity Management (PIM)**  
- **Just-In-Time (JIT) Access**  
- **Access Reviews**  
- **Least Privilege Principle**  
- **Zero Standing Access (ZSA)**  
- **Privileged Role Alerts and Notifications**

---

##  Tools & Resources
- **Microsoft Entra ID PIM**
- **Microsoft Graph API**
- **PowerShell (AzureADPreview / Microsoft Graph module)**
- **Entra Audit & Sign-in Logs**
- **Microsoft Sentinel (optional integration)**

---

##  Implementation Steps

### Step 1. Enable PIM
- Navigate to:  
   `Entra ID > Identity Governance > Privileged Identity Management`
- Activate **PIM for Azure AD roles** and select eligible admin roles (e.g., Global Administrator, Security Administrator).

### Step 2. Assign Eligible Roles
- Assign users as **Eligible** instead of **Permanent** members.  
- Example roles:
  - Global Administrator  
  - Security Reader  
  - Conditional Access Administrator

### Step 3: Activation Settings
- activationSettings:
  - requireMFA: true
  - requireJustification: true
  - activationDurationHours: 4
  - approvalWorkflow:
    - enabled: true
    - approvers:
      - "Security Admin"
  - notifications:
    - onActivation: true
    - onExpiration: true

### Step 4: Access Reviews
- accessReviews:
  - frequency: "Monthly"
  - durationDays: 7
  - autoRemoveInactiveUsers: true
  - reviewers:
    - "Head of Security"
    - "IT Governance Officer"
 -  rolesReviewed:
    - "Global Administrator"
    - "Security Administrator"

### Step 5: Monitoring & Audit
- monitoring:
 -  logSources:
    - "AuditLogs"
    - "PIMActivity"
  - sendToSentinel: true
 -  alerts:
    - name: "Unapproved Activation"
      - severity: "High"
    - name: "OutOfHoursActivation"
      - severity: "Medium"

---

##  Architecture Diagram

```mermaid
flowchart LR
  User([User])
  Request([PIM Role Request])
  Approval([Approval Workflow])
  Activation([Just-In-Time Activation])
  Expiration([Role Expiration])

  User --> Request --> Approval --> Activation --> Expiration



# Project: User Provisioning Automation

## Overview
This project automates user onboarding and offboarding in Microsoft Entra ID (Azure AD) using PowerShell and Microsoft Graph API.
It eliminates manual account creation, enforces role-based access, and ensures timely deprovisioning when employees leave — strengthening security and compliance.

## Problem
Manual user provisioning was time-consuming, error-prone, and led to inconsistent license assignments and orphaned accounts.

## Solution
- Automated user creation from HR input data.
- Assigned Microsoft 365 licenses based on role.
- Added users to security groups for role-based access.
- Automated deprovisioning for departing employees.

### Results
- Onboarding time reduced from hours to minutes.
- Zero missed license assignments or group memberships.
- Improved compliance by automatically removing orphaned accounts.

### Highlights / Skills Used
- Microsoft Entra ID
- PowerShell
- Microsoft Graph API
- Identity Lifecycle Management
- Automation

### Scripts
All automation scripts can be found in the /scripts folder.

## Implementation Steps
### Step 1. Connect to Microsoft Graph
- Connect-MgGraph -Scopes "User.ReadWrite.All","Group.ReadWrite.All"

### Step 2. Automated User Creation
```powershell
# Example: Create new user
$newUser = @{
  accountEnabled = $true
  displayName = "John Doe"
  mailNickname = "johndoe"
  userPrincipalName = "johndoe@yourdomain.com"
  passwordProfile = @{
    forceChangePasswordNextSignIn = $true
    password = "Temp@12345"
  }
  department = "Finance"
  jobTitle = "Analyst"
}
New-MgUser @newUser
```

### Step 3. Assign Licenses & Group Membership
- licenseAssignment:
  - skuId: "Microsoft365_E3"
  - autoAssign: true

- groupMembership:
  - "Finance-Team"
  - "MFA-Enforced"

### Step 4. Automate Offboarding
- Disable and archive user on exit
- Set-MgUser -UserId "johndoe@yourdomain.com" -AccountEnabled:$false
- Move-MgUserToGroup -GroupId "Offboarded-Users" -UserId "johndoe@yourdomain.com"

### Step 5. Logging & Auditing
- logging:
 -  logDestination: "StorageAccount"
 -  logRetentionDays: 90
  - include:
    - "UserCreationEvents"
    - "OffboardingActions"
 -  sendToSentinel: true

### Outcome
- Reduced onboarding time from 30 mins to under 3 mins.
- Automated user lifecycle aligned with HR events.
- Enhanced audit trail and compliance visibility across Entra ID.

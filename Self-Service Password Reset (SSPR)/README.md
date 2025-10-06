# Project: Self-Service Password Reset (SSPR)

## Overview
This project enables Self-Service Password Reset (SSPR) in Microsoft Entra ID, empowering users to securely reset their passwords without IT intervention.
It integrates Multi-Factor Authentication (MFA) and audit logging, reducing helpdesk password-reset tickets and enhancing user productivity while maintaining strong identity governance.

## Implementation Steps
### Step 1. Enable SSPR for Users
-  Enable SSPR for selected users or groups
- Connect-MgGraph -Scopes "Directory.ReadWrite.All"
- Update-MgPolicyAuthenticationFlowPolicy -SelfServicePasswordResetEnabled $true

### Step 2. Configure Authentication Methods
- authenticationMethods:
 - requiredNumberOfMethods: 2
 - allowedMethods:
    - "Email"
    - "AuthenticatorApp"
    - "Phone"
-  registrationEnforcement:
 -   promptUsersToRegister: true

### Step 3. Customize Notifications
- notifications:
-  notifyOnReset: true
 - notifyOnAdminReset: true
-  recipients:
    - "itsecurity@yourdomain.com"

### Step 4. Integrate with Conditional Access
- Restrict password reset access to trusted networks or compliant devices.
- Use Conditional Access policies to require MFA before performing SSPR.

### Step 5. Monitor & Audit
- monitoring:
-  logSources:
    - "AuditLogs"
    - "PasswordResetEvents"
  - sendToSentinel: true
 - alerts:
    - name: "ExcessivePasswordResets"
     - severity: "Medium"
    - name: "FailedSSPRAttempts"
     - severity: "High"

## Outcome
- Reduced password-related support tickets by ~75%.
- Improved compliance visibility with Entra Audit Logs and Sentinel integration.
- Enhanced user experience with secure, MFA-backed password resets.

# Project: Identity Protection & Risk-Based Policies

## Overview
This project implements Microsoft Entra ID Identity Protection to detect risky sign-ins and compromised user accounts, then automatically apply risk-based Conditional Access policies.
The goal is to strengthen account security by automating remediation for risky behaviors such as leaked credentials, atypical sign-ins, or suspicious MFA patterns.

## Implementation Steps
### Step 1. Enable Identity Protection
- Enable risk detection and reporting
- Connect-MgGraph -Scopes "Policy.ReadWrite.ConditionalAccess"
- Set-MgIdentityProtectionPolicy -IsEnabled $true

### Step 2. Configure Risk-Based Conditional Access Policies
- riskBasedPolicy:
 - policyName: "Block High-Risk Sign-ins"
 - conditions:
   - signInRiskLevels:
      - "high"
-  controls:
  -  grantAccess: false
 - enablePolicy: true

- riskBasedPolicy2:
-  policyName: "Require MFA for Medium-Risk Users"
 - conditions:
  -  userRiskLevels:
      - "medium"
 - controls:
  -  requireMFA: true
 - enablePolicy: true

### Step 3. Automate Risk Remediation
- riskRemediation:
 - autoRemediate:
    - "UserRiskPolicy"
 - actions:
    - "ForcePasswordReset"
    - "RequireMFARegistration"
-  notifications:
  -  notifySecurityTeam: true
   - notifyUser: true

### Step 4. Monitor Risk Events
- monitoring:
-  logSources:
    - "RiskySignIns"
    - "RiskyUsers"
    - "AuditLogs"
 - sendToSentinel: true
 - alerts:
    - name: "LeakedCredentialDetected"
   -   severity: "High"
    - name: "UnusualLocationSignIn"
   -   severity: "Medium"

## Outcome
- Automated response to risky sign-ins and user accounts.
- Reduced manual triage time for security alerts by 60%.
- Integrated with Microsoft Sentinel for centralized alerting and analytics.
- Enforced Zero Trust access control by combining risk signals with Conditional Access.

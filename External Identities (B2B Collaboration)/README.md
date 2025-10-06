# Project: Entra ID Connect / Hybrid Identity Integration

## Overview
This project integrates on-premises Active Directory (AD) with Microsoft Entra ID (Azure AD) using Entra Connect to achieve a hybrid identity model.
The goal is to enable single sign-on (SSO), ensure synchronized user identities, and establish secure interoperability between on-prem and cloud resources while maintaining governance and compliance.

---

##  Implementation Steps

### Step 1. Prepare On-Premises Environment

```powershell
# Verify forest and domain readiness
Get-ADForest
Get-ADDomain
# Enable directory synchronization
Set-ADSyncScheduler -SyncCycleEnabled $true
```

---

### Step 2. Install & Configure Entra Connect

```yaml
entraConnectSetup:
  syncScope: "Selected OUs"
  signInMethod: "Password Hash Synchronization"
  enableSeamlessSSO: true
  enablePasswordWriteback: true
  stagingMode: false
```

---

### Step 3. Verify Synchronization

```powershell
# Check last sync status
Get-ADSyncScheduler
Start-ADSyncSyncCycle -PolicyType Delta

# Verify a sample user
Get-MsolUser -UserPrincipalName "user@domain.com"
```

---

### Step 4. Implement Hybrid SSO

```yaml
hybridSSO:
  primaryAuth: "Azure AD"
  secondaryAuth: "AD FS"
  conditionalAccess:
    requireCompliantDevice: true
    requireHybridJoined: true
```

---

### Step 5. Monitor & Audit Synchronization Health

```yaml
monitoring:
  logSources:
    - "ADSyncHealth"
    - "AuditLogs"
  sendToSentinel: true
  alerts:
    - name: "SyncFailureDetected"
      severity: "High"
    - name: "StaleObjectMismatch"
      severity: "Medium"
```

---

###  Outcome

* Enabled hybrid identity with seamless SSO for cloud and on-prem apps.
* Reduced password management overhead by enabling password writeback.
* Improved identity governance through centralized monitoring in Sentinel.
* Established foundation for Zero Trust access controls across environments.

    class A,B,G onprem;
    class C,E cloud;
    class D,F security;

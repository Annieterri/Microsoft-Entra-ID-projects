# Project: Entra ID Connect / Hybrid Identity Integration

## Overview

This project demonstrates how to integrate on-premises Active Directory (AD) with Microsoft Entra ID using Entra Connect (Azure AD Connect).
The goal is to enable Hybrid Identity, ensuring seamless sign-on, synchronized identities, and secure access to both cloud and on-prem resources — a foundational step in most enterprise Zero Trust architectures.

---

##  Implementation Steps

### Step 1. Install & Configure Entra Connect

```powershell
# Run setup in custom mode to define sync settings
Start-Process "AzureADConnect.exe"
```

* Choose Custom Installation
* Select Password Hash Synchronization (PHS) or Pass-Through Authentication (PTA)
* Enable Seamless SSO for automatic domain sign-in

---

### Step 2. Define Sync Scope

```yaml
syncConfiguration:
  syncMethod: "PasswordHashSync"
  organizationalUnits:
    include:
      - "OU=Users,DC=contoso,DC=com"
      - "OU=Groups,DC=contoso,DC=com"
  filtering:
    enable: true
    rules:
      - "ExcludeDisabledAccounts"
      - "ExcludeServiceAccounts"
```

---

### Step 3. Configure Hybrid Join

```yaml
hybridJoin:
  deviceJoinType: "HybridAzureADJoin"
  scope:
    - "Windows 10"
    - "Windows 11"
  prerequisites:
    - "Azure AD Connect v2.0+"
    - "Active Directory Schema 2012+"
```

---

### Step 4. Monitor & Troubleshoot Sync

```powershell
# Check sync status
Get-ADSyncScheduler
Start-ADSyncSyncCycle -PolicyType Delta
```

```yaml
monitoring:
  logSources:
    - "SynchronizationService"
    - "AADConnectHealth"
  sendToSentinel: true
  alerts:
    - name: "SyncFailure"
      severity: "High"
    - name: "PasswordHashMismatch"
      severity: "Medium"
```

---

## Outcome

* Seamless authentication across on-prem and cloud identities.
* Centralized identity governance through Microsoft Entra ID.
* Enabled Hybrid Azure AD Join for device compliance and management.
* Strengthened enterprise Zero Trust posture with consistent user identity across environments.

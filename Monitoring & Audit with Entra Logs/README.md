# Project: Monitoring & Audit with Entra Logs

## Overview
This project focuses on centralized monitoring and auditing of identity activities in Microsoft Entra ID.
It integrates Audit Logs, Sign-in Logs, and Provisioning Logs with Microsoft Sentinel to enhance visibility, detect anomalies, and support forensic investigations, a core part of Zero Trust operations.

---

## Implementation Steps

### Step 1. Enable and Collect Entra Logs

```powershell
# Enable diagnostic settings to send Entra logs to Sentinel
Set-AzDiagnosticSetting -Name "EntraLogsToSentinel" `
  -ResourceId "/providers/Microsoft.aadiam/diagnosticSettings" `
  -WorkspaceId "<SentinelWorkspaceID>" `
  -Enabled $true `
  -Category "AuditLogs","SignInLogs","ProvisioningLogs"
```

---

### Step 2. Configure Log Categories

```yaml
logCategories:
  - AuditLogs
  - SignInLogs
  - ProvisioningLogs
  retentionDays: 90
  destination:
    - "Log Analytics Workspace"
    - "Storage Account (Archive)"
```

---

### Step 3. Create Alert Rules in Sentinel

```yaml
alertRules:
  - name: "Multiple Failed Sign-ins"
    query: |
      SigninLogs
      | where ResultType != 0
      | summarize FailedAttempts = count() by UserPrincipalName, IPAddress
      | where FailedAttempts > 5
    severity: "High"

  - name: "Admin Role Assigned Outside Business Hours"
    query: |
      AuditLogs
      | where OperationName == "Add member to role"
      | where TimeGenerated between (ago(24h)..now())
      | where datetime_part("hour", TimeGenerated) > 18
    severity: "Medium"
```

---

### Step 4. Monitor with Workbooks & Dashboards

* Build custom dashboards in Sentinel for sign-in trends, risky users, and admin role changes.
* Enable Workbook visualizations for insights into login success/failure rates and user activity timelines.

---

### Step 5. Automate Incident Response

```yaml
automation:
  logicAppTriggers:
    - name: "HighRiskSignInDetected"
      action: "DisableUserAccount"
    - name: "ExcessiveFailedLogins"
      action: "RequireMFAReset"
```

---

## Outcome

* Achieved real-time visibility into identity activities and risk events.
* Improved threat detection by correlating Entra logs with Sentinel analytics.
* Automated remediation for high-risk sign-ins and unusual role changes.
* Strengthened audit and compliance readiness with retention and reporting policies.

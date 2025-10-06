## Tools & Resources
- **Microsoft Entra ID**
- **Microsoft 365 Admin Center**
- **Microsoft Authenticator App**
- **Azure AD Sign-in Logs**
- **Microsoft Graph API (optional)**

---

## Implementation Steps

### Step 1. Define Access Requirements
1. Identify high-risk apps (e.g., Exchange Online, SharePoint, Azure Portal).  
2. Decide on enforcement conditions (user group, location, device state).  
3. Plan for emergency access (break-glass) accounts with excluded conditions.

### Step 2. Create Conditional Access Policies
- Navigate to: `Entra ID > Protection > Conditional Access > New Policy`
- Example Policy:
  - **Users:** All users (exclude break-glass)
  - **Cloud Apps:** All cloud apps
  - **Conditions:**  
    - Sign-in risk: High  
    - Locations: Exclude trusted IPs  
  - **Grant Controls:** Require MFA

### Step 3. Enforce MFA
- Enable MFA via Security Defaults or a CA policy.
- Register MFA methods: Microsoft Authenticator, FIDO2 Key, or SMS (as fallback).
- Test sign-in flow using various scenarios:
  - Trusted vs untrusted network  
  - Compliant vs non-compliant device  
  - Risky sign-ins

### Step 4. Monitor & Review
- Review logs in:
  - `Entra ID > Monitoring > Sign-in Logs`
  - `Conditional Access > Insights and Reporting`
- Export data to Microsoft Sentinel for extended visibility.

---

## Example KQL Query (for Sentinel Integration)
```kql
SigninLogs
| where ConditionalAccessStatus == "failure"
| summarize FailedLogins = count() by UserPrincipalName, Location, ConditionalAccessStatus
| order by FailedLogins desc

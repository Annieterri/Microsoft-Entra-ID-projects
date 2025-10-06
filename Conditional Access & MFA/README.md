# Conditional Access & Multi-Factor Authentication (MFA)

## Overview
This project demonstrates the design and implementation of Conditional Access (CA) and Multi-Factor Authentication (MFA) in Microsoft Entra ID (formerly Azure AD).  
The goal is to enforce Zero Trust access control, verifying user identity, device health, and session context before granting access to corporate resources.

---

## Objectives
- Enforce MFA for all users accessing Microsoft 365 and Azure resources.
- Block legacy authentication protocols.
- Apply access controls based on user risk, location, and device compliance.
- Implement break-glass accounts and emergency access procedures.
- Test and document user experience across different access conditions.

---

##  Concepts Covered
- Conditional Access Policies
- Multi-Factor Authentication (MFA)
- User Risk & Sign-In Risk
- Named Locations & Device Compliance
- Zero Trust Principles: Verify Explicitly, Least Privilege, Assume Breach

---

## Tools & Resources
- **Microsoft Entra ID**
- **Microsoft 365 Admin Center**
- **Microsoft Authenticator App**
- **Azure AD Sign-in Logs**

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

## Architecture Diagram 

```mermaid
flowchart LR
  User([User])
  SignIn([Sign-in Request])
  Eval([Conditional Access Evaluation])
  Grant([Grant Access])
  Block([Block Access])

  User --> SignIn --> Eval
  Eval -->|Meets policies| Grant
  Eval -->|Fails policies| Block

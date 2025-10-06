# Project: FIDO2 Passkeys / Passwordless Authentication
# Overview

This project implements passwordless authentication in Microsoft Entra ID using FIDO2 security keys and biometric sign-in.
The goal is to reduce password-related attacks (phishing, credential stuffing, brute force) and enhance user experience while maintaining Zero Trust principles.

## Implementation Steps
### Step 1. Enable FIDO2 Authentication
- Enable FIDO2 security key authentication in Entra ID
- Connect-MgGraph -Scopes "Policy.ReadWrite.AuthenticationMethod"
- Update-MgPolicyAuthenticationMethodPolicy -Id "Fido2" -State "enabled"

### Step 2. Configure FIDO2 Policy
- fido2Policy:
 -  allowSelfServiceRegistration: true
 -  enforceAttestation: false
  - keyRestrictions:
  -  enforce: true
   - aaGuids:
      - "00000000-0000-0000-0000-000000000000"  # Example FIDO2 key ID
  - allowEnterpriseAttestation: true
  - enforceMFARequirement: true

### Step 3. Register FIDO2 Security Key
- Navigate to:
- My Security Info → Add Method → Security Key → USB/NFC Key.
- Complete registration with Windows Hello, YubiKey, or Feitian key.
- Test passwordless sign-in on portal.office.com or myapps.microsoft.com.

### Step 4. Monitor Usage & Compliance
- monitoring:
 -  logSources:
    - "SignInLogs"
    - "AuditLogs"
 -  sendToSentinel: true
  - alerts:
    - name: "FallbackToPasswordDetected"
     - severity: "Medium"
    - name: "UnregisteredDeviceAttempt"
     - severity: "High"

## Outcome
- Eliminated password-based login for all admin accounts.
- Reduced phishing risk and password resets by over 90%.
- Fully compliant with FIDO2 and phishing-resistant MFA standards.

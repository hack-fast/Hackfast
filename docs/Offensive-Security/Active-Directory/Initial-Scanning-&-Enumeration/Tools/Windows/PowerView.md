---
legal-banner: true
---

### **Enumerate Domain Information**

1. Retrieve detailed information about a domain user  
```powershell
Get-DomainUser -Identity mmorgan -Domain inlanefreight.local | Select-Object -Property name, samaccountname, description, memberof, whencreated, pwdlastset, lastlogontimestamp, accountexpires, admincount, userprincipalname, serviceprincipalname, useraccountcontrol
```

2. List members of the "Domain Admins" group (including nested memberships)  
```powershell
Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```

3. List trust relationships for the domain  
```powershell
Get-DomainTrustMapping
```

4. Check administrative access to a specific machine  
```powershell
Test-AdminAccess -ComputerName ACADEMY-EA-MS01
```

5. Identify users with SPN property set (useful for Kerberoasting)  
```powershell
Get-DomainUser -SPN -Properties samaccountname, ServicePrincipalName
```

6. Enumerate local groups on a target  
```powershell
Get-NetLocalGroup -ComputerName <target>
```

7. Enumerate members of the local Administrators group on a target  
```powershell
Get-NetLocalGroupMember -ComputerName <target> -GroupName "Administrators"
```

8. Enumerate shares on a target  
```powershell
Get-NetShare -ComputerName <target>
```

9. Find reachable shares on domain machines  
```powershell
Find-DomainShare
```

10. Search for files matching criteria on readable shares  
```powershell
Find-InterestingDomainShareFile
```

11. List all distributed file systems for the domain  
```powershell
Get-DomainDFSShare
```

12. Retrieve default domain or DC policy  
```powershell
Get-DomainPolicy
```  

### **Domain Information**

1. Get current domain  
```powershell
Get-NetDomain
```

2. Enumerate other domains  
```powershell
Get-NetDomain -Domain $DomainName
```

3. Get domain SID  
```powershell
Get-DomainSID
```

4. Retrieve domain policy  
```powershell
Get-DomainPolicy
(Get-DomainPolicy)."system access"
(Get-DomainPolicy)."kerberos policy"
```

5. Get domain controllers  
```powershell
Get-NetDomainController
Get-NetDomainController -Domain $DomainName
```

6. Get detailed domain info  
```powershell
Get-NetDomain -FullData
```  

### **Enumerate Domain Users**

1. Enumerate domain users  
```powershell
Get-NetUser
Get-NetUser -SamAccountName $user
Get-NetUser | select cn
Get-UserProperty
```

2. Check last password change  
```powershell
Get-UserProperty -Properties pwdlastset
```

3. Get specific attribute value  
```powershell
Find-UserField -SearchField Description -SearchTerm "wtver"
```

4. Enumerate users logged onto a machine  
```powershell
Get-NetLoggedon -ComputerName $ComputerName
```

5. Enumerate session information  
```powershell
Get-NetSession -ComputerName $ComputerName
```

6. Enumerate user locations  
```powershell
Find-DomainUserLocation -Domain $DomainName | Select-Object UserName, SessionFromName
```  

### **Enumerate Domain Computers**

1. Enumerate domain computers  
```powershell
Get-NetComputer -FullData
Get-NetComputer -Ping
```

2. Enumerate live machines  
```powershell
Get-NetComputer -Ping
```  

### **Enumerate Groups and Group Members**

1. Enumerate group members  
```powershell
Get-NetGroupMember -GroupName "$GroupName" -Domain $DomainName
```

2. Get group members  
```powershell
Get-DomainGroup -Identity $GroupName | Select-Object -ExpandProperty Member
```

3. Enumerate GPO local group membership  
```powershell
Get-DomainGPOLocalGroup | Select-Object GPODisplayName, GroupName
```

4. Enumerate all groups  
```powershell
Get-NetGroup -FullData
```  

### **Enumerate Shares**

1. Enumerate domain shares  
```powershell
Find-DomainShare
```

2. Enumerate accessible domain shares  
```powershell
Find-DomainShare -CheckShareAccess
```  

### **Enumerate Group Policies**

1. Get group policies  
```powershell
Get-NetGPO
```

2. Get group policy for a machine  
```powershell
Get-NetGPO -ComputerName $ComputerName
```

3. Get local admin group users  
```powershell
Find-GPOComputerAdmin -ComputerName $ComputerName
```  

### **Enumerate OUs**

1. Enumerate organizational units  
```powershell
Get-NetOU -FullData
Get-NetGPO -GPOname $GPO_GUID
```  

### **Enumerate ACLs**

1. Get ACLs for a user  
```powershell
Get-ObjectAcl -SamAccountName $UserName -ResolveGUIDs
```

2. Get ACLs for a specific path  
```powershell
Get-PathAcl -Path $Path
```

3. Search for interesting ACEs  
```powershell
Invoke-ACLScanner -ResolveGUIDs
```  

### **Enumeration of Data**

1. Enumerate domain trusts  
```powershell
Get-NetDomainTrust
Get-NetDomainTrust -Domain $DomainName
```

2. Enumerate forest trusts  
```powershell
Get-NetForestTrust
Get-NetForestDomain -Forest $ForestName
```

3. Get DNS zones and records  
```powershell
Get-DNSZone
Get-DNSRecord
```

4. Get all domain sites and subnets  
```powershell
Get-NetSite
Get-NetSubnet
``` 
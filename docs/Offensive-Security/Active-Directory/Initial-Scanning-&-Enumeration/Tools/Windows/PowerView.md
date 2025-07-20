### **ENUMERATE DOMAIN INFORMATION**

1. Retrieve Detailed Information about a Domain User:
   `Get-DomainUser -Identity mmorgan -Domain inlanefreight.local | Select-Object -Property name, samaccountname, description, memberof, whencreated, pwdlastset, lastlogontimestamp, accountexpires, admincount, userprincipalname, serviceprincipalname, useraccountcontrol`

2. List Members of the "Domain Admins" Group, Including Nested Group Memberships:
   `Get-DomainGroupMember -Identity "Domain Admins" -Recurse`

3. List Trust Relationships for the Domain:
   `Get-DomainTrustMapping`

4. Check Administrative Access to a Specific Machine:
   `Test-AdminAccess -ComputerName ACADEMY-EA-MS01`

5. Identify Users with the SPN Property Set (Useful for Kerberoasting):
   `Get-DomainUser -SPN -Properties samaccountname, ServicePrincipalName`

6. Enumerate Local Groups on a Specified Target:
   `Get-NetLocalGroup -ComputerName <target>`

7. Enumerate Members of the Local Administrators Group on a Specified Target:
   `Get-NetLocalGroupMember -ComputerName <target> -GroupName "Administrators"`

8. Enumerate Shares on a Specified Target:
   `Get-NetShare -ComputerName <target>`

9. Find Reachable Shares on Domain Machines:
   `Find-DomainShare`

10. Search for Files Matching Specific Criteria on Readable Shares in the Domain:
    `Find-InterestingDomainShareFile`

11. Return a List of All Distributed File Systems for the Domain:
    `Get-DomainDFSShare`

12. Return the Default Domain Policy or the Domain Controller Policy:
    `Get-DomainPolicy`

### **DOMAIN INFORMATION**

1. Get Current Domain:
   `Get-NetDomain`

2. Enumerate Other Domains:
   `Get-NetDomain -Domain $DomainName`

3. Get Domain SID:
   `Get-DomainSID`

4. Get Domain Policy:
   `Get-DomainPolicy`
   `(Get-DomainPolicy)."system access"`
   `(Get-DomainPolicy)."kerberos policy"`

5. Get Domain Controllers:
   `Get-NetDomainController`
   `Get-NetDomainController -Domain $DomainName`

6. Get Detailed Domain Info:
   `Get-NetDomain -FullData`

### **ENUMERATE DOMAIN USERS**

1. Enumerate Domain Users:
   `Get-NetUser`
   `Get-NetUser -SamAccountName $user`
   `Get-NetUser | select cn`
   `Get-UserProperty`

2. Check Last Password Change:
   `Get-UserProperty -Properties pwdlastset`

3. Get Specific Attribute Value:
   `Find-UserField -SearchField Description -SearchTerm "wtver"`

4. Enumerate Users Logged on a Machine:
   `Get-NetLoggedon -ComputerName $ComputerName`

5. Enumerate Session Information:
   `Get-NetSession -ComputerName $ComputerName`

6. Enumerate User Locations:
   `Find-DomainUserLocation -Domain $DomainName | Select-Object UserName, SessionFromName`

### **ENUMERATE DOMAIN COMPUTERS**

1. Enumerate Domain Computers:
   `Get-NetComputer -FullData`
   `Get-NetComputer -Ping`

2. Enumerate Live Machines:
   `Get-NetComputer -Ping`

### **ENUMERATE GROUPS AND GROUP MEMBERS**

1. Enumerate Group Members:
   `Get-NetGroupMember -GroupName "$GroupName" -Domain $DomainName`

2. Get Group Members:
   `Get-DomainGroup -Identity $GroupName | Select-Object -ExpandProperty Member`

3. Enumerate GPO Local Group Membership:
   `Get-DomainGPOLocalGroup | Select-Object GPODisplayName, GroupName`

4. Enumerate All Groups:
   `Get-NetGroup -FullData`

### **ENUMERATE SHARES**

1. Enumerate Domain Shares:
   `Find-DomainShare`

2. Enumerate Accessible Domain Shares:
   `Find-DomainShare -CheckShareAccess`

### **ENUMERATE GROUP POLICIES**

1. Get Group Policies:
   `Get-NetGPO`

2. Get Group Policy for Machine:
   `Get-NetGPO -ComputerName $ComputerName`

3. Get Local Admin Group Users:
   `Find-GPOComputerAdmin -ComputerName $ComputerName`

### **ENUMERATE OUS**

1. Enumerate OUs:
   `Get-NetOU -FullData`
   `Get-NetGPO -GPOname $GPO_GUID`

### **ENUMERATE ACLS**

1. Get ACLs for a User:
   `Get-ObjectAcl -SamAccountName $UserName -ResolveGUIDs`

2. Get ACLs for a Specific Path:
   `Get-PathAcl -Path $Path`

3. Search for Interesting ACEs:
   `Invoke-ACLScanner -ResolveGUIDs`

### **ENUMERATION OF DATA**

1. Enumerate Domain Trusts:
   `Get-NetDomainTrust`
   `Get-NetDomainTrust -Domain $DomainName`

2. Enumerate Forest Trusts:
   `Get-NetForestTrust`
   `Get-NetForestDomain -Forest $ForestName`

3. Get DNS Zones and Records:
   `Get-DNSZone`
   `Get-DNSRecord`

4. Get All Domain Sites and Subnets:
   `Get-NetSite`
   `Get-NetSubnet`
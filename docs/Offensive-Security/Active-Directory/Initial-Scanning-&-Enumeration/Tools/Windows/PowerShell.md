---
legal-banner: true
---

### **Introduction**

PowerShell, released by Microsoft in 2006, is a powerful upgrade to Command Prompt. It includes access to cmdlets (pronounced *command-lets*), which are .NET classes designed to perform specific functions.  
This cheat sheet focuses on using PowerShell for Active Directory (AD) enumeration, leveraging cmdlets included with the AD-RSAT (Active Directory Remote Server Administration Tools) package.

### **Enumerating AD Users**

1. Retrieve AD user details with all properties  
```powershell
Get-ADUser -Identity gordon.stevens -Server za.example.com -Properties *
```

2. Use filters to narrow down user search  
```powershell
Get-ADUser -Filter 'Name -like "*Roland"' -Server za.example.com | Format-Table Name,SamAccountName -A
```

3. List all users in the domain  
```powershell
Get-ADUser -Filter * -Server za.example.com | Format-Table Name,SamAccountName -AutoSize
```  

### **Enumerating AD Groups**

1. Retrieve AD group details  
```powershell
Get-ADGroup -Identity Administrators -Server za.example.com
```

2. Enumerate group membership  
```powershell
Get-ADGroupMember -Identity Administrators -Server za.example.com
```

3. List all groups in the domain  
```powershell
Get-ADGroup -Filter * -Server za.example.com | Format-Table Name,GroupScope -AutoSize
```  

### **Enumerating AD Computers**

1. Retrieve AD computer details with all properties  
```powershell
Get-ADComputer -Identity "DC01" -Server za.example.com -Properties *
```

2. Use filters to narrow down computers by OS  
```powershell
Get-ADComputer -Filter 'OperatingSystem -like "*Server*"' -Server za.example.com | Format-Table Name,OperatingSystem -A
```

3. List all computers in the domain  
```powershell
Get-ADComputer -Filter * -Server za.example.com | Format-Table Name,OperatingSystem -AutoSize
```  

### **Enumerating AD Objects**

1. Search for objects changed after a specific date  
```powershell
$ChangeDate = New-Object DateTime(2024, 05, 25, 12, 00, 00)
Get-ADObject -Filter 'whenChanged -gt $ChangeDate' -IncludeDeletedObjects -Server za.example.com
```

2. List all AD objects  
```powershell
Get-ADObject -Filter * -Server za.example.com | Format-Table Name,ObjectClass -AutoSize
```  
### **Enumerating Domains**

1. Retrieve detailed domain information  
```powershell
Get-ADDomain -Server za.example.com
```  
2. Enumerate organizational units (OUs)  
```powershell
Get-ADOrganizationalUnit -Filter * -Server za.example.com | Format-Table Name,DistinguishedName -A
```  

### **INTRODUCTION**

PowerShell, released by Microsoft in 2006, is a powerful upgrade to Command Prompt. It includes access to cmdlets (pronounced command-lets), which are .NET classes designed to perform specific functions. This cheat sheet focuses on using PowerShell for Active Directory (AD) enumeration, leveraging cmdlets included with the AD-RSAT (Active Directory Remote Server Administration Tools) package.

### **ENUMERATING AD USERS**

1. Enumerates AD users and retrieves properties of AD user objects.  
   `Get-ADUser -Identity gordon.stevens -Server za.example.com -Properties *`

2. Use filters to narrow down the user search.  
   `Get-ADUser -Filter 'Name -like "*Roland"' -Server za.example.com | Format-Table Name,SamAccountName -A`

3. Lists all users in the AD.  
   `Get-ADUser -Filter * -Server za.example.com | Format-Table Name,SamAccountName -AutoSize`

### **ENUMERATING AD GROUPS**

1. Enumerates AD groups.  
   `Get-ADGroup -Identity Administrators -Server za.example.com`

2. Enumerate group membership using the Get-ADGroupMember cmdlet.  
   `Get-ADGroupMember -Identity Administrators -Server za.example.com`

3. Lists all groups in the AD.  
   `Get-ADGroup -Filter * -Server za.example.com | Format-Table Name,GroupScope -AutoSize`

### **ENUMERATING AD COMPUTERS**

1. Enumerates AD computers.  
  `Get-ADComputer -Identity "DC01" -Server za.example.com -Properties *`

2. Use filters to narrow down the computer search.  
   `Get-ADComputer -Filter 'OperatingSystem -like "*Server*"' -Server za.example.com | Format-Table Name,OperatingSystem -A`

3. Lists all computers in the AD.  
   `Get-ADComputer -Filter * -Server za.example.com | Format-Table Name,OperatingSystem -AutoSize`

### **ENUMERATING AD OBJECTS**

1. Performs generic searches for any AD objects.  
     `$ChangeDate = New-Object DateTime(2024, 05, 25, 12, 00, 00)`
     `Get-ADObject -Filter 'whenChanged -gt $ChangeDate' -IncludeDeletedObjects -Server za.example.com`

2. Lists all AD objects.  
   `Get-ADObject -Filter * -Server za.example.com | Format-Table Name,ObjectClass -AutoSize`

### **ENUMERATING DOMAINS**

1. Retrieves additional information about the specific domain.  
	`Get-ADDomain -Server za.example.com`
2. Retrieves information about organizational units.  
	`Get-ADOrganizationalUnit -Filter * -Server za.example.com | Format-Table Name,DistinguishedName -A`
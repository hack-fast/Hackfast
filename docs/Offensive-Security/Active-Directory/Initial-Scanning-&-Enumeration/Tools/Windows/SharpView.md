---
legal-banner: true
---

### **ENUMERATING AD USERS**

1. Enumerates AD users.  
   `SharpView.exe Get-DomainUser -Identity gordon.stevens -Domain [DOMAIN]`

2. Use filters to narrow down the user search.  
   `SharpView.exe Get-DomainUser -Filter "samaccountname -like '*[USERNAME]*'" -Domain [DOMAIN]`

3. List all users in the AD.  
   `SharpView.exe Get-DomainUser -Domain [DOMAIN]`

### **ENUMERATING AD GROUPS**

1. Enumerates AD groups.  
   `SharpView.exe Get-DomainGroup -Identity Administrators -Domain [DOMAIN]`

2. Enumerate group membership.  
   `SharpView.exe Get-DomainGroupMember -Identity Administrators -Domain [DOMAIN]`

3. List all groups in the AD.  
   `SharpView.exe Get-DomainGroup -Domain [DOMAIN]`

### **ENUMERATING AD COMPUTERS**

1. Enumerates AD computers.  
   `SharpView.exe Get-DomainComputer -Identity "DC01" -Domain [DOMAIN]`

2. Use filters to narrow down the computer search.  
   `SharpView.exe Get-DomainComputer -Filter "operatingsystem -like '*Server*'" -Domain [DOMAIN]`

3. List all computers in the AD.  
   `SharpView.exe Get-DomainComputer -Domain [DOMAIN]`

### **ENUMERATING AD OBJECTS**

1. Performs generic searches for any AD objects.  
   `SharpView.exe Get-DomainObject -Filter "whenchanged -gt 2024-02-28T12:00:00" -Domain [DOMAIN]`

2. List all AD objects.  
   `SharpView.exe Get-DomainObject -Domain [DOMAIN]`

### **ENUMERATING DOMAINS**

1. Retrieve additional information about the specific domain.  
   `SharpView.exe Get-Domain -Domain [DOMAIN]`

2. Retrieve information about organizational units.  
   `SharpView.exe Get-DomainOU -Domain [DOMAIN]`

### **ENUMERATING GROUP POLICY OBJECTS (GPOS)**

1. Retrieve information about Group Policy Objects (GPOs) within the domain.  
   `SharpView.exe Get-DomainGPO -Domain [DOMAIN]`

### **ENUMERATING TRUSTS**

1. Retrieve information about domain trusts.  
   `SharpView.exe Get-DomainTrust -Domain [DOMAIN]`

### **ENUMERATING DOMAIN CONTROLLERS**

1. Retrieve information about domain controllers.  
   `SharpView.exe Get-DomainController -Domain [DOMAIN]`
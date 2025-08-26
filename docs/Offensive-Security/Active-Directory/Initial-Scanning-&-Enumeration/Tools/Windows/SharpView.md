---
legal-banner: true
---

### **Enumerating AD Users**

1. Enumerate AD users  
```bash
SharpView.exe Get-DomainUser -Identity gordon.stevens -Domain [DOMAIN]
```

2. Use filters to narrow user search  
```bash
SharpView.exe Get-DomainUser -Filter "samaccountname -like '*[USERNAME]*'" -Domain [DOMAIN]
```

3. List all AD users  
```bash
SharpView.exe Get-DomainUser -Domain [DOMAIN]
```  

### **Enumerating AD Groups**

1. Enumerate AD groups  
```bash
SharpView.exe Get-DomainGroup -Identity Administrators -Domain [DOMAIN]
```

2. Enumerate group membership  
```bash
SharpView.exe Get-DomainGroupMember -Identity Administrators -Domain [DOMAIN]
```

3. List all AD groups  
```bash
SharpView.exe Get-DomainGroup -Domain [DOMAIN]
```  

### **Enumerating AD Computers**

1. Enumerate AD computers  
```bash
SharpView.exe Get-DomainComputer -Identity "DC01" -Domain [DOMAIN]
```

2. Use filters to narrow computer search  
```bash
SharpView.exe Get-DomainComputer -Filter "operatingsystem -like '*Server*'" -Domain [DOMAIN]
```

3. List all AD computers  
```bash
SharpView.exe Get-DomainComputer -Domain [DOMAIN]
```  

### **Enumerating AD Objects**

1. Perform generic AD object searches  
```bash
SharpView.exe Get-DomainObject -Filter "whenchanged -gt 2024-02-28T12:00:00" -Domain [DOMAIN]
```

2. List all AD objects  
```bash
SharpView.exe Get-DomainObject -Domain [DOMAIN]
```  

### **Enumerating Domains**

1. Retrieve detailed domain information  
```bash
SharpView.exe Get-Domain -Domain [DOMAIN]
```

2. Enumerate organizational units (OUs)  
```bash
SharpView.exe Get-DomainOU -Domain [DOMAIN]
```  

### **Enumerating Group Policy Objects (GPOs)**

1. Retrieve Group Policy Objects in the domain  
```bash
SharpView.exe Get-DomainGPO -Domain [DOMAIN]
```  

### **Enumerating Trusts**

1. Retrieve domain trust information  
```bash
SharpView.exe Get-DomainTrust -Domain [DOMAIN]
```  

### **Enumerating Domain Controllers**

1. Retrieve domain controller information  
```bash
SharpView.exe Get-DomainController -Domain [DOMAIN]
```  
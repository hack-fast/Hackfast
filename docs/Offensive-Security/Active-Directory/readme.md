---
legal-banner: true
---

### **Introduction**

Active Directory (AD) is a Microsoft service for Windows domain networks.It provides essential services for managing network resources, user accounts, authentication, and application-specific data.  

Core components include:

- **AD DS (Domain Services)**  
- **AD LDS (Lightweight Directory Services)**  
- **AD CS (Certificate Services)**  
- **AD FS (Federation Services)**  
- **AD RMS (Rights Management Services)**  


### **Active Directory Structure**

1. **Domains** – Collections of objects (users, groups, devices) sharing a common database  
2. **Trees** – Groups of domains connected in a hierarchical structure  
3. **Forests** – The top layer, composed of multiple trees interconnected by trust relationships  

### **Key Concepts in Active Directory**

1. **Directory** – Stores all information about AD objects  
2. **Object** – Represents entities such as users, groups, or shared folders  
3. **Domain** – A container for directory objects; multiple domains can coexist within a forest  
4. **Tree** – A grouping of domains that share a common root domain  
5. **Forest** – The highest level in AD, made up of multiple trees with trust relationships  

### **Services Provided by Active Directory Domain Services**

1. **Domain Services** – Centralized data storage, authentication, and search functionalities  
2. **Certificate Services** – Creation, distribution, and management of digital certificates  
3. **Lightweight Directory Services** – Directory-enabled application support via LDAP  
4. **Federation Services** – Single sign-on (SSO) across multiple web applications  
5. **Rights Management** – Protects copyrighted content by restricting unauthorized distribution  
6. **DNS Service** – Critical for resolving domain names  

### **Common Active Directory Vulnerabilities**

1. **Weak Password Policies** – Simple, guessable passwords lacking complexity or length  
2. **Unpatched Software and Systems** – Outdated systems exploited via known vulnerabilities  
3. **Excessive Privileges** – Over-privileged accounts increase risk if compromised  
4. **Inadequate Monitoring and Logging** – Fails to detect suspicious activities or breaches  
5. **Lack of Network Segmentation** – Allows attackers to move laterally with ease  
6. **Phishing & Social Engineering** – Tricks users into disclosing credentials or installing malware  
7. **Misconfigured Service Accounts** – Often over-privileged and high-value targets  
8. **Insecure LDAP Bindings** – Unencrypted LDAP traffic can expose credentials to interception  
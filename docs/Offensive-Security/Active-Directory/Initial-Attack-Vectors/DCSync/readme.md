---
legal-banner: true
---

### **Overview**

The DCSync attack simulates the behavior of a Domain Controller and requests other Domain Controllers to replicate information using the **Directory Replication Service Remote Protocol (MS-DRSR)**.  
This is a legitimate and necessary function of Active Directory, which means it cannot be disabled.  

- **Operational Mechanism:**  
  The DCSync attack works by impersonating a Domain Controller. It requests replication data from other Domain Controllers through MS-DRSR.  
  Since MS-DRSR is a core function of Active Directory, it cannot be turned off or restricted.  

- **Required Privileges:**  
  Only certain high-privilege groups have the permissions required to perform a DCSync attack. These include:  
  - Domain Admins  
  - Enterprise Admins  
  - Administrators  
  - Domain Controllers  

- **Retrieving Cleartext Passwords:**  
  If account passwords are stored using reversible encryption, **Mimikatz** can be used to extract these passwords in plaintext. 
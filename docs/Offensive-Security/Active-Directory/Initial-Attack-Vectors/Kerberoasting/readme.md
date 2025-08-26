---
legal-banner: true
---

### **Introduction**

Kerberos is a network authentication protocol that uses secret-key cryptography to provide secure authentication for client/server applications.  
It employs a trusted third party, the **Key Distribution Center (KDC)**, to issue tickets that validate user identities, protecting against eavesdropping and replay attacks.  
Kerberos is commonly integrated into secure operating environments, such as Microsoft Windows, to safeguard user data and services.  

### **Key Components**

| Component | Description |
| --- | --- |
| **KDC (Key Distribution Center)** | Central authority for issuing session keys and tickets. |
| **TGT (Ticket Granting Ticket)** | Issued by the KDC to request service tickets from the TGS. |
| **TGS (Ticket Granting Service)** | A KDC component that issues service tickets for accessing services. |
| **Service Ticket** | Grants user access to network services. |

### **Common Kerberos Commands**

Key commands for managing Kerberos tickets.  

| Command | Description |
| --- | --- |
| `kinit [username]` | Obtain a Ticket-Granting Ticket (TGT) for the user and save it in the cache. |
| `klist` | Display all cached Kerberos tickets. |
| `ktutil` | Manage Kerberos keytab files. |
| `kdestroy` | Remove all cached Kerberos tickets for the user. |
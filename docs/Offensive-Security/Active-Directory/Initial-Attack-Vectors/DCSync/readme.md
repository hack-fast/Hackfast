### **OVERVIEW**

The DCSync attack simulates the behavior of a Domain Controller and requests other Domain Controllers to replicate information using the Directory Replication Service Remote Protocol (MS-DRSR). This is a valid and necessary function of Active Directory, hence it cannot be turned off or disabled.

- Operational Mechanism: The DCSync attack operates by mimicking a Domain Controller. It requests replication data from other Domain Controllers through the Directory Replication Service Remote Protocol (MS-DRSR). Since MS-DRSR is a core function of Active Directory, it cannot be deactivated or disabled.

- Required Privileges: Typically, only specific high-privilege groups have the permissions needed to carry out a DCSync attack. These groups include:
   - Domain Admins
   - Enterprise Admins
   - Administrators
   - Domain Controllers
- Retrieving Clear Text Passwords: If account passwords are stored using reversible encryption, Mimikatz can be Used to extract these passwords in plain text. 
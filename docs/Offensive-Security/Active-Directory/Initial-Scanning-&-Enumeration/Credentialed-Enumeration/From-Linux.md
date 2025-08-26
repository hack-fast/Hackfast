---
legal-banner: true
---

### **Using CrackMapExec (CME)**

1. Enumerate domain users:  
   ```bash
   sudo crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p [PASSWORD] --users
   ```

2. Enumerate domain groups:  
   ```bash
   sudo crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p [PASSWORD] --groups
   ```

3. Enumerate logged-on users:  
   ```bash
   sudo crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p [PASSWORD] --loggedon-users
   ```

4. Enumerate shares:  
   ```bash
   sudo crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p [PASSWORD] --shares
   ```

5. Enumerate shares recursively (Spider):  
   ```bash
   sudo crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p [PASSWORD] -M spider_plus --share
   ```

### **Using SMBMap**

1. Check access:  
   ```bash
   smbmap -u [USERNAME] -p [PASSWORD] -d [DOMAIN] -H [IP-ADDRESS]
   ```

2. Recursively list all directories:  
   ```bash
   smbmap -u [USERNAME] -p [PASSWORD] -d [DOMAIN] -H [IP-ADDRESS] -R 'Department Shares' --dir-only
   ```

### **Using Windapsearch**

1. Enumerate Domain Admins:  
   ```bash
   python3 windapsearch.py --dc-ip [IP_ADDRESS] -u [USERNAME]@[DOMAIN] -p [PASSWORD] --da
   ```

2. Enumerate privileged users:  
   ```bash
   python3 windapsearch.py --dc-ip [IP_ADDRESS] -u [USERNAME]@[DOMAIN] -p [PASSWORD] -PU
   ```

### **Using BloodHound**

1. Execute BloodHound.py:  
   ```bash
   sudo bloodhound-python -u '[USERNAME]' -p '[PASSWORD]' -ns [IP-ADDRESS] -d [DOMAIN] -c all
   ```

2. Start Neo4j service:  
   ```bash
   sudo neo4j start
   ```

3. Start BloodHound GUI:  
   ```bash
   bloodhound
   ```

Example:  
   ```bash
   /opt/Windows/BloodHound_Python/bloodhound.py -d hutch.offsec -u fmcsorley -p CrabSharkJellyfish192 -c all -ns 192.168.219.122
   ```

- Extract all user emails from a BloodHound user JSON file:  
   ```bash
   cat 202020020_user.json | jq '.data[].Properties.name' | cut -d '"' -f 2 > useremail.txt
   ```

- Extract usernames (without domain part) from emails:  
   ```bash
   cat useremail.txt | cut -d '@' -f 1 > users.list
   ```

### **Using Impacket PsExec**

1. Remote command execution with PsExec:  
   ```bash
   psexec.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]
   ```

2. Remote command execution with WMIExec:  
   ```bash
   wmiexec.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]
   ```

---
legal-banner: true
---

### **USING CRACKMAPEXEC (CME)**

1.  Enumerate Domain Users:  
    `sudo crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p [PASSWORD] --users`
    
2.  Enumerate Domain Groups:  
    `sudo crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p [PASSWORD] --groups`
    
3.  Enumerate Logged On Users:  
    `sudo crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p [PASSWORD] --loggedon-users`
    
4.  Enumerate Shares:  
    `sudo crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p [PASSWORD] --shares`
    
5.  Enumerate Shares (Spider):  
    `sudo crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p [PASSWORD] -M spider_plus --share`
    

### **USING SMBMAP**

1.  Check Access:  
    `smbmap -u [USERNAME] -p [PASSWORD] -d [DOMAIN] -H [IP-ADDRESS]`
    
2.  Recursive List of All Directories:  
    `smbmap -u [USERNAME] -p [PASSWORD] -d [DOMAIN] -H [IP-ADDRESS] -R 'Department Shares' --dir-only`
    

### **USING WINDAPSEARCH**

1.  Enumerate Domain Admins:  
    `python3 windapsearch.py --dc-ip [IP_ADDRESS] -u [USERNAME]@[DOMAIN] -p [PASSWORD] --da`
    
2.  Enumerate Privileged Users:  
    `python3 windapsearch.py --dc-ip [IP_ADDRESS] -u [USERNAME]@[DOMAIN] -p [PASSWORD] -PU`
    

### **USING BLOODHOUND**

1.  Execute BloodHound.py:  
    `sudo bloodhound-python -u '[USERNAME]' -p '[PASSWORD]' -ns [IP-ADDRESS] -d [DOMAIN] -c all`
    
2.  Start Neo4j service:  
    `sudo neo4j start`
    
3.  Start BloodHound GUI:  
    `bloodhound`
    

`/opt/Windows/BloodHound_Python/bloodhound.py -d hutch.offsec -u fmcsorley -p CrabSharkJellyfish192 -c all -ns 192.168.219.122`

grap all useremail from usernmae bloodhound file :  
`cat 202020020_user.json | jq '.data[].Properties.name' | cut -d '"' -f 2 > useremail.txt`  
to extract all user from user  
`cat useremail.txt | cut -d '@' -f 1 > users.list`

### **USING IMPACKET PSEXEC**

1.  psexec.py:  
    `psexec.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]`
    
2.  wmiexec.py:  
    `wmiexec.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP-ADDRESS]`
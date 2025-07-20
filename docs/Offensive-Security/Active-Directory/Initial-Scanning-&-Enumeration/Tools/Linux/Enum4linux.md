### **ENUM4LINUX**

Enum4linux is a Linux-based command-line tool used for gathering information from Windows machines and Samba servers. It is a popular tool among penetration testers and security professionals for performing SMB (Server Message Block) enumeration.

### **BASIC USAGE**
    
1.  Basic enumeration of a target  
    `enum4linux [IP-ADDRESS]`
2.  Full basic enumeration (all options)  
    `enum4linux -a [IP-ADDRESS]`

### **USER AND GROUP ENUMERATION**
    
1.  Enumerate users  
    `enum4linux -U [IP-ADDRESS]`      
2.  Enumerate users with credentials  
   `enum4linux -u [USERNAME] -p [PASSWORD] -U [IP-ADDRESS]` 
3.  Enumerate groups  
   `enum4linux -G [IP_ADDRESS]`  
4. Enumerate groups with credentials  
   `enum4linux -u [USERNAME] -p [PASSWORD] -G [IP-ADDRESS]`
        
### **RID CYCLING**

1.  Perform RID cycling to enumerate users  
    `enum4linux -r [IP-ADDRESS]`
2.  Perform RID cycling with credentials  
    `enum4linux -u [USERNAME] -p [PASSWORD] -r [IP-ADDRESS]`

### **SHARE ENUMERATION**
    
1.  List shares on the target  
   `enum4linux -S [IP-ADDRESS]`
2.  List shares with credentials  
   `enum4linux -u [USERNAME] -p [PASSWORD] -S [IP-ADDRESS]`
3.  List publicly accessible shares  
    `enum4linux -s [IP-ADDRESS]`  
4. List publicly accessible shares with credentials  
    `enum4linux -u [USERNAME] -p [PASSWORD] -s [IP-ADDRESS]`
	
### **PASSWORD POLICY ENUMERATION**
    
1.  Retrieve password policy  
    `enum4linux -P [IP-ADDRESS]`
2.  Retrieve password policy with credentials  
    `enum4linux -u [USERNAME] -p [PASSWORD] -P [IP-ADDRESS]`

### **DOMAIN AND WORKGROUP INFORMATION**
    
1.  Get domain and workgroup information  
    `enum4linux -n [IP-ADDRESS]`
2.  Get domain and workgroup information with credentials  
    `enum4linux -u [USERNAME] -p [PASSWORD] -n [IP-ADDRESS]`

### **OS INFORMATION**
    
1.  Get OS information  
     `enum4linux -o [IP-ADDRESS]`
2.  Get OS information with credentials  
     `enum4linux -u [USERNAME] -p [PASSWORD] -o [IP-ADDRESS]`

### **DETAILED REPORT**
1.  Perform all basic checks  
     `enum4linux -a [IP-ADDRESS]`
2.  Perform all basic checks with credentials  
     `enum4linux -u [USERNAME] -p [PASSWORD] -a [IP-ADDRESS]`

### **MISCELLANEOUS**
1.  List hostnames  
    `enum4linux -i [IP-ADDRESS]`
2.  List hostnames with credentials  
   `enum4linux -u [USERNAME] -p [PASSWORD] -i [IP-ADDRESS]`
3.  Check for null sessions  
    `enum4linux -N [IP-ADDRESS]`
4.  Check for null sessions with credentials  
    `enum4linux -u [USERNAME] -p [PASSWORD] -N [IP-ADDRESS]`
5.  List all available options  
    `enum4linux -A [IP-ADDRESS]`
6.  List all available options with credentials  
    `enum4linux -u [USERNAME] -p [PASSWORD] -A [IP-ADDRESS]`
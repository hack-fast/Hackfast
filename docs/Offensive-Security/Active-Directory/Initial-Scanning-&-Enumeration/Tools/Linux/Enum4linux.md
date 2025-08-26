---
legal-banner: true
---

### **Enum4Linux**

Enum4Linux is a Linux-based command-line tool used for gathering information from Windows machines and Samba servers.  
It is commonly used by penetration testers and security professionals to perform SMB (Server Message Block) enumeration.  

### **Basic Usage**

1. Run basic enumeration against a target:  
   ```bash
   enum4linux [IP-ADDRESS]
   ```

2. Run full basic enumeration (all options):  
   ```bash
   enum4linux -a [IP-ADDRESS]
   ```

### **User and Group Enumeration**

1. Enumerate users:  
   ```bash
   enum4linux -U [IP-ADDRESS]
   ```

2. Enumerate users with credentials:  
   ```bash
   enum4linux -u [USERNAME] -p [PASSWORD] -U [IP-ADDRESS]
   ```

3. Enumerate groups:  
   ```bash
   enum4linux -G [IP-ADDRESS]
   ```

4. Enumerate groups with credentials:  
   ```bash
   enum4linux -u [USERNAME] -p [PASSWORD] -G [IP-ADDRESS]
   ```
### **RID Cycling**

1. Perform RID cycling to enumerate users:  
   ```bash
   enum4linux -r [IP-ADDRESS]
   ```

2. Perform RID cycling with credentials:  
   ```bash
   enum4linux -u [USERNAME] -p [PASSWORD] -r [IP-ADDRESS]
   ```

### **Share Enumeration**

1. List shares on the target:  
   ```bash
   enum4linux -S [IP-ADDRESS]
   ```

2. List shares with credentials:  
   ```bash
   enum4linux -u [USERNAME] -p [PASSWORD] -S [IP-ADDRESS]
   ```

3. List publicly accessible shares:  
   ```bash
   enum4linux -s [IP-ADDRESS]
   ```

4. List publicly accessible shares with credentials:  
   ```bash
   enum4linux -u [USERNAME] -p [PASSWORD] -s [IP-ADDRESS]
   ```

### **Password Policy Enumeration**

1. Retrieve password policy:  
   ```bash
   enum4linux -P [IP-ADDRESS]
   ```

2. Retrieve password policy with credentials:  
   ```bash
   enum4linux -u [USERNAME] -p [PASSWORD] -P [IP-ADDRESS]
   ```

### **Domain and Workgroup Information**

1. Get domain and workgroup information:  
   ```bash
   enum4linux -n [IP-ADDRESS]
   ```

2. Get domain and workgroup information with credentials:  
   ```bash
   enum4linux -u [USERNAME] -p [PASSWORD] -n [IP-ADDRESS]
   ```

### **OS Information**

1. Get OS information:  
   ```bash
   enum4linux -o [IP-ADDRESS]
   ```

2. Get OS information with credentials:  
   ```bash
   enum4linux -u [USERNAME] -p [PASSWORD] -o [IP-ADDRESS]
   ```

### **Detailed Report**

1. Perform all basic checks:  
   ```bash
   enum4linux -a [IP-ADDRESS]
   ```

2. Perform all basic checks with credentials:  
   ```bash
   enum4linux -u [USERNAME] -p [PASSWORD] -a [IP-ADDRESS]
   ```

### **Miscellaneous**

1. List hostnames:  
   ```bash
   enum4linux -i [IP-ADDRESS]
   ```

2. List hostnames with credentials:  
   ```bash
   enum4linux -u [USERNAME] -p [PASSWORD] -i [IP-ADDRESS]
   ```

3. Check for null sessions:  
   ```bash
   enum4linux -N [IP-ADDRESS]
   ```

4. Check for null sessions with credentials:  
   ```bash
   enum4linux -u [USERNAME] -p [PASSWORD] -N [IP-ADDRESS]
   ```

5. List all available options:  
   ```bash
   enum4linux -A [IP-ADDRESS]
   ```

6. List all available options with credentials:  
   ```bash
   enum4linux -u [USERNAME] -p [PASSWORD] -A [IP-ADDRESS]
   ```

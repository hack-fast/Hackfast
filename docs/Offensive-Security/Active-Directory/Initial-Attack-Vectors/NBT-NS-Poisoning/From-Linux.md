---
legal-banner: true
---

### **STEP 1. STARTING RESPONDER**

To begin, ensure that Responder is installed on your system. If it's not already installed, follow these steps to install it:

1.  Update your package list:  
    `sudo apt update`
2.  Install Responder:  
    `sudo apt install responder`
3.  Run Responder with default settings: Replace ens224 with the name of your network interface.  
    `sudo responder -I ens224`

### **STEP 2. RESPONDER OPERATIONAL OVERVIEW**

Responder actively listens for LLMNR, NBT-NS, and MDNS requests on your network and responds to them. When it captures a hash, it saves it in the /usr/share/responder/logs directory. You can monitor the activity either directly in the terminal or by checking the log files.

1.  Terminal Output: Responder displays captured hashes and other relevant information directly in the terminal.
2.  Log Files: Captured hashes are saved in the /usr/share/responder/logs directory.
3.  Captured Data: Typically, Responder captures NTLMv2 hashes and logs them, providing real-time updates in the terminal.

### **STEP 3. CRACKING NTLMV2 HASHES**

Once you have captured the hashes, you can use tools like Hashcat or John the Ripper to crack them. The following commands demonstrate how to crack NTLMv2 hashes using the rockyou.txt wordlist.

1.  Using Hashcat  
    `hashcat -m 5600 captured_ntlmv2 /usr/share/wordlists/rockyou.txt`
2.  Using John the Ripper  
    `john --format=netntlmv2 --wordlist=/usr/share/wordlists/rockyou.txt captured_ntlmv2.txt`
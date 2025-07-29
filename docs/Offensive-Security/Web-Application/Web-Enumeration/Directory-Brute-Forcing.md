### **Wordlist Types**

1. General-Purpose: Contains a broad range of common subdomain names (e.g., dev, staging, blog, mail, admin, test). Useful when the target's naming conventions are unknown.
2. Targeted: Focused on specific industries, technologies, or naming patterns relevant to the target. More efficient and reduces false positives.
3. Custom: Created based on specific keywords, patterns, or intelligence gathered from other sources.

### **Using Gobuster**

1. Basic usage with a predefined wordlist:  
    `gobuster dir -u [TARGET-URL] -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x txt,php,html -t 30`    
2. Using a big wordlist:  
    `gobuster dir -u [TARGET-URL] -w /usr/share/wordlists/dirb/big.txt`
    

### **Using Feroxbuster**

1.  Default usage with a common wordlist:  
    `feroxbuster -u [TARGET-URL] -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt -x php,txt,html`
    
2.  Using a big wordlist:  
    `feroxbuster -u [TARGET-URL] -w /usr/share/wordlists/dirb/big.txt -x php,txt,html`
    
3.  Lowercase wordlist for Windows:  
    `feroxbuster -u [TARGET-URL] --no-recursion -k -w /opt/SecLists/Discovery/Web-Content/raft-medium-directories-lowercase.txt`
    
4.  For ASP.NET/IIS servers:  
    `feroxbuster -u [TARGET-URL] -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories-lowercase.txt -x aspx`
    

### **Using Dirsearch**

1.  Basic usage:  
    `dirsearch -u [TARGET-URL] -w /usr/share/dirb/wordlists/common.txt`
    
2.  With multiple extensions:  
    `dirsearch -u [TARGET-URL] -e sh,txt,htm,php,cgi,html,pl,bak,old`
    
3.  Custom wordlist:  
    `dirsearch -u [TARGET-URL] -e sh,txt,htm,php,cgi,html,pl,bak,old -w path/to/wordlist`
    

### **Using FFUF**

1.  Using a common wordlist:  
    `ffuf -u [TARGET-URL]/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt`

### **Using Dirb**

1.  Basic usage:  
    `dirb [TARGET-URL] /path/to/wordlist`
    
2.  With multiple extensions:  
    `dirb [TARGET-URL] /usr/share/wordlists/dirb/big.txt -X .sh,.txt,.htm,.php,.cgi,.html,.pl,.bak,.old`
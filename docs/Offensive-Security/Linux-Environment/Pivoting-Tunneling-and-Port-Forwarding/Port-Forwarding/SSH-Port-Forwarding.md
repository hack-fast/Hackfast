### **PORT FORWARDING WITH SSH AND SOCKS TUNNELING**


### **EXECUTING LOCAL PORT FORWARD**

1.  Forward local port 1234 to MySQL port 3306 on the Ubuntu server:  
    `ssh -L 1234:localhost:3306 ubuntu@[TARGET-IP]`
    
2.  Forwarding Multiple Ports  
    `ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@10.129.202.64`
    
3.  Confirming Port Forward with Netstat  
    `netstat -antp | grep 1234`
    
4.  Confirming Port Forward with Nmap  
    `nmap -v -sV -p1234 localhost`
    

### **DYNAMIC PORT FORWARDING WITH SSH AND SOCKS TUNNELING**

1.  Start SSH client to enable dynamic port forwarding:  
    `ssh -D 9050 ubuntu@10.129.202.64`
    
2.  Modify /etc/proxychains.conf to include:  
    `socks4 127.0.0.1 9050`
    
3.  Using Nmap with Proxychains  
    `proxychains nmap -v -sn 172.16.5.1-200`
    
4.  Enumerating Windows Target through Proxychains  
    `proxychains nmap -v -Pn -sT 172.16.5.19`
    
5.  Using xfreerdp with Proxychains  
    `proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123`
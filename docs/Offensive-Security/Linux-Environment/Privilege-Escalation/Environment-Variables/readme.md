---
legal-banner: true
---

### **Introduction**

Environment variables are used to store configuration settings and system information, such as paths to executable files, user preferences, and system directories. Common examples include `PATH`, `HOME`, and `LD_LIBRARY_PATH`.

### **Manipulating PATH**

| Aspect | Description |
| --- | --- |
| Definition | The PATH variable specifies the directories the shell searches for executable files. |
| Attack Method | By modifying PATH, an attacker can prioritize malicious binaries over legitimate ones. |
| Example | An attacker can create a malicious script named `ls` and place it in a directory they control. If they prepend this directory to the PATH, the system will execute their malicious script instead of the genuine `ls` command. |

### **Exploiting LD_PRELOAD**

| Aspect | Description |
| --- | --- |
| Definition | LD_PRELOAD is used to load shared libraries before any others when a program is run. |
| Attack Method | An attacker can set LD_PRELOAD to a malicious library to execute arbitrary code with the privileges of the target program. |
| Risk | This is particularly dangerous for setuid binaries (programs that run with elevated privileges). |

### **Exploiting LD_LIBRARY_PATH**

| Aspect | Description |
| --- | --- |
| Definition | LD_LIBRARY_PATH specifies directories to search for shared libraries. |
| Attack Method | By adding a directory with malicious libraries to this variable, an attacker can manipulate which libraries are loaded, potentially executing malicious code. |
| Risk | This method can compromise the integrity and security of applications, especially those running with elevated privileges, leading to unauthorized actions and data breaches. |
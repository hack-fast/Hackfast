---
legal-banner: true
---

### **Introduction to SUID and SGID Files**

SUID (Set Owner User ID up on execution) and SGID (Set Group ID up on execution) are special permissions set on executable files. These permissions allow the files to be executed with the privileges of the file owner or group respectively, rather than the privileges of the user running the file.  
It's essential to note that using `LD_PRELOAD` and `LD_LIBRARY_PATH` for hijacking library loads does not work with SUID executables as these environment variables are ignored to prevent security breaches.

### **Understanding SUID and SGID**

1.  **SUID (Set User ID):** When a file with the SUID bit set is executed, it runs with the privileges of the file owner, not the user who launched it. This is often used for programs that need to perform tasks requiring higher privileges than those of the average user.
2.  **SGID (Set Group ID):** When a file with the SGID bit set is executed, it runs with the privileges of the file group. This can be used for allowing members of a group to execute a file with the permissions of the group.

### **Enumerating Custom SUID Binaries**

we likely won’t find a quick win on GTFOBins for binaries are custom, so we need to understand more about the custom binaries to determine if they can be exploited, checklist that can be followed when enumerating custom SUID binaries:

1.  Interact with the binary – Run it with various inputs to understand its functionality and output.
    
2.  Search the binary’s name on GTFOBins to see if there are any quick win exploitation methods.
    
3.  Use `strings` or `--version` (if available) to extract version information, Search on Google for known exploits.
    
4.  Extract strings from the binary – look for shared libraries or binaries being loaded / executed at runtime
    
5.  Debug the program – deep dive into how the program works some Dangerous Functions:  
    `system()`, `execvp()`, `strcpy()`, `sprintf()`, `gets()`
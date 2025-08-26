---
legal-banner: true
---

### **Step 1. Automated enumeration tools**

1.  Run **winPEAS** to gather extensive information about the system:  
    `.\winpeas.exe cmd > output.txt`
2.  Run **Seatbelt** for a broader range of checks:  
    `.\Seatbelt.exe -group=all -full > output.txt`
3.  If scripts fail, use manual commands (see the enumeration section).

### **Step 2. Review and analyze enumeration results**

1.  Carefully review results: Tools like winPEAS and Seatbelt produce large amounts of output. Take time to understand it.  
2.  Make notes of interesting findings such as misconfigurations, sensitive files, or unusual permissions.  
3.  Avoid rabbit holes: Prioritize promising leads instead of chasing low-value findings.  

### **Step 3. File and directory inspection**

1.  Inspect common directories for sensitive files, such as:  
    - `C:\`  
    - `C:\Program Files`  
    - `C:\Users\Public\Desktop`  
2.  Read through any interesting files they may contain credentials, configuration details, or escalation clues.  

### **Step 4. Quick wins**

1.  Prioritize easy methods: Look for registry exploits, weak service permissions, or obvious misconfigurations.  
2.  Check running administrative processes, note versions, and search for known exploits.  
3.  Identify internal ports that may be forwarded to your attacker machine for lateral movement or privilege escalation.  

### **Step 5. Re-evaluate enumeration data**

1.  If privilege escalation is not yet achieved, review all collected data again.  
2.  Highlight anything unusual: unfamiliar processes, suspicious file names, or unexpected users.  
3.  Reconsider kernel exploits as a last resort.  

### **General tips**

1.  Stay calm and methodical — privilege escalation often requires patience.  
2.  Document your findings meticulously. Even small details may prove useful later.  
3.  Use multiple tools: different tools (winPEAS, Seatbelt, manual enumeration) reveal different insights.  
4.  Focus on quick wins: weak permissions, stored credentials, or misconfigured services.  
5.  Be ready to adapt: if one approach fails, reassess and try alternatives.  

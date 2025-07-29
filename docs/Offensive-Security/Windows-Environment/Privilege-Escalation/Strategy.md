---
legal-banner: true
---

### **STEP 1. AUTOMATED ENUMERATION TOOLS**

1.  Run Winpeas gathers extensive information about the system.  
    `.\winpeas.exe cmd > output.txt`
2. Seatbelt: Use this alongside other scripts to cover a broader range of checks.  
    `.\Seatbelt.exe -group=all -full > output.txt`
3. Manual Commands: If scripts fail, refer to manual enumeration commands available in the enumeration section.

### **STEP 2. REVIEW AND ANALYZE ENUMERATION RESULTS**

1.  Spend Time Reviewing Results: Tools like winPEAS and Seatbelt provide extensive output. Take your time to understand the information.
2.  Make notes of interesting findings: Document anything unusual or potentially useful, such as misconfigurations, sensitive files, and permissions.
3.  Avoid rabbit holes: Focus on the most promising leads and avoid spending too much time on less likely paths.

### **STEP 3. FILE AND DIRECTORY INSPECTION**

1.  Look for files in common directories: Directories to inspect include `C:\`, `C:\Program Files`, `C:\Users\Public\Desktop`.
2. Read through any interesting files: Files may contain credentials, configuration details, or other information useful for privilege escalation.

### **STEP 4. QUICK WINS**

1.  Prioritize simple methods: Focus on methods that require fewer steps, such as registry exploits and service misconfigurations.
2.  Check administrative processes: Note their versions and search for known exploits.
3.  Identify Internal Ports: Look for ports that can be forwarded to your attacking machine.

### **STEP 5. RE-EVALUATE ENUMERATION DATA**

1.  Review All Gathered Data Again: If you haven't achieved privilege escalation, re-read your full enumeration data.
2.  Highlight anything unusual: Focus on unfamiliar processes, file names, or user names.
3.  Consider kernel exploits: Assess the potential for kernel exploits at this stage.

### **GENERAL TIPS**

1. Stay Calm and Focused: Privilege escalation is often a multi-step process that requires careful attention to detail.
2. Be Patient: Results may not be immediate. Take your time to analyze and understand the environment.
3. Keep Notes: Document your findings meticulously. A small piece of information might become crucial later.
4. Use Multiple Tools: Different tools might reveal different information. Cross-reference findings from tools like winPEAS, Seatbelt, and manual commands.
5. Focus on Quick Wins: Identify and exploit low-hanging fruits such as weak permissions, misconfigured services, and stored credentials.
6. Be Ready to Adapt: If an initial approach fails, reassess the situation and be prepared to try alternative methods.

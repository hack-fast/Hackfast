---
legal-banner: true
---

### **Establishing Persistence Using the Startup Folder**

Using the **Startup folder** for persistence is one of the simplest techniques. Any executable or script placed in this folder will automatically run each time the user logs in.

### **Step 1: Identifying the Startup Folder**

The Startup folder exists in two common locations:

1. **User-specific Startup folder** (runs only for the logged-in user):  
    `C:\Users\[Username]\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\`
    
2. **All users Startup folder** (runs for all accounts):  
    `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\`
    

### **Step 2: Creating the Executable**

1. Create an executable or script you want to run at each system startup.  
Examples: a `.bat` file, `.ps1` PowerShell script, or compiled `.exe`.

2. Ensure the payload runs without requiring additional permissions that might block execution at startup.
    

### **Step 3: Placing the Executable in the Startup Folder**

- Copy the payload to the **user-specific Startup folder**:  
    `copy path\to\your\executable.exe "C:\Users\[Username]\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\"`
- Or copy it to the all users Startup folder:
    `copy path\to\your\executable.exe "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\"`

### **Step 4: Testing Persistence**

1.  Restart the computer or log out and log back in to trigger startup items.
    
2.  Verify that your executable or script launches as expected.
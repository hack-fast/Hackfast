### **ESTABLISHING PERSISTENCE USING STARTUP FOLDER:**

Using the Startup folder for persistence is a straightforward and effective method. This technique leverages the fact that any executable placed in the Windows Startup folder will automatically run each time the user logs in.

### **STEP 1: IDENTIFYING THE STARTUP FOLDER**

The Startup folder can be located in two places—one for each user and another for all users.

1.  User-specific Startup folder:  
    `C:\Users\[Username]\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\`
    
2.  All users Startup folder:  
    `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\`
    

### **STEP 2: CREATING THE EXECUTABLE**

3.  Create an executable or script that you want to run at each system startup. This could be a simple batch file, PowerShell script, or a compiled executable.
    
4.  Ensure that your script or executable runs correctly and does not require additional permissions that might prevent it from executing at startup.
    

### **STEP 3: PLACING THE EXECUTABLE IN THE STARTUP FOLDER**

5.  You can manually copy the file to the appropriate Startup folder, or use a command line to do so.  
    `copy path\to\your\executable.exe "C:\Users\[Username]\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\"`
6.  Alternatively, Copy the file to the all users Startup folder:  
    `copy path\to\your\executable.exe "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\"`

### **STEP 4: TESTING PERSISTENCE**

1.  To test if the persistence mechanism works, restart your computer or log out and log back in to trigger the startup process.
    
2.  After logging back in, verify that your script or executable runs as expected.
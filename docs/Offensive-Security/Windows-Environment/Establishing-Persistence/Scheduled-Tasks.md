---
legal-banner: true
---

### **INTRODUCTION**

Creating and managing scheduled tasks in Windows can be done via the graphical user interface of Task Scheduler or through the command line using `schtasks`, a powerful command-line tool provided by Windows.

### **Step-by-Step Guide to Establish Persistence Using Scheduled Tasks**

1.  Make sure the script or executable that you want to run is tested and properly located in a secure directory where it has appropriate permissions.
    
2.  Create a Task that Triggers at System Startup  
    `schtasks /create /tn "MyStartupTask" /tr "C:\Path\To\Your\Executable.exe" /sc onstart /ru SYSTEM`
    
3.  Create a Task that Triggers at User Logon: Replace UserName with the actual username if you want it to run only under a specific user account, or use users to run for all users.  
    `schtasks /create /tn "MyLogonTask" /tr "C:\Path\To\Your\Executable.exe" /sc onlogon /ru "UserName"`
    
4.  Create a Task that Triggers Daily at a Specific Time:  
    `schtasks /create /tn "MyDailyTask" /tr "C:\Path\To\Your\Executable.exe" /sc daily /st 10:00 /ru SYSTEM`
    
5.  Change the Properties of an Existing Task:  
    `schtasks /change /tn "MyStartupTask" /sc daily /st 09:00`
    
6.  Remove a Task When No Longer Needed:  
    `schtasks /delete /tn "MyStartupTask" /f`
    
7.  View All Scheduled Tasks:  
    `schtasks /query`
    
8.  After setting up your task, you can run it manually to test if it works correctly:  
    `schtasks /run /tn "MyStartupTask"`
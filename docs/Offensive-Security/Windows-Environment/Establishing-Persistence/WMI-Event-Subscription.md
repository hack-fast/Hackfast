---
legal-banner: true
---

### **INTRODUCTION**

Windows Management Instrumentation (WMI) is a powerful feature in Windows that allows for system management and monitoring. WMI Event Subscription is a technique that can be used by administrators—and malicious actors—for persistence, allowing scripts or programs to execute automatically when specified system events occur.

### **STEP-BY-STEP TO ESTABLISH PERSISTENCE USING WMI EVENT SUBSCRIPTION**

1.  Identify the Trigger Event: Determine the system event that will trigger the execution of your script or program. An example is `__InstanceCreationEvent`, which occurs when a new instance of a WMI class is created.
    
2.  Create an event filter to specify the conditions under which the event is triggered. The filter uses WQL (WMI Query Language) to define these conditions.
    
    ```POWERSHELL
    New-CimInstance -Namespace root\subscription -ClassName __EventFilter -Property @{
        Name = 'MyEventFilter';
        EventNamespace = 'root\cimv2';
        QueryLanguage = 'WQL';
        Query = "SELECT * FROM __InstanceCreationEvent WITHIN 60 WHERE TargetInstance ISA 'Win32_Process' AND TargetInstance.Name = 'notepad.exe'"
    }
    ```
    
3.  Define the action to be taken when the event filter conditions are met by creating an event consumer. In this example, a script is executed.
    
    ```POWERSHELL
    New-CimInstance -Namespace root\subscription -ClassName CommandLineEventConsumer -Property @{
        Name = 'MyEventConsumer';
        CommandLineTemplate = "C:\\Path\\To\\Your\\Script.bat"
    }
    ```
    
4.  Link the event filter to the event consumer to ensure that the specified action is taken when the conditions are met.
    
    ```POWERSHELL
    New-CimInstance -Namespace root\subscription -ClassName __FilterToConsumerBinding -Property @{
        Filter = (Get-CimInstance -Namespace root\subscription -ClassName __EventFilter -Filter "Name = 'MyEventFilter'").__PATH;
        Consumer = (Get-CimInstance -Namespace root\subscription -ClassName CommandLineEventConsumer -Filter "Name = 'MyEventConsumer'").__PATH
    }
    ```
    
5.  Test your setup by triggering the event. Start `notepad.exe` and check if the script runs as expected.
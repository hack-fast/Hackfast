### **SYSTEM ENUMERATION**

1. display the hostname of the system:
    `hostname`
2. display detailed kernel information:
    `uname -a`
3. display the distribution information:
    `cat /etc/issue`
4. list all running processes:
    `ps aux`
5. show disk usage in human-readable format:
    `df -h`
6. display memory usage:
    `free -m`

### **USER ENUMERATION**

1. display the current user:
    `whoami`
2. display user identity:
    `id`
3. view commands that can be run as sudo:
    `sudo -l`
4. list all users:
    `cat /etc/passwd`
5. extract usernames:
    `cat /etc/passwd | cut -d : -f 1`
6. list user password hashes (requires root):
    `cat /etc/shadow`
7. list all groups:
    `cat /etc/group`
8. view command history:
    `history`
9. check for nfs shares:
    `cat /etc/exports`
10. list users in the sudo group:
    `grep '^sudo:.*$' /etc/group`
11. display the version of sudo:
    `sudo -V`

### **NETWORK ENUMERATION**

1. display all ip addresses:
    `ip a s`
2. display routing table:
    `ip route`
3. display arp table:
    `ip neigh`
4. check open ports and associated services:
    `netstat -ano`
5. display all network interfaces:
    `ifconfig -a`
6. list all listening ports:
    `ss -tuln`
7. display summary of active connections:
    `ss -s`
8. display dns configuration:
    `cat /etc/resolv.conf`
9. list all firewall rules:
    `iptables -L`
10. display wireless network interfaces:
    `iwconfig`

### **ENUMERATION TECHNIQUES**

1. find files with the suid bit set:
    `find / -perm -4000 2>/dev/null`
2. find files with the sgid bit set:
    `find / -perm -2000 2>/dev/null`
3. find writable directories:
    `find / -writable 2>/dev/null`
4. find world writable files:
    `find / -perm -o+w -type f 2>/dev/null`
5. list open files and associated network connections:
    `lsof -i`
6. identify potential kernel exploits:
    `uname -r; searchsploit $(uname -r)`
7. display the status of all services:
    `service --status-all`
8. list scheduled cron jobs:
    `crontab -l; ls -la /etc/cron*`
9. list installed packages:
    `dpkg -l | grep -i "package_name"`
10. extract and display firefox browser history:
    `cat ~/.mozilla/firefox/*.default-release/places.sqlite`

### **AUTOMATED ENUMERATION TOOLS**

1. [LINPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
2. [LINENUM](https://github.com/rebootuser/LinEnum)
3. [LINUX EXPLOIT SUGGESTER](https://github.com/mzet-/linux-exploit-suggester)
4. [LINUX SMART ENUMERATION](https://github.com/diego-treitos/linux-smart-enumeration)
5. [LINUX PRIV CHECKER](https://github.com/linted/linuxprivchecker)
6. [PSPY](https://github.com/DominicBreuker/pspy)
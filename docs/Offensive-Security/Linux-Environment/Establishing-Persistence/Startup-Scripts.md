### **STEP 1: CREATE THE SCRIPT**

1.  Create the backdoor script with the necessary commands and actions:
    `nano /path/to/your_script.sh`
    
2.  Make the script executable.  
    `chmod 755 /path/to/your_script.sh`
    
3.  Add the script to the default runlevels so it runs at startup:
    `update-rc.d /path/to/your_script.sh defaults`
    

### **STEP 2: VERIFY SCRIPT PERMISSIONS AND PATH**

1.  Check the script permissions to ensure it is executable by the system:
    `ls -l /path/to/your_script.sh`
2.  Confirm the script path and the shebang (`#!/bin/bash`) at the top of the script to specify the interpreter.

### **STEP 3: MANUAL METHOD - CREATE A SYSTEMD SERVICE (ALTERNATIVE)**

1.  Create a new service file for your script in the systemd directory.  
    `nano /etc/systemd/system/your_service_name.service`
    
2.  Add the following content to the service file:
    
    ```
    [Unit]
    Description=Your Script Service
    After=network.target
    
    [Service]
    ExecStart=/path/to/your_script.sh
    ExecReload=/bin/kill -HUP $MAINPID
    ExecStop=/bin/kill -SIGINT $MAINPID
    Restart=on-failure
    User=root
    
    [Install]
    WantedBy=multi-user.target
    ```
    
3.  Reload the systemd daemon to recognize the new service.  
    `systemctl daemon-reload`
    
4.  Enable the service so it starts on boot.  
    `systemctl enable your_service_name.service`
    
5.  Start the service immediately (optional).  
    `systemctl start your_service_name.service`
    

### **STEP 4: VERIFYING AND TROUBLESHOOTING**

1.  Check the status of your service to ensure it’s running correctly:
    `systemctl status your_service_name.service`
2.  View logs for any issues or errors.  
    `journalctl -u your_service_name.service`
3.  Check the runlevel symlinks to ensure your script is correctly linked:
    `ls -l /etc/rc*.d/*your_script.sh`
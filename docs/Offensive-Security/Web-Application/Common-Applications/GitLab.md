### **INTRODUCTION**

GitLab is an open-source DevOps platform that combines source code management (SCM) with continuous integration and delivery (CI/CD) tools. It allows teams to collaborate on code, track issues, and automate the software development process from a single application. GitLab is widely appreciated for its comprehensive suite of features that support project planning, source code management, testing, deployment, and monitoring.

### **PUBLIC INFORMATION GATHERING**

1.  Searching for GitLab Usernames:  
    `site:[GITLAB-DOMAIN] "username"`
2.  Google Dorking for Projects:  
    `site:[GITLAB-DOMAIN] inurl:projects "project name"`

### **IDENTIFYING GITLAB VERSION**

1.  Obtaining Version from Header:  
    `curl -s -I http://[GITLAB-DOMAIN]/ | grep -i "X-Gitlab-Version"`
2.  Obtaining Version from Help Page  
    `http://[GITLAB-DOMAIN]/help`
3.  Obtaining Version Using Nmap:  
    `nmap -sV -p 80,443 [GITLAB-DOMAIN]`
4.  Using GitLab API to List Users:  
    `curl -s http://[GITLAB-DOMAIN]/api/v4/users?private_token=[YOUR_TOKEN]`

### **DEFAULT CONFIGURATION AND CREDENTIALS**

1.  Attempt default credentials (e.g., root:5iveL!fe) on the login page:  
    `http://[GITLAB-DOMAIN]/users/sign_in`
2.  Checking for Exposed .git Directories:  
    `wget -r -np -R "index.html*" http://[GITLAB-DOMAIN]/.git/`
3.  Searching for Leaked Sensitive Information in Repositories:  
    `trufflehog --json https://gitlab.com/[USERNAME]/repository.git`
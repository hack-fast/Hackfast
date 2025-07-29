### **Introduction**

GitLab is an open-source DevOps platform that combines source code management (SCM) with continuous integration and delivery (CI/CD) tools. It allows teams to collaborate on code, track issues, and automate the software development process from a single application. GitLab is widely appreciated for its comprehensive suite of features that support project planning, source code management, testing, deployment, and monitoring.

### **Public Information Gathering**

1.  Search for GitLab usernames:  
    `site:[GITLAB-DOMAIN] "username"`
2.  Google Dorking for Projects:  
    `site:[GITLAB-DOMAIN] inurl:projects "project name"`

### **Identifying GitLab Version**

1.  Obtaining Version from Header:  
    `curl -s -I http://[GITLAB-DOMAIN]/ | grep -i "X-Gitlab-Version"`
2.  Obtaining Version from Help Page  
    `http://[GITLAB-DOMAIN]/help`
3.  Obtaining Version Using Nmap:  
    `nmap -sV -p 80,443 [GITLAB-DOMAIN]`
4.  Using GitLab API to List Users:  
    `curl -s http://[GITLAB-DOMAIN]/api/v4/users?private_token=[YOUR_TOKEN]`

### **Default Configuration and Credentials**

1.  Attempt default credentials (e.g., root:5iveL!fe) on the login page:  
    `http://[GITLAB-DOMAIN]/users/sign_in`
2.  Check for exposed .git directory:  
    `wget -r -np -R "index.html*" http://[GITLAB-DOMAIN]/.git/`
3.  Search for leaked secrets using TruffleHog:  
    `trufflehog --json https://gitlab.com/[USERNAME]/repository.git`
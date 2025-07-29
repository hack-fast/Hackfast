### **Methodology**

There are many applications you may come across while performing penetration testing. I've provided a cheat sheet for some of the most common ones that you may find during your assessments. If you discover a software application that I haven't covered, don't worry. I've also included a generalized methodology that can be applied to any software application you might come across.

### **Step 1. Information Gathering**

Begin by Understand the application architecture and identifying the version of the Application This information can often be critical in determining known vulnerabilities.

| Task | Description |
| --- | --- |
| HTTP Headers | Inspect HTTP headers as they might reveal version details of web servers, frameworks. |
| HTML and JavaScript Comments | Inspect the source code of web pages for comments that may include version numbers or build dates. |
| Review Open Source Application | Explore the GitHub/GitLab repository structure and read documentation to find files that could provide valuable information. |
| File and Directory Discovery | Use tools like DirBuster to discover hidden files and directories that are not linked from the main site but are still accessible. |
| Use Specialized Automation Tools | Use automation tools specifically built for certain applications, like WPScan for WordPress, to uncover vulnerabilities and configuration issues. |
| Error Messages and Stack Traces | Check for verbose error messages or stack traces in the application which can reveal useful information about the backend architecture. |
| Browser Extensions | Use browser extensions like Wappalyzer or BuiltWith to automatically detect and display web technology details. |

### **Step 2. Try Default Passwords**

Check for default passwords. Many Software are left with default credentials, which poses significant security risks if not changed.

| Task | Description |
| --- | --- |
| Identify Default Credentials | Refer to the application documentation or installation guides, search online for default credential lists, and be aware that some applications have default credentials that are not publicly documented. |
| Common Default Credentials | Examples `admin/admin`, `root/root`, `user/password`, `guest/guest`. |
| Online Resources | Use websites like `default-password.info` to find default credentials for various web applications. |
| Automated Tools | Use tools like `Hydra`, `Medusa`, or `Ncrack` to automate the process of trying default credentials . |
| Check Configuration Files | Inspect web application configuration files for hardcoded credentials that might have been left by developers. |
| Review Source Code | Analyze the web application source code for any hardcoded default credentials. |
| Consult Community Forums | Visit forums and community sites such as Reddit, Stack Overflow, or vendor-specific forums to see if others have shared default credentials for the web application in question. |

### **Step 3. Check for Common Vulnerabilities**

check If the version or plugin version vulnerable. by Using the following techniques:

| Technique | Query Example |
| --- | --- |
| Use specialized databases and forums | `site:exploit-db.com | site:github.com | site:securityfocus.com "[Software Name] [version]"` |
| Search for exploits or vulnerabilities | `"[Software Name] [version]" +exploit | vulnerability` |
| Look for patches, updates, or changelogs | `"[Software Name] [version]" +patch | update | changelog` |

### **Step 4. Learn to Exploit the Vulnerability**

If you are unfamiliar with how to exploit a found vulnerability, consider learning from practical examples and tutorials. A good resource for walkthroughs and tutorials is IppSec and 0xdf, which can be accessed here:

- `https://ippsec.rocks/`
- `https://0xdf.gitlab.io/tags`  
    Use the search function to find relevant videos by entering the software name and version.
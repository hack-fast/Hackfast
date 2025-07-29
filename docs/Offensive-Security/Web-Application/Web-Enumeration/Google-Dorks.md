### **Introduction**

Google Dorks Used advanced search syntax to find specific information on the internet. This table lists various search queries, known as "Google Dorks," that are used to identify potential security exposures in server configurations.

### **Server Configurations and Exposures**

| Google Dork | Description | Example Use-Case |
| --- | --- | --- |
| `intitle:"Apache2 Ubuntu Default Page: It works"` | Searches for Apache default installation page on Ubuntu servers. | Used to find unsecured servers needing configuration. |
| `inurl:"server-status" intitle:"Apache Status" intext:"Apache Server Status for"` | Finds pages displaying Apache server status. | Helps identify servers potentially exposing sensitive operational data. |
| `filetype:conf inurl:firewall -intitle:cvs` | Locates firewall configuration files. | Used to check for insecure firewall settings that are publicly accessible. |
| `inurl:Dashboard.jspa intext:"Atlassian Jira Project Management Software"` | Searches for Atlassian Jira dashboards. | Identifies companies using Jira and possibly exposes project management practices. |
| `intitle:"IIS Windows Server" -inurl:"IIS Windows Server"` | Finds Microsoft IIS servers without the default page in the URL. | Used to discover IIS servers that might be poorly configured. |
| `inurl:app/kibana intext:Loading Kibana` | Locates Kibana applications in use. | Finds exposed Kibana dashboards which may reveal insights into log analytics. |
| `intitle:"Swagger UI - " + "Show/Hide"` | Finds Swagger UI endpoints. | Used to uncover API endpoints, potentially revealing API methods and data structures. |
| `inurl:/filedown.php?file=` | Searches for file download PHP scripts. | Identifies direct file download links that could be unsecured. |
| `intitle:"Webmin *" "login" "Username" -filetype:html` | Finds login pages for Webmin server management. | Pinpoints Webmin panels that may allow unauthorized access if unsecured. |
| `inurl:/phpMyAdmin/index.php server=*` | Finds phpMyAdmin interfaces with server parameters. | Detects openly accessible database management interfaces that might be exploitable. |

### **Database and File Exposures**

| Google Dork | Description | Example Use-Case |
| --- | --- | --- |
| `ext:(doc \| pdf \| xls \| txt \| ps \| rtf \| odt \| sxw \| psw \| ppt \| pps \| xml) (intext:confidential salary \| intext:"budget approved") inurl:confidential` | Searches for sensitive documents marked as confidential containing salaries or budget information. | Used to locate leaked financial documents which could expose personal or corporate financial data. |
| `filetype:SWF SWF` | Finds Adobe Flash SWF files. | Used to identify outdated or potentially vulnerable Flash content on websites. |
| `filetype:TXT TXT` | Locates plain text files. | Often used to find exposed configuration files or logs that contain sensitive information. |
| `filetype:XLS XLS` | Searches for Excel spreadsheet files. | Helps in locating spreadsheets that might contain personal data, financial figures, or internal data. |
| `filetype:pdf "Assessment Report" nessus` | Finds Nessus assessment report PDFs. | Used to uncover security assessment reports which might disclose vulnerabilities. |
| `"index of" "database.sql.zip"` | Searches for zipped SQL database backups. | Locates exposed database backups which can be downloaded without authentication. |
| `intitle:index.of.?.sql` | Finds directory listings of SQL files. | Used to discover unprotected databases available directly via the browser. |
| `filetype:url +inurl:"ftp://" +inurl:";@"` | Searches for URL files containing FTP credentials. | Identifies FTP access details that are inadvertently exposed online. |
| `s3 site:amazonaws.com filetype:log` | Finds Amazon S3 log files. | Used to spot exposed log files on S3 buckets that might contain sensitive operational data. |
| `intext:"@gmail.com" AND intext:"@yahoo.com" filetype:sql` | Searches for SQL files containing personal email addresses. | Detects databases dumped online which include user email addresses, potentially for data breach analysis. |
| `intitle:"index of" "/aws.s3/"` | Finds directories listing content from AWS S3 buckets. | Used to identify open S3 buckets with possibly sensitive stored data. |
| `inurl:/_cat/indices?v` | Searches for Elasticsearch indices information. | Used to locate exposed Elasticsearch clusters that might reveal a wealth of structured data. |

### **Login Portals and Authentication Exposures**

| Google Dork | Description | Example Use-Case |
| --- | --- | --- |
| `site:*/auth intitle:login` | Searches for login pages across various domains containing '/auth' in the URL. | Used to identify accessible login pages that could be targeted for password attacks. |
| `inurl: admin/login.aspx` | Finds login pages specifically for admin portals using ASP.NET. | Utilized to pinpoint administrative login portals that may have weaker security settings. |
| `inurl:cgi/login.pl` | Locates login pages executed by Perl CGI scripts. | Used to detect outdated or potentially insecure Perl-based authentication pages. |
| `inurl:zoom.us/j and intext:scheduled for` | Searches for publicly accessible Zoom meeting links and schedules. | Helps in finding Zoom meetings that could be accessed without proper invitations. |
| `inurl:office365 AND intitle:"Sign In \| Login \| Portal"` | Finds login portals for Office 365 services. | Used by security professionals to audit for phishing pages that mimic Office 365 login portals. |
| `inurl:'/scopia/entry/index.jsp'` | Locates login pages for Scopia  video conferencing tools. | Identifies potential entry points into video conference systems that could be exploited. |
| `inurl:"/vpn/tmindex.html"` | Searches for specific VPN login pages. | Find exposed VPN services that might allow unauthorized access if compromised. |

### **Miscellaneous Exposures**

| Google Dork | Description | Example Use-Case |
| --- | --- | --- |
| `intitle:"qBittorrent Web UI" inurl:8080` | Finds instances of the qBittorrent web interface, typically hosted on port 8080. | Used to identify unprotected qBittorrent interfaces that could allow unauthorized access to torrent downloads. |
| `site:https://docs.google.com/spreadsheets edit` | Searches for publicly editable Google Spreadsheets. | Utilized to find spreadsheets that are inadvertently exposed and could be modified by unauthorized users. |
| `intitle:"index of" unattend.xml` | Locates directories containing unattend.xml files, which automate Windows setup processes. | Used to discover unsecured Windows deployment files that could expose sensitive setup parameters. |
| `intitle: "index of" "./" "./bitcoin"` | Finds directories that could potentially contain Bitcoin wallet files or related information. | Helps in identifying potentially exposed Bitcoin-related files for security checks or recovery. |
| `inurl:"/index.aspx/login"` | Searches for login pages associated with ASP.NET web applications. | Aids in locating ASP.NET login portals that may be vulnerable to attacks. |
| `inurl:"/wp-content/uploads/" intext:"web.config" intitle:"Index of"` | Finds instances where the 'web.config' file for Windows servers might be exposed in WordPress uploads directories. | Used to detect exposed server configuration files which could compromise a website's security. |

### **Automating Google Dork Queries**

Automating search queries enhances efficiency and coverage. Tools and scripts can rapidly scan for multiple queries, identifying potential exposures quickly and systematically.

### **GooFuzz**

GooFuzz is a fast, Go-based tool with a variety of flags for efficient dorking. Follow these steps to install and use it:

1.  Clone the repository:  
    `sudo git clone https://github.com/m3n0sd0n4ld/GooFuzz`
2.  Navigate to the directory:  
    `cd GooFuzz`
3.  Copy the binary to your local bin:  
    `sudo cp GooFuzz /usr/local/bin/`
4.  Display the help menu for more information :  
    `GooFuzz -h`
5.  Run GooFuzz with specified options:  
    `GooFuzz -t target.com -e pdf,doc,bak`

### **GoDork**

go-dork supports multiple search engines, making it ideal for comprehensive dork scanning. Follow these steps to get started:

1.  Clone the repository:  
    `sudo git clone https://github.com/dwisiswant0/go-dork.git`
2.  Navigate to the directory:  
    `cd go-dork`
3.  Build the binary:  
    `sudo go build`
4.  Copy the binary to your local bin:  
    `sudo cp go-dork /usr/local/bin/`
5.  Perform a dork scan with a specific query:  
    `go-dork -q "inurl:'testTarget'"`
6.  Specify a search engine and query:  
    `go-dork -e google -q ".php?id="`
7.  Use a list of dorks from a file:  
    `cat dorks.txt | go-dork -p 5`

### **Google Dork Scanning Script**

A simple shell script for rapid Google Dork scanning:

1.  Clone the repository:  
    `git clone https://github.com/IvanGlinkin/Fast-Google-Dorks-Scan.git`
2.  Navigate to the directory:  
    `cd Fast-Google-Dorks-Scan/`
3.  Make the script executable:  
    `chmod +x FGDS.sh`
4.  Run the script:  
    `./FGDS.sh target.com`
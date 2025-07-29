### **Manual Techniques**

Web Application Firewalls (WAFs) are designed to protect web applications by filtering and monitoring HTTP traffic. They can block malicious requests such as SQL injections or cross-site scripting (XSS) attacks, and can interfere with security testing and enumeration.

| Technique | Description | Example |
| --- | --- | --- |
| Analyze HTTP Responses | Manually inspect HTTP responses for WAF-specific headers or messages. | Use browser developer tools or a proxy tool like Burp Suite to inspect HTTP headers. |
| Observe Response Codes | Pay attention to HTTP response codes, | particularly `403 Forbidden` |
| Rate Limiting Detection | Send a high number of requests in a short period to see if rate limiting is triggered, indicating WAF protection. | Use tools like `Slowloris` or `Burp Suite Intruder`. |
| Custom Error Pages | Note if custom error pages are served when certain payloads are sent, which can indicate WAF intervention. | A unique error page or message that differs from standard server error pages. |
| Check Response Time | Measure the response time of the web server. A significant delay in responses might indicate WAF processing. | Use tools like OWASP ZAP or Burp Suite to automate response time analysis. |

### **Using Nmap**

Nmap can detect and fingerprint Web Application Firewalls (WAFs) using the http-waf-detect script to identify WAF presence and the http-waf-fingerprint script to determine the WAF type.

| Function | Command | Description |
| --- | --- | --- |
| Detect WAF | `nmap -p80 --script http-waf-detect [host]` | Detects the presence of a WAF. |
| Fingerprint WAF | `nmap -p80 --script http-waf-fingerprint [host]` | Determines the type of WAF. |
| Scan Multiple Ports | `nmap -p80,443 --script http-waf-detect,http-waf-fingerprint [host]` | Scans multiple ports to detect and fingerprint WAF. |

### **Using WAFW00F**

WAFW00f is a tool specifically designed to identify and fingerprint WAFs by sending multiple requests and analyzing the responses. It supports a wide range of WAFs.

| Function | Command | Description |
| --- | --- | --- |
| List Supported WAFs | `wafw00f -l` | Lists all WAFs supported by WAFW00f. |
| Fingerprint WAF | `wafw00f [url]` | Fingerprints the WAF protecting a specific URL. |
| Verbose Output | `wafw00f -v [url]` | Provides verbose output for detailed analysis. |
| Output in JSON Format | `wafw00f -o json [url]` | Outputs the results in JSON format. |

### **Using SQLMap**

SQLMap can be used to detect the presence of a WAF and even attempt to bypass it during SQL injection testing. The --identify-waf flag helps in identifying the WAF.

| Function | Command | Description |
| --- | --- | --- |
| Detect and Bypass WAF | `sqlmap --url=[url] --batch --identify-waf` | Detects the presence of a WAF and attempts to bypass it during SQL injection testing. |
| Set Custom HTTP Headers | `sqlmap --url=[url] --headers="X-Forwarded-For: 127.0.0.1"` | Sets custom HTTP headers to potentially bypass WAF rules. |
| Delay Between Requests | `sqlmap --url=[url] --delay=5` | Adds a delay between requests to avoid rate limiting by the WAF. |
| Randomize User-Agent | `sqlmap --url=[url] --random-agent` | Randomizes the User-Agent header to evade detection. |

### **Using Burp Suite**

Burp Suite can detect and fingerprint WAFs by using the Intruder tool to analyze response patterns for WAF activity and has extensions like WAF Detector to identify WAF signatures and behaviors automatically.

| Function | Description |
| --- | --- |
| Initial Detection | Use the Intruder tool in Burp Suite to send multiple payloads. Analyze the responses for patterns that indicate WAF activity, such as consistent blocking of certain types of requests. |
| WAF Fingerprinting | Use Burp Suite extensions like WAF Detector to automate the detection and fingerprinting process. These extensions leverage known signatures and behavioral patterns. |
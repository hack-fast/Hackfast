### **Importance of Subdomains in Web Reconnaissance**

Subdomains are crucial targets in web reconnaissance because they often reveal overlooked and potentially vulnerable parts of an organization's infrastructure. Here's why they're important:

| Aspect                  | Details                                                                                              |
|-------------------------|-------------------------------------------------------------------------------------------------------|
| Development and Staging | Subdomains used to test new features or updates before deploying them to the main site. These environments sometimes contain vulnerabilities or expose sensitive information due to relaxed security measures. |
| Hidden Login Portals    | Subdomains hosting administrative panels or other login pages not meant to be publicly accessible. Attacker seeking unauthorized access can find these as attractive targets.                        |
| Legacy Applications     | Older, forgotten web applications residing on subdomains, potentially containing outdated software with known vulnerabilities.                                          |
| Sensitive Information   | Subdomains inadvertently exposing confidential documents, internal data, or configuration files that could be valuable to attacker.                              |

### **Active Subdomain Enumeration**

Active subdomain enumeration involves directly interacting with the target to discover subdomains. Here are some of the most effective methods and their commands:

| Method | Command |
| --- | --- |
| Fuzzing with wfuzz | `wfuzz -u http://10.10.11.177 -H "Host: FUZZ.hackfa.st" -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt --hh 1131` |
| Fuzzing with FFUF | `ffuf -u http://10.10.11.251 -H "Host: FUZZ.hackfa.st" -w /opt/SecLists/Discovery/DNS/subdomains-top1million-20000.txt -mc all -ac` |
| Virtual Host Scanning | `gobuster vhost -u https://[DOMAIN-NAME] -w subdomains.txt` |
| DNS Zone Transfers | `dig @ns.example.com domain.com AXFR`  <br>`host -t axfr domain.com ns.example.com` |
| DNS Brute Forcing | `dnsmap [DOMAIN-NAME] -r report.txt`  <br>`fierce --domain [DOMAIN-NAME]` |
| DNS Zone Transfer | `dig @[DNS-SERVER] [DOMAIN-NAME] AXFR` |
| Content Discovery Tools | `Burp Suite` -> `Target` -> `Site map` -> `Right-click` -> `Passively scan this host` |

### **Passive Subdomain Enumeration**

Passive subdomain enumeration leverages external data sources to identify subdomains without directly interacting with the target. Here are some effective methods and their commands:

| Method | Command |
| --- | --- |
| Certificate Transparency | `curl -s "https://crt.sh/?q=$[DOMAIN-NAME]&output=json" \| jq -r '.[] \| "\(.name_value)\n\(.common_name)"' \| sort -u` |
| Search Engines | `theHarvester -d [DOMAIN-NAME] -b google`  <br>`theHarvester -d [DOMAIN-NAME] -b bing -l 500` |
| DNS Enumeration Scripts | `dnsenum --enum [DOMAIN-NAME]`  <br>`nmap --script dns-brute --script-args dns-brute.domain=[DOMAIN-NAME]` |
| APIs for DNS Data | `curl -s "https://api.securitytrails.com/v1/domain/domain.com/subdomains" -H "APIKEY: APIKEYHERE"`  <br>`curl -s "https://www.virustotal.com/vtapi/v2/domain/report?apikey=[APIKEY]&domain=[DOMAIN-NAME]"` |
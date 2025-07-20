### **INVESTIGATING UNRECOGNIZED OPEN PORTS**

Identifying an unrecognized open port during penetration testing might provide a potential entry point .This guide provides straightforward steps to understand and inspect these ports.

### **STEP 1. IDENTIFYING SERVICE DETAILS**

1. Port Information Database: Use a port information database to identify the common services associated with the port. A reliable source is SpeedGuide. `https://www.speedguide.net/port.php?port=[PORT]`  
    Replace `[PORT]` with the actual number of the port you are investigating.
2. Banner Grabbing: Use tools like Telnet, Netcat to extract banners from open services, which often include the type and version of the service running. For example:
    1.  `telnet [TARGET-IP] [PORT]`
    2.  `nc -v [TARGET-IP] [PORT]`

### **STEP 2: CHECK FOR COMMON VULNERABILITIES**

Once you have identified which service is running on the port, use a combination of search techniques to identify any known vulnerabilities associated with it.

1.  Query well-known vulnerability databases and blogs:  
    `site:exploit-db.com | site:github.com | site:0xdf.gitlab.io "[Software Name] [version]"`
2.  Search to find any known exploits or vulnerabilities:  
    `"[Service Name] [version]" +exploit | vulnerability`

    ??? info "Tips & Tricks"

        Prioritize sources from ExploitDB/Github/Rapid7.

3.  Search for any patches, updates, or changelogs that might indicate security fixes:     
    `"[Service Name] [version]" +patch | update | changelog`  
    

### **STEP 3: LEARN FROM PRACTICAL EXAMPLES AND TUTORIALS**

If you are unfamiliar with how to exploit a found vulnerability, consider learning from practical examples and tutorials. Excellent resources for walkthroughs and tutorials include:

1.  Search for relevant videos by entering the software name.  
    `https://ippsec.rocks/`
2.  Use tags to find detailed walkthroughs.  
    `https://0xdf.gitlab.io/tags`

### **STEP 4: ADDITIONAL TOOLS AND TECHNIQUES**

1. NMAP SCRIPTING ENGINE (NSE): Use NSE scripts to gather more information and potentially identify vulnerabilities automatically. For example:  
`nmap -sV --script vuln [TARGET-IP] -p [PORT-NUMBER]`
2. SHODAN AND CENSYS: Use scanners like Shodan and Censys to find similar services on other systems and see if they are known to be vulnerable.
    1.  [Shodan](https://www.shodan.io/)
    2.  [Censys](https://censys.io/)
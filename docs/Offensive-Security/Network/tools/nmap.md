---
legal-banner: true
---

### **Overview**

Nmap (Network Mapper) is a free and open-source tool used for network discovery and security auditing. It is widely used to identify what devices are running on a network, discover open ports, detect security risks, and map network infrastructure.

```bash
# Full TCP port scan with high rate
nmap -p- --min-rate 10000 [TARGET-IP] -oN ports.nmap

# Extract open ports as comma-separated list
cat ports.nmap | grep 'open' | awk '{ print $1 }' | awk '{print ($0+0)}' | sed -z 's/\n/,/g;s/,$/\n/'

# Run default scripts and version detection on specific ports
nmap -p [PORT,PORT,PORT] -sCV [TARGET-IP] -oN Final.nmap
```

### **Target Specification**


| Option | Example | Description |
| --- | --- | --- |
|     | `nmap [IP-ADDRESS]` | Scan a single IP to identify open ports and associated services. |
|     | `nmap [IP-ADDRESS] [IP-ADDRESS]` | Scan a two specified IP addresses to identify open ports and associated services. |
|     | `nmap [IP-ADDRESS/RANGE]` | Scan an entire subnet to identify open ports and associated services. |
|     | `nmap [DOMAIN-NAME]` | Scan a domain to resolve and assess services hosted publicly. |
| `-iL` | `nmap -iL [TARGETS].txt` | Import a list of targets from a file, useful for organized sequential scans. |
| `-iR` | `nmap -iR 100` | Randomly scan 100 hosts to discover potentially vulnerable systems. |
| `--exclude` | `nmap --exclude [IP-ADDRESS]` | Exclude specific hosts from scan to focus on untested systems. |


### **Scan Techniques**


| Option | Example | Description |
| --- | --- | --- |
| `-sS` | `nmap [IP-ADDRESS] -sS` | Executes a stealth SYN scan to discreetly identify active ports without finalizing the TCP handshake. |
| `-sT` | `nmap [IP-ADDRESS] -sT` | Use a TCP connect scan to check port availability through full TCP handshake (less stealthy). |
| `-sU` | `nmap [IP-ADDRESS] -sU` | Probe UDP ports, which are often less monitored and can reveal services like DNS, SNMP, or DHCP. |
| `-sA` | `nmap [IP-ADDRESS] -sA` | Conduct an ACK scan to map out firewall rules regarding stateful inspection and filtered ports. |
| `-sW` | `nmap [IP-ADDRESS] -sW` | Use a TCP Window scan to infer window size and detect potential for denial-of-service vulnerabilities. |
| `-sM` | `nmap [IP-ADDRESS] -sM` | Implement a Maimon scan to explore peculiarities in TCP stack implementations. |

### **Host Discovery**


| Option | Example | Description |
| --- | --- | --- |
| `-sL` | `nmap 192.168.1.1-3 -sL` | Lists network hosts without sending packets, useful for network inventories. |
| `-sn` | `nmap [IP-ADDRESS/RANGE] -sn` | Ping scan to identify which hosts are up. |
| `-Pn` | `nmap 192.168.1.1-5 -Pn` | Assumes all hosts are live, useful when hosts block ping requests. |
| `-PS` | `nmap 192.168.1.1-5 -PS22-25,80` | Use TCP SYN packets to lightly probe specified ports and detect responsive hosts. |
| `-PA` | `nmap 192.168.1.1-5 -PA22-25,80` | Sends TCP SYN packets to specified ports to infer active hosts. |
| `-PU` | `nmap 192.168.1.1-5 -PU53` | Sends UDP packets to the specified port to check for host responses. |
| `-PR` | `nmap 192.168.1.1-1/24 -PR` | Uses ARP to find active hosts within a local network segment. |
| `-n` | `nmap 192.168.1.1 -n` | Disables DNS resolution to speed up the scan. |

### **Port Specification**


| Option | Example | Description |
| --- | --- | --- |
| `-p` | `nmap [IP-ADDRESS] -p [PORT]` | Scan a specific port to check for services like FTP and potential misconfigurations. |
| `-p` | `nmap [IP-ADDRESS] -p [FROM-PORT]-[TO-PORT]` | Scan a range of ports to uncover a wider array of services and their states. |
| `-p` | `nmap [IP-ADDRESS] -p U:[PORT],T:[PORT]-[PORT],[PORT]` | Scan both TCP and UDP ports to gather comprehensive details about network services. |
| `-p` | `nmap [IP-ADDRESS] -p-` | Perform a full port scan to thoroughly assess all potential points of network entry. |
| `-p` | `nmap [IP-ADDRESS] -p http,https` | Target specific service ports based on protocol names to focus on web services. |
| `-F` | `nmap [IP-ADDRESS] -F` | Conduct a fast scan of top 100 most common ports, ideal for initial reconnaissance. |
| `--top-ports` | `nmap [IP-ADDRESS] --top-ports 2000` | Scan the top 2000 most common ports for a broad yet efficient overview of network exposure. |
| `-p-65535` | `nmap [IP-ADDRESS] -p-65535` | Scan from port 1 through 65535 to explore less commonly used ports. |
| `-p0-` | `nmap [IP_ADDRESS] -p0-` | Start from port 0 to include all possible ports in the scan up to 65535. |

### **Service and Version Detection**


| Option | Example | Description |
| --- | --- | --- |
| `-sV` | `nmap [IP-ADDRESS] -sV` | Detects service versions running on open ports. |
| `-sV --version-intensity` | `nmap [IP-ADDRESS] -sV --version-intensity 8` | Sets the intensity of version detection to level 8, balancing thoroughness and speed. |
| `-sV --version-light` | `nmap [IP-ADDRESS] -sV --version-light` | Uses a lighter version detection mode for faster scanning. |
| `-sV --version-all` | `nmap [IP-ADDRESS] -sV --version-all` | Forces Nmap to use the most aggressive version detection. |
| `-A` | `nmap [IP-ADDRESS] -A` | Enables OS detection, version detection, script scanning, and traceroute. |

### **OS Detection**


| Option | Example | Description |
| --- | --- | --- |
| `-O` | `nmap [IP-ADDRESS] -O` | Detects the operating system of the host. |
| `-O --osscan-limit` | `nmap [IP-ADDRESS] -O --osscan-limit` | Limits OS detection to confirmed open and closed ports for faster scanning. |
| `-O --osscan-guess` | `nmap [IP-ADDRESS] -O --osscan-guess` | Makes an educated guess about the OS when detection is uncertain. |
| `-O --max-os-tries` | `nmap [IP-ADDRESS] -O --max-os-tries 1` | Limits the number of OS detection tries to speed up the process. |
| `-A` | `nmap [IP-ADDRESS] -A` | Enables comprehensive scanning, including OS detection, service version detection, script scanning, and traceroute. |

### **Timing and Performance**


| Option | Example | Description |
| --- | --- | --- |
| `-T0` | `nmap [IP-ADDRESS] -T0` | Paranoid timing: extremely slow, used to evade intrusion detection systems. |
| `-T1` | `nmap [IP-ADDRESS] -T1` | Sneaky timing: very slow, reduces the chance of detection. |
| `-T2` | `nmap [IP-ADDRESS] -T2` | Polite timing: limits bandwidth use to avoid network disruption. |
| `-T3` | `nmap [IP-ADDRESS] -T3` | Normal timing: balances speed and stealth, suitable for routine scans. |
| `-T4` | `nmap [IP-ADDRESS] -T4` | Aggressive timing: faster scanning that may be detected by modern IDS. |
| `-T5` | `nmap [IP-ADDRESS] -T5` | Insane timing: very fast but likely to be detected and can overload network resources. |

### **Timing and Performance**

| Option | Example | Description |
| --- | --- | --- |
| `-f` | `nmap [IP-ADDRESS] -f` | Fragments packets to help hide the scan from firewalls and intrusion detection systems. |
| `--mtu` | `nmap [IP-ADDRESS] --mtu 32` | Sets a custom MTU to fragment packets, helping to evade network filters. |
| `-D` | `nmap -D 192.168.1.101,192.168.1.102,192.168.1.103 192.168.1.1` | Uses fake IP addresses during the scan to make the real scanning source harder to detect. |
| `-S` | `nmap -S www.microsoft.com www.example.com` | Spoofs the scan’s source IP to mimic another host, confusing defenses that rely on IP recognition. |
| `-g` | `nmap -g 53 [IP-ADDRESS]` | Specifies a source port for scanning, which can help avoid detection by port-specific rules. |
| `--proxies` | `nmap --proxies http://[IP-ADDRESS]:[PORT], http://[IP-ADDRESS]:[PORT] [IP-ADDRESS]` | Routes the scan through proxies to hide the scanner's true IP and avoid network monitoring. |
| `--data-length` | `nmap --data-length 200 [IP-ADDRESS]` | Adds extra random data to the scan packets, disrupting detection by systems that recognize scan patterns. |

### **NSE Scripts**

| Option | Example | Description |
| --- | --- | --- |
| `-sC` | `nmap [IP-ADDRESS] -sC` | Runs default scripts for quick vulnerability scans and service identification. |
| `--script default` | `nmap [IP-ADDRESS] --script default` | Executes all default scripts to comprehensively assess network security. |
| `--script` | `nmap [IP-ADDRESS] --script=banner` | Captures service banners to identify software versions and potential vulnerabilities. |
| `--script` | `nmap [IP-ADDRESS] --script=http*` | Targets all HTTP-related scripts to explore web services in depth. |
| `--script` | `nmap [IP-ADDRESS] --script=http,banner` | Combines HTTP and banner scripts for detailed service analysis. |
| `--script-args` | `nmap --script snmp-sysdescr --script-args snmpcommunity=admin [IP-ADDRESS]` | Runs the 'snmp-sysdescr' script with specified arguments to retrieve system descriptions from SNMP-enabled devices. |

### **Output Options**

| Option | Example | Description |
| --- | --- | --- |
| `-oN` | `nmap [IP-ADDRESS] -oN normal.file` | Saves the scan results in a normal, human-readable format to the specified file. |
| `-oX` | `nmap [IP-ADDRESS] -oX xml.file` | Outputs the scan results in XML format, suitable for parsing by software or further processing. |
| `-oG` | `nmap [IP-ADDRESS] -oG grep.file` | Writes the scan results in a format easily usable with Unix grep command, ideal for automated processing. |
| `-oA` | `nmap [IP-ADDRESS] -oA results` | Saves the scan results in all available formats (normal, XML, and grepable) to the specified base filename. |
| `--append-output` | `nmap [IP-ADDRESS] --append-output` | Appends the results to existing files rather than overwriting them, useful for continuous scanning operations. |

1.  `ls /usr/share/nmap/scripts/ftp*`
2.  `nmap --script ftp-vsftpd-backdoor -p 21 10.129.159.8`
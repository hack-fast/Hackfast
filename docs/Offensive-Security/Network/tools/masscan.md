
### **Overview**

Masscan is a high-speed port scanner built for Internet-scale reconnaissance. It sends packets like Nmap but is much faster. Use it to map large networks, identify exposed services, and narrow targets before deeper scans.

### **Target Selection**

| Option | Example | Description |
|--------|---------|-------------|
| *(none)* | `masscan 192.168.1.10` | Scan a single IP address. |
| *(none)* | `masscan 192.168.1.10 192.168.1.20` | Scan multiple IPs separated by spaces. |
| *(none)* | `masscan 192.168.1.10-192.168.1.100` | Scan an IP range (inclusive). |
| *(none)* | `masscan example.com` | Resolve and scan a domain name. |
| *(none)* | `masscan 10.0.0.0/8` | Scan a subnet using CIDR notation. |
| `-iL` | `masscan -iL targets.txt` | Load a list of targets from a text file (one per line). |
| `--exclude` | `masscan --exclude 192.168.1.1` | Exclude specific IPs from scanning (useful to avoid internal or sensitive systems). |
| `--excludefile` | `masscan --excludefile exclude.txt` | Load exclusions from a file (one IP or CIDR per line). |


### **Scan Rate & Performance**

| Option | Example | Description |
|--------|---------|-------------|
| `--rate` | `masscan --rate 10000` | Controls packets per second. Start low for stealth. Raise for speed. Defaults to 100 packets/sec if not set. |
| `--top-ports` | `masscan --top-ports 100` | Scans the most commonly used 100 TCP ports based on global traffic data. Faster recon than full port scans. |
| `--max-rate` | `masscan --max-rate 5000` | (Alias) Same as `--rate`. Useful when scripting or integrating. |
| `--adapter-ip` | `masscan --adapter-ip 192.168.1.100` | Sets the source IP if scanning from a system with multiple interfaces or IPs. |
| `--adapter-port` | `masscan --adapter-port 40000` | Sets the source port range to use for packets. Helps avoid firewall detection or port conflicts. |


### **Port Scanning Options**

| Option | Example | Description |
|--------|---------|-------------|
| `-p` | `masscan -p80,443 10.0.0.1` | Scan specific ports on a host. Useful for targeting known services. |
| `-p` | `masscan -p0-65535 10.0.0.1` | Full TCP port scan. Use only when you need full visibility (can take longer). |
| `--ports` | `masscan --ports 21,22,23,80,443` | Alternate way to define ports. Can help when scripting. |
| `--banners` | `masscan --banners` | Attempt to grab service banners (like version strings). Requires open port and banner support. Slower, use with `--rate` throttling. |


### **Output Formats**

| Option | Example | Description |
|--------|---------|-------------|
| `-oJ` | `masscan -p80 1.2.3.4 -oJ output.json` | Saves results as JSON. Useful for parsing or automation. |
| `-oX` | `masscan -p80 1.2.3.4 -oX output.xml` | XML output, compatible with many tools (like Metasploit, Nmap GUIs, etc). |
| `--output-format list` | `masscan --output-format list` | Simple text format like `IP:port`. Useful for piping into other tools or quick review. |


### **Workflow Example**

```bash
# Fast recon of top 100 ports on a /16 subnet
masscan 192.168.0.0/16 --top-ports 100 --rate 5000 -oJ scan.json

# Targeted scan of known ports with exclusions
masscan -p22,80,443 -iL targets.txt --exclude 192.168.1.1 --rate 1000 -oX results.xml

# Full port scan on one host for deeper analysis
masscan 10.0.0.5 -p0-65535 --rate 2000 -oJ fullscan.json
```
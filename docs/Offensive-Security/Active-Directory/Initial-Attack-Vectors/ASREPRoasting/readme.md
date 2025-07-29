---
legal-banner: true
---

### **OVERVIEW**

ASREPRoast is a  attack that exploits users who lack the Kerberos pre-authentication required attribute. This vulnerability allows attacker to request authentication for a user from the Domain Controller (DC) without needing the user password. The DC then responds with a message encrypted with the user password-derived key, which attacker can attempt to crack offline to discover the user password.

### **MAIN REQUIREMENTS**

1.  Lack of Kerberos Pre-Authentication: Target users must not have this security feature enabled.
2.  Connection to the Domain Controller (DC): Attacker need access to the DC to send requests and receive encrypted messages.
3.  Optional Domain Account: Having a domain account allows attacker to more efficiently identify vulnerable users through LDAP queries. Without such an account, attacker must guess usernames.
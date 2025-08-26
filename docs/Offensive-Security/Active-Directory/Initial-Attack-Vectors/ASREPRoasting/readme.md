---
legal-banner: true
---

### **Overview**

ASREPRoast is an attack that exploits accounts without the **Kerberos pre-authentication required** attribute.This vulnerability allows an attacker to request authentication data for a user from the Domain Controller (DC) without knowing the user’s password.The DC responds with a message encrypted using a key derived from the user password.The attacker can then attempt to crack this response offline to recover the user’s password.  

### **Main Requirements**

1. **Lack of Kerberos Pre-Authentication**  
   Target accounts must have the *“Do not require Kerberos pre-authentication”* flag set.  

2. **Connection to the Domain Controller (DC)**  
   The attacker needs access to the DC to send authentication requests and receive encrypted responses.  

3. **Optional Domain Account**  
   Having a valid domain account allows the attacker to efficiently identify vulnerable users through LDAP queries.  
   Without such an account, the attacker must rely on guessing or enumerating usernames.  

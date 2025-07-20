1.  **SMB (Server Message Block)**
    
    - [ ] Look for any open shares.
    - [ ] Try to log in with the guest user.
    - [ ] Enumerate shared folders and their permissions.
    - [ ] Search for sensitive files within shares (e.g., configuration files, scripts, documents).

2.  **LDAP (Lightweight Directory Access Protocol)**
    - [ ] Check for accessible information without credentials.
    - [ ] Enumerate users, groups, and organizational units.
    - [ ] Retrieve detailed information about user accounts (e.g., attributes, password last set, account status).
    - [ ] Search for service principal names (SPNs).

3.  **Kerberos**
    - [ ] Brute force usernames.
    - [ ] Check for AS-REP roastable accounts.
    - [ ] Retrieve Kerberos tickets (TGT/TGS) and analyze them.
    - [ ] Check for accounts with unconstrained delegation.
    - [ ] Enumerate accounts with constrained delegation.
    - [ ] Extract and analyze service principal names (SPNs).

4.  **DNS (Domain Name System)**
    - [ ] Attempt zone transfer.
    - [ ] Brute force subdomains.
    - [ ] Enumerate DNS records to identify hosts and services.
    - [ ] Investigate reverse lookup zones for additional information.

5.  **RPC (Remote Procedure Call)**
    - [ ] Investigate anonymous access possibility.
    - [ ] Enumerate users and groups via RPC.
    - [ ] Query domain controllers for detailed information.
    - [ ] Check for accessible services and endpoints.
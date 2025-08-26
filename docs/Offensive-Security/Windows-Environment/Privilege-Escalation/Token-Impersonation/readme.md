---
legal-banner: true
---

### **What are tokens?**

Tokens are temporary keys that allow access to a system or network without re-entering credentials each time.  
Think of them as “cookies for computers.”

| Privilege | Impact | Execution |
| --- | --- | --- |
| **SeAssignPrimaryToken** | Admin | + Use tools like `potato.exe`, RottenPotato, or JuicyPotato to impersonate tokens and escalate to **NT SYSTEM**. <br>+ Leverage token manipulation with tools like **PrintSpoofer** or **RogueWinRM**. |
| **SeAudit** | Threat | + Write events to the Security event log using the `AuthzReportSecurityEvent` API to manipulate or overwrite logs. <br>+ Generate misleading events to hinder forensic analysis. |
| **SeBackup** | Admin | + Backup registry hives such as `HKLM\SAM` and `HKLM\SYSTEM` to extract local account hashes. <br>+ Reuse hashes for **Pass-the-Hash** attacks. <br>+ Read sensitive files while bypassing normal access controls. |
| **SeCreateToken** | Admin | + Create arbitrary tokens with elevated privileges (e.g., local admin) using system APIs. <br>+ Use token crafting to escalate privileges. |
| **SeDebug** | Admin | + Duplicate tokens of sensitive processes (e.g., `lsass.exe`) using debugging privileges. <br>+ Interact with `lsass.exe` to extract credentials. |
| **SeImpersonate** | Admin | + Use Potato exploits, **RogueWinRM**, or **PrintSpoofer** to create a process under another user’s context by impersonating their token. |
| **SeLoadDriver** | Admin | + Load a vulnerable kernel driver to escalate privileges. <br>+ Unload security-related drivers to weaken protections. |
| **SeRestore** | Admin | + Abuse this privilege to manipulate system files. <br>+ Replace binaries such as `utilman.exe` with `cmd.exe` to gain SYSTEM-level shells. <br>+ Replace service executables to maintain persistence. |
| **SeSecurity** | Threat | + Clear or shrink Security event logs to erase evidence. <br>+ Read logs for insights into system activity. <br>+ Flood with events to purge older entries. <br>+ Modify object SACLs to change auditing. |
| **SeShutdown** | Availability | + Shut down the system to disrupt availability. <br>+ Trigger BSOD and generate crash dumps for analysis. |
| **SeTakeOwnership** | Admin | + Take ownership of sensitive files or directories. <br>+ Modify ACLs to gain full access. <br>+ Replace system binaries (e.g., swap `cmd.exe` for `utilman.exe`). |
| **SeTcb** | Admin | + Manipulate tokens to include admin rights, enabling arbitrary token creation. <br>+ Use PoC code or tools from exploit repos to craft tokens. |
| **SeTrustedCredManAccess** | Threat | + Dump and access credentials stored in **Windows Credential Manager**. |
| **SeSystemEnvironment** | Unknown | + Manipulate UEFI variables via system calls to alter boot behavior. <br>+ Modify driver entries or other environment values. |
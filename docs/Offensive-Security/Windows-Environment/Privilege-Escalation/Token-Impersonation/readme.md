### **WHAT ARE TOKENS?**

Temporary Keys that allow you access to a system/network without having to provide credentials each time you access a file, Think of it as cookies for computers

| PRIVILEGE | IMPACT | EXECUTION |
| --- | --- | --- |
| SeAssignPrimaryToken | Admin | \+ Use tools like potato.exe, RottenPotato, JuicyPotato to impersonate tokens and escalate privileges to NT SYSTEM.  <br>\+ Leverage token manipulation techniques via tools like PrintSpoofer or RogueWinRM. |
| SeAudit | Threat | \+ Write events to the Security event log using the AuthzReportSecurityEvent API to manipulate audit logs or overwrite existing events.  <br>\+ Create misleading events to confuse forensic analysis. |
| SeBackup | Admin | \+ Backup registry hives such as HKLM\\SAM and HKLM\\SYSTEM to access local account hashes.  <br>\+ Extract and leverage these hashes for Pass-the-Hash attacks.  <br>\+ Alternatively, read sensitive files bypassing usual access controls. |
| SeCreateToken | Admin | \+ Create arbitrary tokens with elevated privileges, such as local admin rights, using system APIs.  <br>\+ Use token crafting techniques to escalate privileges. |
| SeDebug | Admin | \+ Duplicate tokens of sensitive processes like lsass.exe using debugging privileges.  <br>\+ Use tools to interact with and manipulate lsass.exe for credential extraction. |
| SeImpersonate | Admin | \+ Employ tools like the Potato family, RogueWinRM, or PrintSpoofer to create a process under another user's context by obtaining a handle to their token. |
| SeLoadDriver | Admin | \+ Load and exploit a buggy kernel driver to escalate privileges.  <br>\+ Use techniques to unload security-related drivers for further exploitation. |
| SeRestore | Admin | \+ Launch processes with SeRestore privilege to manipulate system files.  <br>\+ Replace system binaries like utilman.exe with other executables (e.g., cmd.exe) to gain system-level access.  <br>\+ Replace service binaries to maintain persistence. |
| SeSecurity | Threat | \+ Clear or shrink the Security event log to hinder forensic analysis.  <br>\+ Read the Security event log for insights on system and user activity.  <br>\+ Generate numerous events to purge old ones and cover tracks.  <br>\+ View and modify object SACLs to alter auditing settings. |
| SeShutdown | Availability | \+ Use system shutdown commands to disrupt operations.  <br>\+ Invoke system calls to cause an immediate BSOD and create memory dumps for analysis. |
| SeTakeOwnership | Admin | \+ Take ownership of critical system directories and files.  <br>\+ Modify access control lists to gain full access.  <br>\+ Replace system binaries to gain elevated access (e.g., renaming cmd.exe to utilman.exe). |
| SeTcb | Admin | \+ Manipulate tokens to include local admin rights, enabling the creation of arbitrary tokens with elevated privileges.  <br>\+ Utilize sample code and executables available in security repositories to craft tokens. |
| SeTrustedCredManAccess | Threat | \+ Access and dump credentials stored in the Windows Credential Manager. |
| SeSystemEnvironment | Unknown | \+ Manipulate UEFI variables using system calls to alter system boot behavior and settings.  <br>\+ Use specific system calls to modify driver entries and other system environment values. |
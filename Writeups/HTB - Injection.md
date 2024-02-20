---
type: info
scope: []
aliases: []
related: []
created: 24-04-2023 14:19
---
%%
related::
%%

#todo #writeup

## Overview
Injection is a **Linux** box.

- Topics
	- Privilege escalation
	- 

- Tools
	- `nmap`
	- `linpeas`
	- `searchsploit`
	- privesc
		- `ps aux | grep root | egrep -v "\[.*\]"`
		- `find / -perm -4000 -ls 2>/dev/null`
		- `find / -group staff -ls 2>/dev/null`

## Methodology
- `nmap` Port scan
	- discover ssh, http
- won't start ssh brute without finding username first
- http is vulnerable to directory traversal
- 


## Learning Points
- Access dates in Windows executables are important to know ___

## References
1. 
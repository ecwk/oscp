---
type: info
scope: []
aliases: []
related: []
created: 23-04-2023 03:29
---
%%
related::
%%

#todo #writeup

## Overview
Jarvis is a **Linux** box

- tools used
	- [[Tool]]

## Methodology
- nmap scan
- feroxbuster http
	- found php sql thingy
- explore website
	- found sql injectable query
	- get sql user database password
	- crack with hashcat
- access web interface admin
	- find searchsploit shell for that ui
	- gain initial shell
- sudo -l
	- pepper can execute py script
	- run script in context of pepper to gain his shell
	- run `find / -perm -4000 -ls 2>/dev/null` to find SUDO or SUID executables and use GTFOBin to escalate privileges


## Learning Points
- Access dates in Windows executables are important to know ___

## References
1. 
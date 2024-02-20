---
type: info
os: Linux
scope: [Forensics, Default Credentials, Brute-force]
created: 21-04-2023 22:57
---
%%
tools:: [[hydra]]
tools:: [[searchsploit]]
tools:: [[feroxbuster]]
tools:: [[dd]]
tools:: [[dcfldd]]
tools:: [[df]]
tools:: [[seclist]]
tools:: [[scp]]
tools:: [[binwalk]]
tools:: [[testdisk]]
tools:: [[xxd]]
tools:: [[grep]]
tools:: [[strings]]
%%

#todo #writeup

## Overview
Mirai is a **Linux** box

## Methodology
- nmap
- discover lighttpd, plex media server
- use searchsploit to find vulns for services, but none work
- run feroxbuster on port 80 service
	- `feroxbuster --url http://<IP> --status-codes 200`
	- discover ``/admin`
	- it is 
- try ssh with raspberry pi default creds `pi:raspberry`
	- success shell
- alternatively if never enum service, could've just started of ssh brute-force with default creds `/usr/share/seclists/Passwords/Default-Credentials/ssh-betterdefaultpasslist.txt`
- `/root/root.txt` is false, says that it is on usb stick
	- run `df -h` to discover mounted devices
	- usb is `/dev/sdb` mounted at `/media/usbstick`
	- `/media/usbstick` does not show flag
	- its probably deleted, however, possible to recover the deleted file because usbstick does not write zeros delete, meaning file is deleted in file system but still physically exists (orphaned/unallocated)
	- to view deleted files
		- recovery tool (not 100% success rate)
			- `dcfldd if=/dev/sdb | gzip | dcfldd of==out.gz`
			- `scp pi@<ip>:~/out.gz ./`
			- `binwalk -Me out.gz`
				- doesnt work
			- `testdisk out.gz`
				- doesnt work (root.txt found but is empty)
		- `xxd /dev/sdb | grep -v "0000 0000 0000" | grep -v "ffff ffff ffff"` 
		- `cat`
		- `strings`
		- `cat /dev/sdb | grep -a "[a-zA-Z0-9]\{30,\}" -A2 -B2`

## Learning Points
- Always feroxbuster on http
- Learn to scp
- Learn that raw viewing binary is good skill
- Learn how to use disk viewing tools and disk imaging tols
- Instead of brute-force default, the moment you understand what OS is running, lookup the default credentials

## References
1. https://www.youtube.com/watch?v=SRmvRGUuuno&ab_channel=IppSec
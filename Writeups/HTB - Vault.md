---
type: info
os: OS
scope: []
created: 02-05-2023 03:43
---
%%
tools::
%%

#todo #writeup

## Notes
- www-data through php5 upload
- ssh
	- `dave:Dav3therav3123`
```bash
www-data@ubuntu:/home/dave/Desktop$ ls
Servers  key  ssh
www-data@ubuntu:/home/dave/Desktop$ cat ssh
dave
Dav3therav3123
www-data@ubuntu:/home/dave/Desktop$ cat key
itscominghome
www-data@ubuntu:/home/dave/Desktop$ cat Servers
DNS + Configurator - 192.168.122.4
Firewall - 192.168.122.5
The Vault - x
www-data@ubuntu:/home/dave/Desktop$

dave@ubuntu:/home/alex$ cat ./Desktop/.
./             ../            .root.txt.swp
dave@ubuntu:/home/alex$ cat ./Desktop/.root.txt.swp
rootubunturoot.txtdave@ubuntu:/home/alex$

```

- DNS server
	- 192.168.5.0/24 via 192.168.122.5 dev ens3
	- `dave:dav3gerous567`
	- `ip -6 n`
		- fe80::5054:ff:fec6:7066
		- fe80::5054:ff:fee1:7441 




## Overview
{description}

## Methodology
1. Step one
2. Step two

## Learning Points
- the moment find directory in web, run directory enum again
- need to learn how to automate filie extension upload fuzzing
	- [[ffuf]]
- SSH port forwarding is crucial for pivoting
- if sit behind firewall, might have poor firewall rules.
	- in this case, the server sits behind a firewall
	- the firewall blocks all source ports except 53 and 4444, probably because it wants to allow the server behind the firewall to receive DNS requests and whatever port 4444 is for
	- however, this also allows a hacker to change their source port to 53 for any communication request and access services behind the firewall
- Bypassing firewalls
	- change source port
	- try ipv6
- Get more comfortable without network protocols and tools
	- ssh
		- also ssh port forwarding (static and dynamic)
	- scp
	- proxychains
	- socks5
	- `ip`
	- ipv4 & ipv6
		- discovering ipv6 addresses using multicast address
		- good for some poorly configured firewalls who block only ipv4 addresses

## References
1. 
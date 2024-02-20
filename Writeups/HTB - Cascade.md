---
type: info
os: Windows
scope: []
created: 20-04-2023 14:41
---
%%
related::
%%

#todo #writeup

## Overview
Cascade is a **Windows Active Directory** box.

- Tools used
	- [[nmap]]
	- [[rpcclient]]
	- [[ldapsearch]]

## Methodology
- `nmap` scan
- Attempt anonymous bind on services
	- [!] [[Server Message Block (SMB)|smb]]
	- [+] [[Remote Procedure Call (RPC)|rpc]], [[Lightweight Directory Access Protocol (LDAP)|ldap]]

- Use `ldapsearch` to enumerate users
	- discover anomalous attribute `cascadeLegacyPwd` with password for user ``
`ldapsearch -x -H ldap://10.129.93.110 -s sub -b "DC=cascade,DC=local" | grep ".*:" | cut -d " " -f1 | sort -u`
- Run bloodhound
- Cant remote (according to bloodhound)
- Is member of share group
- Check share
- Discover VNC in /Data share
- Crack s.smith password VNC
- Discover html document saying that a deleted user `tempadmin` has password same as `administrator`
- s.smith in group of Audit share
	- Audit share contains exe .net
	- Contains DB
	- DB contains username password of arksvc account but password is encrypted
	- .bat file shows that database file is passed into .exe function
- Decompile with dnspy
	- reveals that the password from db file will be decrypted using aes
	- set breakpoint and show the decrypted password variable is password
- remote with arksvc
- discover that arksvc has ad recycling bin group membership
- use `Get-ADObject` to find deleted tempadmin password in `cascadeLegacyPwd` then `echo <password> | base64 -d` to gain admin access


- How to solve
	1. ldapsearch find weird legacy password thingy
- ldapsearch can use the following to find weird attribute keys
- `ldapsearch -x -H ldap://10.129.93.110 -s sub -b "DC=cascade,DC=local" | grep ".*:" | cut -d " " -f1 | sort -u`
- Password
	- grep -i pwd
	- pwd
	- passwd
	- pass
	- basically rmb combinations to grep, esp long data like ldapsearch
- Filtering LDAP keys with uniq and sort
	- that weird `cascadeLegacyPassword` key
- learn google if find hash, even if weird like vnc hash crack
- [[Privileged Groups Windows#2. AD Recycle Bin]] find deleted password
- Debugging .NET with dpsync
	- Adding breakpoints
	- Alternatively, use CyberChef to decrypt
- `whoami /all`

```bash
sudo mount -t cifs -o 'user=<user>,password=<password>' //<ip>//<share> /mnt/<name>
```


## Learning Points
- Learning to use `Get-ADObject`
	- `-IncludeDeletedObjects`
		- 

## References
1. https://www.youtube.com/watch?v=mr-fsVLoQGw&ab_channel=IppSec

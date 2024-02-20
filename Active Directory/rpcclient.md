---
ports: [135]
type: tool
---

#todo #tool 

- Used for [[Server Message Block (SMB)|SMB]] queries and commands through

- `rpcclient -U "DOMAIN/USER" <ip>`
	- then use tab auto-complete
- Main commands to know
	- `query___`
	- `enum___`
- For automated enumeration use these instead
	- [[CrackMapExec]]
	- [[enum4linux]]

## Anonymous Login
![[Pasted image 20230311072218.png]]

- SMB may allow anonymous logins
	- In SMB V1, anonymous/guest login is enabled by default
	- Admins have to manually disable, even after update
	- However, fresh new installs of SMB have it disabled by default

## Others
Attempting to get Password Policy info with command: rpcclient -W 'WORKGROUP' -U''%'' '10.129.95.210' -c "getdompwinfo" 2>&1  
- Get domain password info

## References
- https://book.hacktricks.xyz/network-services-pentesting/pentesting-smb/rpcclient-enumeration
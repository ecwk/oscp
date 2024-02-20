#tool #todo 

- `/usr/share/doc/python3-impacket/examples`
	- Or `locate impacket | grep python`
- Multiple enumeration tools for various network protocols

- impacket-rpcdump
	- Only dumps information about rpc endpoint but does not enumerate, as compared to [[enum4linux]] and [[CrackMapExec]] (SMB RPC)
- impacket-getNPUsers
	- If no usersfile
		- Uses LDAP to enumerate userlist
		- wont work if LDAP doesn't allow anonymous user, else requires a domain account who can access ldap
	- If usersfile
		- doesn't request LDAP 
		- directly start Kerberod Request

## References
- https://www.kali.org/tools/impacket/
- https://www.kali.org/tools/impacket-scripts/
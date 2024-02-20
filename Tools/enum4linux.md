---
ports: [135, 139, 445]
type: tool
---

#tool 

**enum4linux** is a [[Server Message Block (SMB)|SMB]] and [[Remote Procedure Call (RPC)|RPC]] enumeration tool

- Wraps around networking command-line tools
	- [[smbclient]]
	- [[rpcclient]]
	- [[net]]
	- [[nmblookup]]
- Displays command executed with verbose `-v` flag

## Example
`enum4linux -a -M -l -d -v <IP>`

## Similar
- [[CrackMapExec]]

## References
- https://www.kali.org/tools/enum4linux
- https://github.com/CiscoCXSecurity/enum4linux
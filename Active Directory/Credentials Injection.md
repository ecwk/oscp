#active-directory #enumeration #method

**Credential injection** involves loading credentials into a Windows shell.

- **Prerequisite** for [[Active Directory (AD)]] enumeration
	- AD access requires domain credentials
- **Requires**
	- AD Network access - has a device within network (may be rogue device)

## runas.exe
- `runas.exe` - built-in Windows tool
	- Injects credentials into shell memory
- `/netonly` option will cause all subsequent network connections to use injected credentials
	- Does not authentiate with DC initially
	- Hence, If password is wrong will not give error until network connection is made
	- E.g. can use `runas.exe` to connect to SMB share as another user
		- [[Server Message Block (SMB)#SYSVOL|Accessing SYSVOL]]
- Example command
	- `runas /netonly /user:DOMAIN\USERNAME cmd.exe`
	- `runas /netonly /user:sg.example.com\admin01 cmd.exe`

Authentication Server (AS) Response (REP) Roast is an attack targetting [[Kerberos]] misconfigurations.

- Kerberos Misconfig
	- User account does not require pre-auth
		![[Disable Kerberos Preauthentication.png]]
		*From [here](https://learn.microsoft.com/en-US/troubleshoot/windows-server/identity/useraccountcontrol-manipulate-account-properties#:~:text=2097152-,DONT_REQ_PREAUTH,-0x400000)*
	- [[Kerberos#^56cabd|KRB_AS_REQ]] does not require password hash to encrypt its contents
		![[Kerberos ASREPRoast Diagram.png|400]]
	- KDC AS will still send the KRB_AS_REP
		- Contains content encrypted with password hash which
		- can be brute-forced offline

Technically this process is still secure as the TGT is still encrypted with the user's password. A hacker can't just make a KRB_AS_REQ to get a TGT further request for TGS without the user's password.

However, hacker can 

The reason this is allowed is for applications that can not do pre-auth.

## Prerequisites
- Either
	- List of potential usernames
	- List of users without preauth

- [[PowerSploit]]
	- `Get-DomainUser -PreauthNotRequired -verbose`
- IMPacket
	- Get hash

## References
- https://software.intel.com/sites/manageability/AMT_Implementation_and_Reference_Guide/default.htm?turl=WordDocuments%2Fintroductiontokerberosauthentication.htm
- https://www.thehacker.recipes/ad/movement/kerberos/asreproast
- https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/asreproast
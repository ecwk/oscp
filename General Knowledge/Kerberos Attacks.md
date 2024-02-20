---
type: info
scope: []
aliases: [Kereros Brute-forcing, ASRepRoast, Kerberoast, Pass the Hash, Over Pass the Hash, Pass the Key (PTK), Pass the Ticket (PTT), Silver Ticket, Golden Ticket]
related: []
created: 21-04-2023 12:30
---
%%
related::
%%

#todo 

## Overview

## References
- https://www.tarlogic.com/blog/how-to-attack-kerberos/
- https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/over-pass-the-hash-pass-the-key
- https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/pass-the-ticket
- https://gist.github.com/TarlogicSecurity/2f221924fef8c14a1d8e29f3cb5c5c4a

- Pass the hash (NTLM)
	- Usually after kerberoast or asreproast, will have NTLM hash
	- This just passes the NTLM hash to NTLM auth processs
	- If NTLM disabled, wont work
- Overpass the hash / Pass the key
	- Use the hash to generate a KRB_AS_REQ to then obtain the TGT
	- With TGT, can access services by requesting TGS

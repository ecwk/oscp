- [ ] ---
type: info
scope: []
ports: [53]
services: [kerberos]
aliases: [krb]
related: []
created: 12-03-2023 20:54
---
%%
related:: [[ASREPRoast]]
%%

#todo 

## What is Kerberos
- Authentication protocol
	- Default for [[Active Directory (AD)]]
	- Supersedes [[NetNTLM]]
- Involves 3 systems
	- Client (requester)
	- Kerberos (responder) - trusted third-party
	- Service (target)
- Authenticates client and service through Kerberos

## Structure
- Components
	1. **Key Distribution Center (KDC)**
	2. **Authorisation Server (AS)**

- can be used to enum users
	- when auth with username, if user dont exist kerberos will say "user no exist", else will say username is wrong


KRB_AS_REQ ^56cabd

## Auth
- Terminology
	- Key Distribution Center (KDC)
		- Consists of the AS and TGS
	- Authentication Server (AS)
	- Ticket Granting Server (TGS)
	- Ticket
		- main component used in Kerberos authentication, think JWTs in webdev
	- Ticket Granting Ticket (TGT)
		- used by client to request for TGS
	- Service Ticket (ST)
		- used by client to request access to service
	- `krbtgt`
		- the account running the KDC service
	- Principal
		- an entity involved in authentication
	- Principal Name
		- a standardised format that uniquely identifies a principal
		- `$primary/$instance@EXAMPLE.COM`
		- Primary - the name of the service or user
		- Instance - the FQDN of the host running the service
		- Realm - the domain in AD
		- e.g. user `bob@EXAMPLE.COM`
			- users omit the **instance** portion
		- e.g. SQL service `MSSQLSvc/sql.example.com:1433@EXAMPLE.COM`
		- e.g. KDC service `krbtgt/EXAMPLE.COM@EXAMPLE.COM`
			- KDC is the only service that places the realm in the instance field, indicating that KDC is logically "owned" by the domain even though it is most likely running on a domain controller like `dc1.example.com`

1. KRB_AS_REQ (Keberos Authentication Server Request)
	- **User** Principal (name of user)
		- e.g. `bob@EXAMPLE.COM`
	- **KDC** Principal (name of KDC service)
		- e.g. `krbtgt\EXAMPLE.COM@EXAMPLE.COM`
	- **Timestamp**
		- timestamp prevents replay attack
		- **encrypted** with **user** hash prevents confidentiality breach
	- If `DONT_REQ_PREAUTH`, timestamp not required, TGT will be given
1. KRB_AS_RES (Kerberos Authentication Server Response)
	- TGT
		- encrypted with `krbtgt` hash
			- ensures 
	- Session key encrypted with user hash
		- This is the part where ASRepRoast happens


## References
- https://www.tarlogic.com/blog/how-kerberos-works/

- **Directory services** (DS) are databases that store and maintain data about users and resources in a network.
	- The term "Directory" is coined by the hierarchical nature that resources are stored
		- e.g. users stored in groups by departmen

- Active Directory (AD) is a DS
- Domain networks are networks where all entities are interconnected and managed by a central system
	- AD uses domain controllers (DCs) for this
- AD is used for Identity and Access Management (IAM)
- IAM vs IDM vs IDMS
	- Auth0, Okta
- AD is a logical collection of information of resources (users, computers, servers, in a network
	- Uses the AD schema that describes rules of objects that can be stored in AD as well as their attributes
- AD resources can be anything, e.g users, groups, printers, servers, volumes, and services (like email - Microsoft Exchange)

- Purpose
	- Provides a central managagment solution to manage network resources
**Without AD**, administrators would have to **individually modify each device** in a network which does not scale well with more complex networks. A common use case of AD is to quickly manage and synchronise user credentials (**workgroup**) of workstations, allowing a user to login with the same set of credentials in any workstation.

The majority of large companies use Active Directory because it allows for the control and monitoring of their user's computers through a single domain controller. It allows a single user to sign in to any computer on the active directory network and have access to his or her stored files and folders in the server, as well as the local storage on that machine. This allows for any user in the company to use any machine that the company owns, without having to set up multiple users on a machine. Active Directory does it all for you.
# Domain Structure
- A domain has a **root domain name**
	- e.g. `apple.com`
- A child domain shares the root domain
	- e.g. `sg.apple.com`, `us.apple.com`
	- domains with the same root domain are in the same tree
- The domain at the top of three is the **root domain**
	- e.g. `apple.com`
	- only has the root domain name
- Domains in the same tree automatically create **trust** between them by AD
	- Trust allows domains to **access resources** in other domain given correct access control
- Two domains with different root domain names will form two trees
	- e.g. `apple.com` & `samsung.com`
	- Multiple trees create a forest

## AD Components
- Global Catalog (GC) contains basic information about all objects in a forest for quick indexing
- Domain Controller (DC) 
- DCs
	- Windows server with Active Directory Domain Services (AD DS) installed and
	- Promoted to domain controller in the forest
	- Contain, control, and manage AD objects
	- Handles authentication, authorisation, and accounting services
	- Handles replication with other domain controllers in the forest
	- Provides admin with capabilities to manage domain resources
	- Stores data in AD DS data store
		- Contains `NTDS.dit` - a database storing all information of an AD DC as well as password hashes for domain users
		- All information stored in `%SystemRoot%\NDTS\NTDS.dit`
		- Accessible only by the domain controller

- Forests, Trees, Domains
	- Forests contains trees
	- Trees contains a hierarchy of domains
	- Domains contain, group, and manage AD objects
	- Organisational Units (OUs) are containers for AD objects
	- Objects are users, groups, computers, services (printers, emails, shares), and servers
	- Domain services - DNS server, LLMNR, IPv6
	- Domain schema - rules for object creation and attributes
- Users and Groups
	- Default users - `Administrator` and `Guest`
	- Types of users
		- `Domain Admin` - only ones access to DC to control domains
		- `Service Accounts` (Can be `Domain Admins`) - These are for the most part never used except for service maintenance, they are required by Windows for services such as SQL to pair a service with a service account
		- `Local Administrators` - These users can make changes to local machines as an administrator and may even be able to control other normal users, but they cannot access the domain controller
		- `Domain Users` - These are your everyday users. They can log in on the machines they have the authorization to access and may have local administrator rights to machines depending on the organization.
	- Default groups
		- ![[THM - AD Default Security Groups.png]]
	- Groups make it easier for admins to set permissions for users and objects
		- AGDLP22
		- Two types of AD groups
			1. **Security** Groups - permissions for users in a group
			2. **Distribution** Groups - specify service distribution lists, such as e-mail. This group is not that useful for hackers but can be used for enumeration
- Trusts
	- Maintain security inside AD network
	- Restrict and control communications between domains, trees
	- Can be extended outside of the forest i.e. domains can communicate with external domains outside its current forest
	- By default, all domains in the same forest have trust relations i.e. can communicate with each other
	- Two types of trust
		1. **Directional** - The direction of the trust flows from a trusting domain to a trusted domain i.e. only concerns A and B
		2. **Transitive** - The trust relationship expands beyond just two domains to include other trusted domains 
			- Transitive trusts are two-way trust relationships that are established between domains in a single forest. In a transitive trust, two domains trust each other, and the trust relationship is transitive, meaning that any domains that are trusted by those domains are also trusted. For example, if Domain A trusts Domain B, and Domain B trusts Domain C, then Domain A also trusts Domain C, and users in Domain A can access resources in Domain C.
	- Hackers can abuse this trust to move laterally throughout the network
- Policies
	- Domain policies - apply to whole domain as compared to domain groups which only apply to users in that group. Additionally, it can be thought of rules rather than permissions, e.g. disable Windows Defender across all devices in the domain
	- Created using Group Policy Objects (GPOs)
- Domain Services
	- AD DS is a service that contains the core functions of an AD network
	- Manages the domain, security certificates, LDAPs, and more
	- Many services AD DS can be configured with
	- Default services
		- Lightweight Directory Access Protocol (LDAP) - provides communication between applications and directory services
		- Certificate Services - allows the DC to create, validate, and revoke public key certificates
		- DNS, LLMNR, NBT-NS - Domain Name Services for identifying IP hostnames
- Domain Authentication
	- Important part and also a vulnerable part of AD that hackers can exploit
	- Two main types of authentication
		1. Kerberos - default authentication service for AD, uses ticket-granting tickets and service tickets for user authentication and authorisation across the domain
		2. NTLM - default Windows authentication protocol using encrypted challenge/response protocol

- Domain admins control the domain
- Enterprise admins control the forest
- NTLM
- Keberos
- GPOs
- LDAP
- SAM Windows
- ## AD Authentication
- User credentials stored in DC
- AD authentication is the process of the DC verifying user login request with stored credentials using either two protocols
	- **Kerberos** - Default protocol in newer domains
	- **NetNTLM** - Legacy protocol kept for compatibility purposes
- Though obsolete, NetNTLM is still enabled in most networks for compatibility reasons


### Kerberos
- https://www.youtube.com/watch?v=5N242XcKAsM&ab_channel=DestinationCertification
- Users logging into domain receive a ticket as proof of authentication
	- Ticket presented to services to determine if user has been authenticated and can use the service

**Steps**
![[THM - Kerberos 1.png]]
![[THM - Kerberos 2.png]]
![[THM - Kerberos 3.png]]

1. User creates payload containing username (unencrypted) and a timestamp encrypted with the a derived key from thed input password (may be wrong). Then, it is sent to the Key Distribution Center (KDC) on the DC
2. KDC reads the unencrypted username and looksup the password in the AD database, derives the key from the password, and decrypts the timestamp. If decrypted, the KDC will send back a **Ticket Granting Ticket (TGT)** and a **session key**.
	- TGT allows user to request additional tickets (usually TGS) to access specific services
	- TGT allows TGS request without the user constantly keying in their credentials each time they connect to a service
	- The session key is required to generate further requests by the user
	- Note in the below diagram, both TGT and Session Key are encrypted using the user's password hash and the `krbtgt` password hash
	- The TGT should not be readable by the user, hence, it is encrypted using `krbtgt` hash, only the KDC can decrypt it
	- The TGT includes the session key as part of its contents, hence, the KDC does not need to store the session key
3. Request TGS
- To access services, a user will have to request for a **Ticket Grantint Service (TGS)** from the KDC
- TGS is provided to a service that proves a client is authenticated to use it
- TGS only used per service
- Request Payload
	- Username & Timestamp (encrypted with `Session Key`)
	- TGT (encrypted with `krbtgt`)
	- Service Principal Name (SPN) - uniquely identifies a service
4. TGS Response
- KDC decrypts TGT using key derived from `krbtgt` account's password hash
- KDC takes `Session Key` from TGT
- If KDC can decrypt the Username & Timestamp using the Session Key, it is proved that client is request is valid
- TGS is encrypted with the password hash from the user account running the service, as identified by the SPN
- TGS only can be decrypted by the service owner (not the user) hence more secure
- Then, the service owner can extract the service session key to verify if the requesting user actually has the correct service session key
- Response Payload
	- TGS + Service Session Key (encrypted with service owner's password hash)
	- Service Session Key
5. Authenticate to the Service
- Request Payload
	- Username & Timestamp (encrypted with `Service Session Key`)
	- TGS (encrypted with `<service_owner>` password hash)
- Service owner will use its configured account's password hash to decrypt the TGS and obtain the service session key
- Then, it will attempt to decrypt the username & timestamp with the key, if successful, the user proven to be authenticated

### NetNTLM
![[THM - NetNTLM Authentication.png]]

1.  The client sends an authentication request to the server they want to access.
2.  The server generates a random number and sends it as a challenge to the client.
3.  The client combines their NTLM password hash with the challenge (and other known data) to generate a response to the challenge and sends it back to the server for verification.
4.  The server forwards the challenge and the response to the Domain Controller for verification.
5.  The domain controller uses the challenge to recalculate the response and compares it to the original response sent by the client. If they both match, the client is authenticated; otherwise, access is denied. The authentication result is sent back to the server.
6.  The server forwards the authentication result to the client.

Note that the user's password (or hash) is never transmitted through the network for security. It uses a HMAC function

**Note:** The described process applies when using a domain account. If a local account is used, the server can verify the response to the challenge itself without requiring interaction with the domain controller since it has the password hash stored locally on its SAM.

## PowerShell
- `. .<name>.ps1` to load ps1 module
- `powershell -ep bypass` to bypass execution policy and allow ps1 modules to run
- `Select-Object`
	- `Select`
- `Where-Object`
	- `?`, `Where`
	- `-Match`

```powershell
Add-Content -Path $PROFILE "Set-PSReadlineKeyHandler -Key Tab -Function MenuComplete"
```


## PowerSploit
- https://github.com/PowerShellMafia/PowerSploit
- https://powersploit.readthedocs.io/en/latest/
- `PowerView` module for AD enumeration and exploitation

## Attacks
- Pass the ticket
- Pass the hash
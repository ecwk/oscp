## Initial Access
- Breaching aka Initial Access
	- Obtain initial set of credentials or
	- Initial shell
- Many AD services & features, large attack surface
- Once access, can begin enumeration or privesc if needed
- Initial access either through public-facing service or planting a rogue device on the organisation's network
- OSINT and Social Engineering (Phishing) are common ways to gain initial access
	-   Users who ask questions on public forums such as [Stack Overflow](https://stackoverflow.com/) but disclose sensitive information such as their credentials in the question.
	-   Developers that upload scripts to services such as [Github](https://github.com/) with credentials hardcoded.
	-   Credentials being disclosed in past breaches since employees used their work accounts to sign up for other external websites. Websites such as [HaveIBeenPwned](https://haveibeenpwned.com/) and [DeHashed](https://www.dehashed.com/) provide excellent platforms to determine if someone's information, such as work email, was ever involved in a publicly known data breach.

## NetNTLM Authenticated Services
- Potential NetNTLM Authenticated Services
	- Mail servers
	- RDP services
	- VPN services
	- Web Applications
- Can brute-force to gain initial access
	- Use Python
	- Use hydra
	- Use metasploit
	- Or any other automated tools

## LDAP Bind Credentials
![[THM - LDAP Authentication.png]]

- Lightweight Directory Access Protocol (LDAP) is used by non-Microsoft devices that integrate with AD, such as:
	- Gitlab
	- Jenkins
	- Custom-developed web applications
	- Printers
	- VPNs
- LDAP Auth is similar to NetNTLM, however, auth occurs directly on the LDAP application which stores a pair of AD credentials
	- AD credentials can be recovered from the LDAP application to then gain access to AD
	- LDAP also susceptible to brute force like in NTLM
- E.g., if gain foothold to a server, such as a Gitlab server, you can read the config file to recover the AD credentials as they are often stored in plaintext because the LDAP AD Bind auth security model relies on the secure storage location of config files rather than its contents

### LDAP Pass-back Attacks
- A common attack against LDAP AD network devices, such as printers
	- Done when you have gained initial access into the internal network, maybe you have planted a rogue device into a school campus
- Client LDAP such as printers usually use default credentials e.g. `admin:admin` and store  them,but they are protected and can't be read or is hidden
- However, they are always sent to an LDAP server for authentication
- Hacker can intercept and modify the LDAP request or redirect (LDAP directory referrals) it to a malicious LDAP server
- Then, can capture and recover the LDAP credentials
- Protection
	- Don't allow LDAP redirects
	- Only allow trusted LDAP servers
- Use `ncat` to see raw request after modiftying printer settings
- Use `slapd` to set up rogue LDAP server
	- May have to configure to support less secure authentication protocols as LDAP requests won't get sent if the min authentication protocol is too secure
	- PLAIN and LOGIN are weak protocols
	- Use `ldapmodify` and `.ldif` file
	
```ldif
#olcSaslSecProps.ldif

dn: cn=config
replace: olcSaslSecProps
olcSaslSecProps: noanonymous,minssf=0,passcred
```

- Then, use tcpdump to listen for traffic on the ldap port
	- `tcpdump -SX -i <interface> port 389`
	- `-SX` display in string `-S` and hex `-X`

## Other Authentication Relays
- AD has many other authentication relays that can be exploited
	- Relays are similar to LDAP and require a server to authenticate and relay requests
	- Hackers can setup malicious LDAP servers instead

### Sever Message Block (SMB)
- Used in Windows for communication
	- E.g. file sharing, remote administration, printer notifications (like "out of paper")
- Port 139 and 445
- Older legacy versions are infamously vulnerable to RCE, however many adminstrators have yet to update SMB to secure versions for compatibility reasons
- Two common exploits for SMB using NetNTLM authentication
	- Intercept the NTLM challenge and brute-force crack the hash offline
		- Offline brute-force is fast, but this is much slowly than cracking the NTLM password hash directly
	- MITM attack to relay SMB authentication between client and server, effectively providing hacker with an authenticated session and access to the target server


- https://book.hacktricks.xyz/generic-methodologies-and-resources/pentesting-network/spoofing-llmnr-nbt-ns-mdns-dns-and-wpad-and-relay-attacks
	- Microsoft systems use Link-Local Multicast Name Resolution (LLMNR) and the NetBIOS Name Service (NBT-NS) for local host resolution when DNS lookups fail.
 
### Responder
`Responder` is a tool for poisoning Windows-based networks using NTLMv1/NTLMv2/LMv2, Extended Security NTLMSSP and Basic HTTP authentication. It posions LLMNR, NBT-NS, and WPAD requests (automatically poisons LLMNR, NBT-NS, or WPAD requests when detected). Can also act as a rogue authentication server for HTTP/SMB/MSSQL/FTP/LDAP.

- Local DNS Resolution
	- Link-Local Multicast Name Resolution (LLMNR) is a protocol used in Microsoft Windows systems to resolve the NetBIOS name of a network-connected device to its IP address, typically in small, single-subnet networks. (Used over NBT-NS)
	- NetBIOS Name Service (NBT-NS) is a legacy protocol used to resolve NetBIOS names to IP addresses on a local network. NBT-NS is often used in conjunction with SMB for file and printer sharing.
	- Web Proxy Auto-Discovery (WPAD) is a protocol used to automatically discover the URL of a proxy configuration file on a network. WPAD is typically used in corporate environments to simplify the configuration of web proxies for network clients.
- Allows clients to perform DNS in their LAN rather than overloading DNS servers
- Clients send out the aforementioned requests (broadcast or multicast) to check if the service they are looking for is on the LAN, if service available, it will respond to this request. This can be intercepted and poisoned by `Responder`
- Responder will actively listen to the requests and send poisoned responses telling the requesting host that our IP is associated with the requested hostname. By poisoning these requests, Responder attempts to force the client to connect to our AttackBox. In the same line, it starts to host several servers such as SMB, HTTP, SQL, and others to capture these requests and force authentication.
- `Responder` has to win the "race"
	- Race refers to responding to request before the legitimate server does
- After getting hashes, use `hashcat` to crack them

![[THM - NTLM MiTM.png]]

- Relaying is possible with `Responder` under several conditions
	- SMB signing is disabled or enabled but not enforced. Else, cannot modify request and forge the message signature as server would reject it
	- The request account needs relevant permissions, ideally accounts with administrative privileges over the serverr, as this would allow to gain a foothold
	- To determine this account privileges, you can:
		- Guess (maybe from username) what accounts will have what permissions on which hosts
		- If already breached AD, can perform initial enumeration first before doing NTLM hash relay
	- Not determining this is known as a **blind relay**
		- Ideally, first breach AD to enumerate and get information on account privileges
		- Then, perform lateral movement for privesc across the domain
		- However, its still good to fundamentally understand how a relay attack works
	- Relaying different from above because time is not spent decrypting the password hash, just passing the hash to impersonate the user and access their resources

## Automated Deployment
- Manually deploying and configuring OS is slow
	- Potentially insecure if using DVDs or USBs
- Microsoft Deployment Toolkit (MDT) and System Center Configuration Manager (SCCM)
- MDT automates Microsoft OS deployment
	- System admins deploy OS more efficiently because can manage base images in central location
	- E.g. can pre-install software like Office365 and anti-virus
- SCCM manages updates for all Microsoft applications, services, and OSs. 
	- Can test in sandbox before deploying updates domain-wide
- Difference - MDT is used for new/initial deployments to preconfigure and manage boot images but SCCM is post-deployment management and updates (patch management)
- However, anything that provides central management is a critical weakpoint in systems and can compromise large portions of the network

### MDT PXE Boot
![[THM - MDT PXE Boot Process.png]]

- MDT can be configured in different ways, each with different vulnerabilities
- MDT Preboot Execution Environemnt (PXE) Boot is one such configuration
	- New network devices can install OS directly over network connection
	- Usually done in combi with DHCP - when leased an IP, the host is allowed to request for PXE boot images and start OS installation
- Important part of PXE boot installation is `8`. A hacker can
	1. Inject privesc vector, such as Local Administrator account, to gain admin access to OS once PXE boot completes
	2. Perform password scraping to recover AD credentials used during install
- Recovering Credentials from PXE Boot Image
	- Exfiltrate stored credentials
	- Inject local admin when image boots
	- When image boots, the machine will join the domain, i.e. have access to domain
- [[PowerPXE]] can be used to extract information from insecure PXE boot

## Configuration Files
- Once foothold, important to gain even more information (enumeration), such as for configuration files
- [[Seatbelt]] can check for useful configuration file data for hackers

## Mitigations
- Literally overly large attack surface for AD: config files, NTLM MiTM, offline password NTLM hash cracking, social engineering & phishing, OSINT, brute-force login pages, LDAP bind, authentication relays, and much more
- Hence, best to create a enumeration methodology and continously update it as you learn new stuff
	- AKA this obsidian project!
- Mitigations
	- User awareness and training - The weakest link in the cybersecurity chain is almost always users. Training users and making them aware that they should be careful about disclosing sensitive information such as credentials and not trust suspicious emails reduces this attack surface.
	- Limit the exposure of AD services and applications online - Not all applications must be accessible from the internet, especially those that support NTLM and LDAP authentication. Instead, these applications should be placed in an intranet that can be accessed through a VPN. The VPN can then support multi-factor authentication for added security.
	- Enforce Network Access Control (NAC) - NAC can prevent attackers from connecting rogue devices on the network. However, it will require quite a bit of effort since legitimate devices will have to be allowlisted.
	- Enforce SMB Signing - By enforcing SMB signing, SMB relay attacks are not possible.
	- Follow the principle of least privileges - In most cases, an attacker will be able to recover a set of AD credentials. By following the principle of least privilege, especially for credentials used for services, the risk associated with these credentials being compromised can be significantly reduced.

## Privilege Escalation
## Lateral Movement
## Goal Execution
- NTLM is a suite of security protocols
	- NetNTLM is one of them, used for authentication using challenge-repsonse based scheme
	- Services using NTLM can be Internet exposed (potential initial access)
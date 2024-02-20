---
type: info
scope: [overview]
aliases: [dns]
ports: [53]
services: [dns]
---

#todo 

- Resolves domain names to IP addresses
	- Good for human readability
- Hierarchical 
	- as in levels of domains, e.g. child.parent.tld

## Overview

### DNS Servers
- Main types
	1. Primary
		- Contains authoritative records - *INSERT EXPLANATION AUTH RECORDS*
	2. Secondary
		- Copy of Primary DNS records
		- Used for redundancy
		- Periodically updates its records with primary
- Others
	1.  Recursive DNS server
		- This is a DNS server that is responsible for resolving DNS queries from clients that do not have a cached copy of the DNS record. The recursive DNS server performs a recursive search through the DNS hierarchy until it finds the authoritative DNS server for the requested domain.
	2.  Caching DNS server
		- This is a DNS server that temporarily stores DNS records in its cache to reduce the time it takes to resolve future DNS queries for the same domain. This can improve the overall performance of DNS resolution and reduce the load on authoritative DNS servers.
	3.  Forwarding DNS server
		- This is a DNS server that forwards DNS queries to another DNS server, usually a recursive DNS server. The forwarding DNS server does not perform recursive searches itself but simply passes the query on to the specified DNS server.
	4.  Authoritative DNS server
		- This is a DNS server that is responsible for maintaining the original copy of DNS records for a particular domain. When a client sends a DNS query for a domain, the authoritative DNS server is the final authority on the DNS records for that domain.
			- If not the auth server, will use other protocols to resolve DNS query, e.g. recursion
- Common records
	- SOA (State of Authority) - states which DNS server is the primary
	- NS (Name Server) - lists DNS servers in the domain (primary and secondary)
	- SRV (Service) - contains services in a domain, compared to A records, SRV records are more detailed and useful for clients trying to find and use domain services

## DNS Enumeration
- Before enumeration, must reverse lookup IP address
	- [[nslookup]]
		- `nslookup <target IP> <dns IP>`
	- Note however, there are cases where IP addresses can be used instead of domain names
- Then, use dns enumeration tools
	- Manual enumeration
		- [[nslookup]]
		- [[dig]]
	- Generic information enumeration
		- [[dnsrecon]]
	- In-depth enumeration (brute-force)
		- [[dnsenum]]
	- Subdomain enumeration (for public domains/websites) using search engines
		- [[sublist3r]]
	- Email to non-existent account
		- Simply sending an email message to a nonexistent address at a target domain often reveals useful internal network information through a _nondelivery notification_ (NDN).
		- obtains sensitive information throuhg non-delivery/bounce message - a message from email that says email is not delivered
			![[Pasted image 20230312070719.png]]
			*Note*. From [hacktricks](https://book.hacktricks.xyz/network-services-pentesting/pentesting-dns#mail-to-nonexistent-account)

## Potentital Exploits
### Zone Transfer Exploit
- Zone transfer uses either
	1. All Zone Transfer (AXFR)
	2. Incremental Zone Transfer (IXFR)
- Misconfigured primary DNS zone transfer
	- Primary DNS allows any secondary DNS of any IP address to initiate AXFR 
	- Allows hackers to zone transfer i.e. replicate sensitive primary DNS records to their own secondary DNS servers
- Tools to test this
	- [[dig]]
	- [[dnsrecon]]
		- `dnsrecon -d <target domain> -t axfr`
	- [[Information Gathering/nmap#Scripts|nmap]] scripts
		- automated by [[AutoRecon]]


## References
- https://www.cloudflare.com/learning/dns/what-is-dns/
- https://book.hacktricks.xyz/network-services-pentesting/pentesting-dns
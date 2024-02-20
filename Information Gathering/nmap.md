#tool 

## Steps
![[nmap Steps.png|400]]

1. Enumerate Targets
	- User-defines IPs to discover
2. Host Discovery
	- Use communication protocols to determine if a host is live or not, e.g. `icmp`, `arp`, `tcp/udp`
	- Refered to as `state`, either `open`, `closed`, `filtered`, `unfiltered`
3. Reverse-DNS Lookup
	- Map IPS to domain names using DNS
4. Scan Ports

### 1. Target Enumeration
- Specify to `nmap` which hosts you want to scan
- 4 Methods
	- **List** (space separated) e.g. `google.com bing.com yahoo.com`
	- **Range** (inclusive) e.g. `192.168.0.50-60`, `10.10.0-255.101-125`
	- **Subnet** (CIDR notation) e.g. `192.168.0.0/24`
	- **File** (line separated) `-iL hosts.txt`
- `-sL`, scan list, lists all the IP addresses that will be scanned
	- by default, nmap attempts DNS resolution which takes time
	- `-n` used in combination to skip DNS resolution

### 2. Host Discovery
- Used to determine if the [[#1. Target Enumeration|enumerated target]] is online or offline
- Different network protocols for different scenarios
	- E.g. ARP, ICMP, TCP/UDP
	- ARP -> OSI Layer 2 - Data Link Layer
	- ICMP -> OSI Layer 3 - Network Layer
	- TCP/UDP -> OSI Layer 4 - Transport Layer
- `-Pn`,  no ping, to skip host discovery and immediately begin port scanning
- `-sn`, scan only, to only enable host discovery and not proceed with port scanning

**Address Resolution Protocol (ARP)**
- `-PR`, ping ARP, used when scanner machine is in the same subnet as host machine
	- if scan targets outside subnet, router will drop packet
	- [[arp-scan]] can also be used for arp host discovery

**Internet Control Message Protocol (ICMP)**
- Prefer ARP > ICMP when same network because many firewalls block ICMP ping requests
- `-PE` echo ping `Type8`
- `-PP` timestamp ping `Type13`, may not be blocked compared to echo ping
- `-PM` address mask ping `Type17`, alternative if other ICMP types are blocked

**Transmission Control Protocol (TCP)**
- `-PS` TCP SYN ping
	- defaults port 80
	- one `-PS21`
	- range `-PS21-25`
	- csv `-PS80,443,8080`
	- privileged users don't have to complete the 3-way TCP handshake but unprivileged users do, i.e. its faster to run as `sudo`
- `-PA` TCP ACK ping
	- options and defaults similar to `-PS` 
	- must run as `sudo`
- `-PU` UDP ping
	- Verifies host status through rejection, unlike the previous 2 TCP
	- If online, no response
	- If online, host sends ICMP Type 3, Code 3: Destination Unreacheable (Port Unreachable)

**Reverse-DNS  Lookup**
![[Reverse-DNS Lookup Info.png]]

## 3. Port Scanning
- `nmap` port states
	1. `open` - a service is listening on the port
	2. `closed` - no service is listening on the port but port is **accessible**
	3. `filtered` - cannot determine `open` or `closed` because port is **inaccessible**. Either firewall block (inbound) nmap from reaching port or outbound response is blocked
	4. `unfiltered` - cannot determine `open` or `closed` but port is **accessible**. Only occurs with ACK scan `-sA`
	5. `open|filtered` - cannot determine whether port is `open` or `filtered`
	6. `closed|filtered` - cannot determine whether a port is `closed` or `filtered`

**Port Scanning Methods**
- `-sT` - TCP connect scan
	- completes the TCP 3-way handshake
	- then, immediately sends `RST`
	- non-sudo can run this method
	- if response `SYN, ACK` , port is `open`
	- if response `RST`, port is `closed`
	- if no response, port is `filtered`
- `-sS` - TCP 
	- doesn't complete TCP 3-way handshake
	- immediately sends `RST` once target sends `SYN, ACK`
	- `open`, `closed`, and `filtered` ports are determined same as `-sT`
- `sU` - UDP
	- doesn't need any handshake because its UDP
	- open ports don't reply
	- closed ports reply ICMP Type 3, Code 3
	- hence, open ports are determined if there is no reply, unlike the previous 2 scan types
	- only this method can get the port states `open|filtered` and `closed|filtered`
	- much slower than TCP scans

**Port Scanning Options**
- `-F` - fast scan
	- default scans top 1000 ports (not sequentially 1 to 1000)
	- fast scan scans **top 100** ports
- `--top-ports <n>` scan top `n` ports
- `-r`
	- scans ports in sequential order
	- e.g. `-p 20,22,80,443` will be scanned in numerical ascending order
- `-p` - port selection
	- either one, multiple (csv), or range
	- e.g. `-p22`, `-p20,22,80,443`, `p20-25`
	- special value `-p-` will scan all 65535 ports
- -`T<0-5>` timing template
	- paranoid (0) - super slow, 5 minutes per probe
	- sneaky(1) - used IRL, avoid IDS detection
	- polite (2) - *not stated* 😠
	- normal(3) - default
	- aggressive(4) - used during CTF, but is detectable IRL
	- insane(5) - inaccurate because of packet loss
- `--min-rate <n>` - packets per second
- `--max-rate <n>`
- `--min-parallelism <nProbes>` - probes happening simulatenously.
	- divides IPs into groups and probes each in parallel
	- not talking about ports
- `--max-parallelism <nProbes>`

## 4. Fingerprinting

## Scripts
- `locate *.nse | grep smb`
- Scan first then use script for each service, or customise the default scripts
	- Do version/protocol/discovery/info scan
	- Then, do vulnerability scan
- Or use **categories**
	- `safe` is best for OSCP

### Categories
`auth`

These scripts deal with authentication credentials (or bypassing them) on the target system. Examples include `x11-access`, `ftp-anon`, and `oracle-enum-users`. Scripts which use brute force attacks to determine credentials are placed in the `brute` category instead.

`broadcast`

Scripts in this category typically do discovery of hosts not listed on the command line by broadcasting on the local network. Use the `newtargets` script argument to allow these scripts to automatically add the hosts they discover to the Nmap scanning queue.

`brute`

These scripts use brute force attacks to guess authentication credentials of a remote server. Nmap contains scripts for brute forcing dozens of protocols, including `http-brute`, `oracle-brute`, `snmp-brute`, etc.

`discovery`

These scripts try to actively discover more about the network by querying public registries, SNMP-enabled devices, directory services, and the like. Examples include `html-title` (obtains the title of the root path of web sites), `smb-enum-shares` (enumerates Windows shares), and `snmp-sysdescr` (extracts system details via SNMP).

`dos`

Scripts in this category may cause a denial of service. Sometimes this is done to test vulnerability to a denial of service method, but more commonly it is an undesired by necessary side effect of testing for a traditional vulnerability. These tests sometimes crash vulnerable services.

`exploit`

These scripts aim to actively exploit some vulnerability. Examples include `jdwp-exec` and `http-shellshock`.

`external`

Scripts in this category may send data to a third-party database or other network resource. An example of this is `whois-ip`, which makes a connection to whois servers to learn about the address of the target. There is always the possibility that operators of the third-party database will record anything you send to them, which in many cases will include your IP address and the address of the target. Most scripts involve traffic strictly between the scanning computer and the client; any that do not are placed in this category.

`fuzzer`

This category contains scripts which are designed to send server software unexpected or randomized fields in each packet. While this technique can useful for finding undiscovered bugs and vulnerabilities in software, it is both a slow process and bandwidth intensive. An example of a script in this category is `dns-fuzz`, which bombards a DNS server with slightly flawed domain requests until either the server crashes or a user specified time limit elapses.

`intrusive`

These are scripts that cannot be classified in the `safe` category because the risks are too high that they will crash the target system, use up significant resources on the target host (such as bandwidth or CPU time), or otherwise be perceived as malicious by the target's system administrators. Examples are `http-open-proxy` (which attempts to use the target server as an HTTP proxy) and `snmp-brute` (which tries to guess a device's SNMP community string by sending common values such as `public`, `private`, and `cisco`). Unless a script is in the special `version` category, it should be categorized as either `safe` or `intrusive`.

`malware`

These scripts test whether the target platform is infected by malware or backdoors. Examples include `smtp-strangeport`, which watches for SMTP servers running on unusual port numbers, and `auth-spoof`, which detects identd spoofing daemons which provide a fake answer before even receiving a query. Both of these behaviors are commonly associated with malware infections.

`safe`

Scripts which weren't designed to crash services, use large amounts of network bandwidth or other resources, or exploit security holes are categorized as `safe`. These are less likely to offend remote administrators, though (as with all other Nmap features) we cannot guarantee that they won't ever cause adverse reactions. Most of these perform general network discovery. Examples are `ssh-hostkey` (retrieves an SSH host key) and `html-title` (grabs the title from a web page). Scripts in the `version` category are not categorized by safety, but any other scripts which aren't in `safe` should be placed in `intrusive`.

`version`

The scripts in this special category are an extension to the version detection feature and cannot be selected explicitly. They are selected to run only if version detection (`-sV`) was requested. Their output cannot be distinguished from version detection output and they do not produce service or host script results. Examples are `skypev2-version`, `pptp-version`, and `iax2-version`.

`vuln`

These scripts check for specific known vulnerabilities and generally only report results if they are found. Examples include `realvnc-auth-bypass` and `afp-path-vuln`.

## References
- https://github.com/jasonniebauer/Nmap-Cheatsheet
- https://tryhackme.com/module/nmap
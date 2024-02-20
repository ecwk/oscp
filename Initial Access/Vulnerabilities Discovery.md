 - Vulnerability is a weakness or flaw in design of a system or application, allowing attackers to exploit them to gain access to unauthorised resources or perform unauthorised actions
 - Common Types
	 - Operating Systems (OS)
	 - Misconfiguration
	 - Weak or Default Credentials
	 - Poor Application Logic or Design
	 - Social Engineering / Human Factor
- Common Scoring Systems
	- Common Vulnerability Scoring System (CVSS)
	- Vulnerability Priority Rating
- Good Databases
	- VulnDB
	- ExploitDB
	- NIST NVD
- **Foothold** is an access vector to a system's console

## Searchsploit
`searchsploit` uses ExploitDB to find vulnerabities
- `-u` update offline database

- `searchsploit <search>` to find vulnerabilities
- options
	- `-p <id>` more information
	- `-w <search>`  search and provide ExploitDB url
	- `-m <id>` get exploit code

## Metasploit Framework
- 3 Components
	- `msfconsole` - the CLI
	- modules - scanners, exploits, payloads, etc
	- tools - msfvenom, meterpreter, etc
- Terminologies
	- **Vulnerability** is a weakness or design flaw in a system
	- **Exploit** is code that uses a exploits a vulnerability
	- **Payload** are codes that use exploits to be executed on a target
- Metasploit modules
	1. `auxiliary` - supporting modules, such as scanners, crawlers, and fuzzers
	2. `encoders` - encode exploit and payloads so they are less likely to be detected by antivirus
	3. `envasion` - try to evade detection from antivirus
	4. `exploits`
	5. `nops` - no operation, used to make CPU do nothing for one cycle and are often used as a buffer to achieve consistent payload sizes
	6. `post` - post-exploitation modules
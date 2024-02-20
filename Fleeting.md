- Windows enumeration
	- SMB - port 145
		- `smbclient` enumeration
		- *need to research how to use `smbclient`!*
		- listing
		- accessing smb shares
		- what are smb shares
		- what is ipc share
		- how to access share as another user
		- maybe can setup smb
	- nmap script (e.g. smb run)
	- RPC - port 135 
		- null login

## Post-exploitation
- Tools
	- `SILENTTRINITY`
		- HTB Jerry (win)
			- https://www.youtube.com/watch?v=PJeBIey8gc4&ab_channel=IppSec
	- `EMPIRE`
		- HTB Blue (win) 
			- https://www.youtube.com/watch?v=YRsfX6DW10E&ab_channel=IppSec
			- https://www.youtube.com/watch?v=efajeZN-sPI&ab_channel=HackerSploit
		- HTB Optimum (win)
			- https://www.youtube.com/watch?v=kWTnVBIpNsE&ab_channel=IppSec
			- https://www.youtube.com/watch?v=gDMGU3rdwfw&ab_channel=HackerSploit
	- `PowerSploit`


### Information Gathering

- `net localgroup administrators` check admin

**msfconsole**
- `session -u <sessionid>` upgrade to meterpreter

**meterpreter**
- `sysinfo` to check if system version has what vulns
- `getuid` see if has admin rights
- `background` session to select post exploitation modules
- good post modules
	- `suggester` to help detect privesc vulnerabiltiies, especially if you're not familiar with OS versions and their exploitsuse 

**shell (windows)**
- `systeminfo` a hellalota info
- `net` command to find out other users
	- `net user` list users
	- `net user <username>` get user info

## Questions
- How do I upload powershell scripts through the shell to the target
	- How do I do this raw (not meterpreter)
	- How do I do this through meterpreter
	- `EMPIRE`, `Sherlock`

## Notes
- use `suggester` to quickly find post exploits
- use version scanners to more accurately detect service versions
	- but if cant use metasploit, should create notes on how to access services to determine their version

## Linux Enumeration
- `locate` command to locate files quickly
	- use update sth command to update the locate database
- `find` to find strings in files

## nmap Automation
- autorecon
- masscan?

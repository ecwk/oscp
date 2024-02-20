#todo 

Security Identifiers (SIDs) identify a [[Security Principal]] or [[Security Group]]

- Security Principal (SP)
	- An entity that can be authenticated by a system or network (e.g. AD)
	- e.g. user account, service account, computer account, or a process running in the security context of a user or computer account (like services like MySQL server run my bobTheAdmin)
- Security Group
	- Group SPs to allow easier management of permissions
- All SPs have a unique SID
- SID format
	- `S-1-5-21-<domain>-<rid>`
	- Prefix (constant) - S-1-5-21
	- RID is only for objects created in the domain
	- The domain itself doesnt have RID

## Relative Indentifier (RID)
- When new SP is created, they are assigned an SID based on the domain's SID, as shown above

## Reference
- https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-identifiers
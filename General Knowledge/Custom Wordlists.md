---
type: info
scope: []
aliases: []
related: []
created: 14-03-2023 18:54
---
%%
related::
%%


#todo 

## Overview
- Rule-based
	- use info on target
	- e.g. current date, dob, names, company

## With Hashcat
- Use rules
	- toggle rules - capitalisation
- Ensure use `| sort -u` to remove duplicate fields
- If known password min length
	- `awk 'length($0)'`
- Notes
	- Windows usernames are not case-sensitive
	- Usually firstname lastname -> `john smith -> jsmith`

## References
- 
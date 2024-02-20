---
type: info
scope: []
aliases: []
related: []
created: 03-05-2023 00:44
---
%%
related::
%%

#todo 

## Overview
- Some commands run with cron can be manipulated to run other commands as another user
	- apt update, pre-invoke script
	- put script into `/etc/apt/apt.conf.d`
		- name must start with 2 digits, lower higher priority
			- e.g. `00pwn`
	```bash
	#!/bin/bash
	APT::Update::Pre-Invoke {"<dangeours command here>"}
	```

## References
- 
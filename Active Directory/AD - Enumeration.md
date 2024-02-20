#active-directory/phases #enumeration 

![[THM - AD Pentesting Phases.png]]

**Enumeration** is done after establishing an gaining access to the [[Active Directory (AD)]] domain. It helps subsequent phases in the [[AD - Pentesting Workflow]].

The goals of enumeration are to:
- Understand domain structure
- Find misconfigurations and vulnerabilities

## Enumeration Techniques
```dataview
TABLE file.etags AS "Tags"
FROM "Active Directory"
WHERE
  contains(file.tags, "active-directory") AND
  contains(file.tags, "method") AND
  contains(file.tags, "enumeration")
```

## Enumeration Tools
```dataview
TABLE file.etags AS "Tags"
FROM "Tools"
WHERE
  contains(file.tags, "active-directory") AND
  contains(file.tags, "enumeration")
```

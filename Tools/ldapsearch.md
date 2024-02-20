	---
ports: [389, 636, 3268, 3269]
os: linux
---
%%
[[Tools]]
%%

#tool/recon #todo 

**ldapsearch** is used establish a connection with an LDAP server, bind to it, and perform a search with filters.

> [!tip]+ Prerequisites
> A good understaning of [[Lightweight Directory Access Protocol (LDAP)]] is recommended.

- **Scope** refers to the depth of an LDAP search
	- base - returns attributes of that object
	- one - returns one-level depth from the object
	- sub(tree) - returns all objects below
	- children - returns all immediate objects i.e. children

## Options

| Option                               | Description                                                                    |
| ------------------------------------ | ------------------------------------------------------------------------------ |
| `-h <IP>`, `-h 10.10.64.128`         | Set host IP to search                                                          |
| `-H <URI>`, `-H ldap://10.10.64.128` | Set host URI to search                                                         |
| `-x`                                 | Use [[Lightweight Directory Access Protocol (LDAP)#^57e10f\|Simple Bind Auth]] |
| <code>-s base\|one\|sub\|children</code>         | Set scope                                                                      |
| `-b <DN>`                            | Set base DN for search i.e. location the search starts                                                             |

> [!warning]-
> `-h` may not work on newer versions of **ldapsearch**

## Examples

| Example                                                                | Description    |
| ---------------------------------------------------------------------- | -------------- |
| `ldapsearch [...options] <search> <...selected-fields>`                                     | Standard usage |
| `ldapsearch -x -H ldap://10.10.64.128 -s base -b "" "(objectClass=*)"` | Get            |
| `ldapsearch -x -H ldap://10.10.64.128 -b "DC=apple,DC=local" "(&(objectClass=Person)(name=*mark*))"`          |                |

## References
- https://devconnected.com/how-to-search-ldap-using-ldapsearch-examples/
- https://ldap.com/ldap-filters/
- http://www.ldapexplorer.com/en/manual/109010000-ldap-filter-syntax.htm
- https://confluence.atlassian.com/kb/how-to-write-ldap-search-filters-792496933.html
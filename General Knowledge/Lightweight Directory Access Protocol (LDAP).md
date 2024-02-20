---
type: info
scope: []
aliases: [ldap]
related: []
created: 20-04-2023 23:05
---
%%
related::
%%

#todo 


## Overview

LDAP is a protocol that provides a standardised interface for applications to interact with  [[Directory Service (DS)]], such as [[Active Directory (AD)]], ApacheDS, OpenLDAP, and so forth.

- **Important**
	- is a protocol not an implementation
	- exists many implementations like [[Active Directory (AD)|AD]] and `OpenLDAP`
- Protocol defines
	- How data is queried (LDAP filters)
	- How data is stored (hierarchy DNs)
	- How data is created (following schema rules)

- [ports::389] (LDAP)
- [ports::636] (LDAPSecure over SSL)
- [ports::3268] (GC)
- [ports::3269] (GCSecure over SSL)

### Terminology

| Term                        | Description                                                                |
| --------------------------- | -------------------------------------------------------------------------- |
|                             |                                                                            |
| Entry                       | Data stored in LDAP                                                        |
| Attribute                   | Entries each have many attributes (key-value pairs)                        |
| Data Information Tree (DIT) | The way entries are stored hierarchicially (tree)                          |
| Distinguished Name (DN)     | The unqiue identifier of an entry compromising of its location in the tree |
|                             |                                                                            |

## LDAP Data Components

- Data stored in LDAP -> **Entries**, *or objects in [[Active Directory (AD)|AD]]*
- Entry is a collection of **attributes**
	- LDAP specification defines official attributes, however, LDAP implementations can add their own such as AD's `sAMAccountName` attribute
- Common default attributes:
	- `namingContexts`
		- Used to identify the available subtree root DNs
		- e.g. `DC=example,DC=com`
	- `objectClass`
		- specifies type of object, e.g. `user`, `group`, `computer`, `organizationalUnit`
			- [!] IDK if the above examples are standard, need check
	- `distinguishedName`
		- The unique identifier, i.e. fully qualified domain name
		- e.g. `CN=Bob,OU=Sales,DC=example,DC=com`
- Each objectClass value has its own predefined attributes
- Default objectClass attributes
	- `user`
		- `fasd`
	- `computer`
- Entries can be displayed in **LDIF** (LDAP Data Interchange Format)
```ldif
# LDIF example:

dn: CN=Bob Ross,OU=Users,OU=Singapore,DC=example,DC=com
objectClass: top
objectClass: person
cn: Bob Ross
sn: Ross
givenName: Bob
sAMAccountName: b.ross

```
- DIT
	- Entries can be logically grouped
	- e.g. users are part of the OU group `CN=user1,OU=users,DC=example,DC=com`
- DN components
	- Domain Component (DC) - identifies domains
	- Organizational Unit (OU) - identifies containers
	- Common Name (CN) - identifies objects
	- Surname (SN) - identifies entities (not commonly used in AD; CN used instead)

**How Entries are Created**
- Attributes are created using special syntax
```
attributetype ( 2.5.4.41 NAME 'name' DESC 'RFC4519: common supertype of name attributes'
        EQUALITY caseIgnoreMatch
        SUBSTR caseIgnoreSubstringsMatch
        SYNTAX 1.3.6.1.4.1.1466.115.121.1.15{32768} )
```
- Attributes are
	- multi-valued by default and can be defined multiple times
	- `SINGLE-VALUE` flag can be added to attributetype to indicate that it can be only declared once per entry
- `objectClass` 
	- define what attributes **must** or **can** be set in an entry
	- two types: **Structural** and **Auxiliary**
	- Each entry **must** have one structural and may have 0 or more auxiliary object classes
	- Structural are used to create an entry
	- Auxiliary are used to add additional attributes
```
objectclass ( 2.5.6.6 NAME 'person' DESC 'RFC2256: a person' SUP top STRUCTURAL
  MUST ( sn $ cn )
  MAY ( userPassword $ telephoneNumber $ seeAlso $ description ) )
```
- Schemas are essentially a collection of objectclass and attribute definitions in a file


## LDAP Authentication
- Simple Bind Authentication ^57e10f
- Simple Authentication and Security Layer (SASL)
- Anonymous Authentication

## LDAP Queries
- LDAP is like database
	- Has its own language like SQL, but is much harder to understand because not meant for humans
- Filters
	- `(key=value)`
		- special chars: wildcard (\*)
	- Conditionals
		- `&`, `|`, `!`
		- `($CONDITIONAL(key=value)(key=value))`
	- Examples
		- `(username=bob)`, `(&(username=bob)(objectClass=user))`
 
- Teminology (DN, DC, CN, OU, etc)
	- distinguished name (DN) is a unique identifier for an object
	- subtrees are used to split up objects
	- Root DSE (Directory Service Entry) contains metadata about the LDAP server and its supported features
		- queried when the search base is set at the root (no distinguished name, its DN is basically no DN, it is assumed that root is being queried when no base DN is supplied)

![[Pasted image 20230419213902.png]]

- Common attributes
	- AD-specific
		- `sAMAccountName`
			- SAM account name of object
		- naming contexts have 3 default
			- `Domain`
			- `Configuration`
			- `Schema`
		- `servicePrincipalName`
		  - SPN, one object may have multiple, indicates which service this account is running, useful for kerberoasting
- HTB Challenges
	- `Phonebook`



## References
- https://www.digitalocean.com/community/tutorials/understanding-the-ldap-protocol-data-hierarchy-and-entry-components
- https://ldap.com/
- https://www.youtube.com/watch?v=0FwOcZNjjQA&ab_channel=TalentedDeveloper

---
type: tool
scope: []
aliases: []
related: []
created: 14-03-2023 13:04
---
%%
related::
%%

#todo 

## Overview

- create wordlists
	- htb sauna
- offline crack hashes

## Methodology
1. [[#Indentify Hash]]
2. [[#Crack Hash]]

### 1. Indentify Hash
- `hashid`
- `hash-identifier`
- Example Hashes
	- `hashcat --example-hashes`
	- https://hashcat.net/wiki/doku.php?id=example_hashes

### 2. Crack Hash
- Command
	- `hashcat -m {mode} -`
- Optimisation
	- `-force` - ignore warnings, also forces use GPU
	- `-w {1-4}` - higher is faster
- Attack modes
	- `-a {0,1,3,6,7}`
	- **0** Straight (*default*) - Wordlist entries are attempted without modification
	- **1** Combination - Wordlist entries are combined in all combinations
		- https://hashcat.net/wiki/doku.php?id=combinator_attack
		- Can use one or more dictionaries
	- **3** Brute-force - No wordlist, every single combination unless with a mask
		- mask specifies characters to use

## Rule-based Attacks (Custom Wordlists)
- Rule-based bruteforce uses information on target to generate custom wordlists
- Custom rules
	- https://github.com/NotSoSecure/password_cracking_rules

## References
- 
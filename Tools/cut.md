---
type: string-manipulation
os: linux
---
%%
[[Tools]]
%%

#linux #tool #util

**cut** is used to remove sections of text and output the remaining.

## Usage

```bash
cut OPTION ... [FILE] ...
```

- Input via
	- `[FILE]`
	- `|`
- Without `[FILE]`
	- cut will read from stdio

## Options
| Option                               | Description                                                                 |
| ------------------------------------ | --------------------------------------------------------------------------- |
| `-d<char> -f<int[]> [-s]`            | Specify delimiter and field. `-s` prevent lines without delim from printing |
| `-f<range>`, `-f-5`, `-f1-2`, `-f5-` | Specify range. Prints field start-5, 1-2, 5-last (incl.)                    |
| `--output-delimiter=<char[]>`        | Print different delimiter                                                   |
| `--complement`                       | Print unselected sections                                                   |

## Examples
| Example                         | Description                               |
| ------------------------------- | ----------------------------------------- |
| `cut -d: -f1 /etc/passwd`       | Print first field of ":" separated file   |
| <code>cat /etc/passwd \| cut -d: -f1</code> | Same as above but with piping             |
| `cut -d " " -f1,2`                | Print fields of space separated file |

## References
- https://man7.org/linux/man-pages/man1/cut.1.html
- https://bencane.com/2012/10/22/cheat-sheet-cutting-text-with-cut/
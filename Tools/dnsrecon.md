---
ports: [53]
---

#todo #tool 

- port 53/udp
- Enumerates DNS servers
	- Queries DNS records
	- Useful for getting more information about devices and domains in an internal network
- Hence, DNS enumeration is often not useful for OSCP, should leave to last
	- Because OSCP will not have hidden resources
- Like [[dnsenum]], supports dictionary brute-force
	- Should use this instead dnsrecon brute-force ineffective

```bash
dnsrecon -d example.com -D /usr/share/wordlists/dnsmap.txt -t std --xml dnsrecon.xml
```

## References
- https://www.youtube.com/watch?v=EpfHhEGz35I&ab_channel=HackerSploit
- https://digi.ninja/projects/zonetransferme.php
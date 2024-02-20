## Copy-paste
```bash
# 1. Quick scan TCP
nmap -vv -Pn -T4 --reason -oN quick-tcp-nmap.txt <IP>

# 2. Full scan TCP
nmap -vv -Pn -T4 -A -sV -sC -O -p- --reason --version-all -oN full-tcp-nmap.txt <IP>

# 3. Top 100 scan UDP
nmap -vv -Pn -T4 -sU -A --top-ports 100 --reason -oN top-100-udp-nmap.txt <IP>

# 4. Scan service
sudo nmap -sS -Pn -n -T4 --script safe -p<PORT> -oN <PORT>-scripts-nmap.txt <IP>
```

## Ports
```dataview
TABLE WITHOUT ID rows.file.link AS Name, port AS Ports
WHERE port > 0 OR port[0] > 0
GROUP BY port
```

## Host Discovery
Host discovery is used to detect which hosts are online and can be port scanned.

| Scan Type           | Example Command                               |
|:------------------- |:--------------------------------------------- |
| ARP                 | `sudo nmap -PR -sn 192.168.0.0/24`            |
| ICMP Echo (default) | `sudo nmap -PE -sn 192.168.0-1.0-255`         |
| ICMP Timestamp      | `sudo nmap -PP -sn 192.168.0.20,192.168.0.21` |
| ICMP Address Mask   | `sudo nmap -PM -sn 10.10.241.5/24`            |
| TCP SYN             | `sudo nmap -PS22,80,443 -sn 192.168.0.0/24`   |
| UDP                 | `sudo nmap -P53,161,162 -sn 192.168.0.0/24`   |

| Option | Description                                      |
| ------ | ------------------------------------------------ |
| `-n`   | no DNS lookup                                    |
| `-R`   | reverse-DNS lookup for all hosts (incl. offline) |
| `sn`   | host discovery only, skip port scanningg         |

*Note.* More information available at [[Information Gathering/nmap]].

## Port Scanning
**Port scanning** is done after [[#Host Discovery]] to detect the open ports. **Fingerprinting** is the process of determining the exact of services running and the underlying OS and configurations.

| Scan Type        | Example Command      |
| ---------------- | -------------------- |
| TCP Connect Scan | `nmap -sT <ip>`      |
| TCP SYN Scan     | `sudo nmap -sS <ip>` |
| UDP Scan         | `sudo nmap -sU <ip>` |

| Option                        | Description                                |
| ----------------------------- | ------------------------------------------ |
| `-p<one,csv,range>`           | select port                                |
| `-p-`                         | scan all ports till 65535                  |
| `-F`                          | scan top 100 most common ports             |
| `--top-ports <n>`            | scan top `n` most common ports             |
| `-T<0-5>`                     | time template, `-T0` slowest `-T5` fastest |
| `--<min,max>-rate <n>`        | packets per second (inclusive)             |
| `--<min,max>-parallelism <n>` | parallel probes (inclusive)                |

*Note.* `sudo` is necessary in scans other than TCP Connect Scan.
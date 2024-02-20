#tool

![[arp-scan example.png]]
`arp-scan` is CLI tool for scanning **local networks** using **ARP** requests. Performs basic fingerprinting, mainly MAC OUI decoding.

- **Limitations**
	- Only scans devices in the same subnet, because ARP packets are dropped by router

## Flags
*Note.* Requires `sudo`.

- `-I --interface` to specify interface to scan with
	- e.g. `arp-scan -I eth0 -l`
- `l --localnet` to specify local network ARP scan

## Commands
- `arp-scan -l` scan local network with default interface
- `arp-scan 192.168.0.0/24` scan the network
- `arp-scan 192.168.0.25-192.168.0.50` scan inclusive network range
- `arp-scan -I eth0 -l` scan local network wiwth `eth0` interface

## Notes
- Manufacturer may not be fingerprinted if the NIC *"hides"* the MAC address
	- E.g. Apple iOS can enaanble `Private Wi-Fi Address` to administer non-registered OUI's
	 ![[ARP iOS Private MAC Address.png]]


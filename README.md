# nmap-cheatsheet

## Passive ARP discovery:
`netdiscover -i <interface> -r <iprange> -p `
example:
`netdiscover -i eth0 -r 192.168.1.0/24 -p `

### Discovery (ping sweep):
`nmap -sn -n -T5 -vv -oA discovery 192.168.28.0/24 `

`-sn` Host discovery only, no port scan

`-n` Disable reverse IP address lookups (more stealthy, note that if you would scan nmap -n localhost it won't work as it wont resolve)

`-T5` increases scan speed

`-vv` extra verbose so we can see what's going on.

`-oA` writes it to a file named discovery

`192.168.28.0/24` the ip range

### Generate Live Hosts List:
`grep "Status: Up" discovery.gnmap | cut -f 2 -d ' ' > livehosts.txt `

outputs a file with only the live hosts.

## Common Ports:
Next will do a scan on the common ports, by default nmap doesn't scan the full port ranges (as this will take a long time).
Will output the results of the scans into a file that can be imported into other tools for example metasploit.

### TCP, syn-stealth scan.
`nmap -n -sS -pn -T5 -vv --reason -oA topTCP -iL livehosts.txt `

`-sS` a Syn scan this doesn't complete the full tcp connection instead it will do a syn packet.
recieve a syn-ack and then do a rst to close the connection.
this is more stealth as it won't show log in a lot of logs.

`-pn` Don't probe, we assume host are up.

`-oA topTCP` outputs it to a file topTCP

`--reason` lets nmap return the reason why it thinks a port is open, filtert or closed.

### UDP scan
`nmap -n -sU -T5 -vv --reason -oA topUDP -iL livehosts.txt `

`-sU` UDP scan

`-oA topUDP` outputs it to a file topUDP

`--reason` lets nmap return the reason why it thinks a port is open, filtert or closed.

### Full Ports:
`nmap -sS -T5 -pn --reason -p 0-65535 -oA FullTCP -iL livehosts.txt `

`-p 0-65535` specify the port range to scan

`nmap -sU -T5 -pn --reason -p 0-65535 -oA FullUDP -iL livehosts.txt `

`-p 0-65535` specify the port range to scan

###  Operating system & version scan:
`nmap -O -sV -T5 -PN -p 80-443, 445 -n 192.168.1.2

`-O` Operating system scan.

`-p 80-443, 445` specify ports that you want to scan, depending on what came out of our results before.
It can be good to combine this with some script scans, especially if services in the target network are not on default ports.

## scripts:
`--script=http-robots.txt.nse` pulls robots.txt files form target machines.

`--script=sshv1.nse` does a version check on ssh services.

`--script=nbstat.nse` pulls machine names, mac addresses and usernames, if port 135,139 or 445 are open it can run.

`--script=smb-enum-users.nse` enums users over port 139 netbios

look into using:
`--script-args-file` provide a file as input for scripts to run

### other script options:
`nmap -sS -T5 -Pn --script “default and safe” <host> `
`nmap -sS -T5 -Pn --script “http-* and safe” <host> `
`nmap -sS -T5 -Pn --script “smb-* and safe” <host> `
`nmap -sS -T5 -Pn --script “safe” <host> `

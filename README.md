# nmap-cheatsheet

  Passive ARP discovery:
  * netdiscover -i <interface> -r <iprange> -p

  Discovery:
  * nmap -sn -T4 -oA Discovery 192.168.28.0/24

  Generate Live Hosts List:
  * grep "Status: Up" Discovery.gnmap | cut -f 2 -d ' ' > LiveHosts.txt

  Common Ports:
  * nmap -sS -T4 -vv -Pn -oA TopTCP -iL LiveHosts.txt
  * nmap -sU -T4 -vv -Pn -oA TopUDP -iL LiveHosts.txt

  Full Ports:
  * nmap -sS -T4 -Pn -p 0-65535 -oA FullTCP -iL LiveHosts.txt
  * nmap -sU -T4 -Pn -p 0-65535 -oA FullUDP -iL LiveHosts.txt

  Example version scan:
  * nmap -sV -T4 -PN -n 192.168.1.2

  Operating system scan:
  * nmap -O -T4 -Pn -oA <host>-service.txt <host>

  script scans:
  * nmap -sS -T4 -Pn --script “default and safe” <host>
  * nmap -sS -T4 -Pn --script “http-* and safe” <host>
  * nmap -sS -T4 -Pn --script “smb-* and safe” <host>
  * nmap -sS -T4 -Pn --script “safe” <host>

# Networking — everything you need

## OSI model (7 layers) & TCP/IP
| OSI | Role | Examples | TCP/IP |
|---|---|---|---|
| 7 Application | user data | HTTP, DNS, SMTP, FTP | Application |
| 6 Presentation | encryption, encoding | TLS, JPEG | Application |
| 5 Session | sessions | NetBIOS, RPC | Application |
| 4 Transport | delivery, ports | **TCP, UDP** | Transport |
| 3 Network | routing, IP | **IP, ICMP, OSPF** | Internet |
| 2 Data Link | MAC, frames | Ethernet, ARP, switch | Link |
| 1 Physical | bits on wire | cable, radio | Link |

## TCP vs UDP
- **TCP** — connection-oriented (3-way handshake: SYN → SYN-ACK → ACK), reliable, ordered. HTTP, SSH, FTP.
- **UDP** — connectionless, faster, no guarantees. DNS, DHCP, VoIP, streaming.

## IP addressing
- **IPv4** — 32-bit (4 octets) e.g. `192.168.1.10`. **IPv6** — 128-bit, 8 hex groups.
- **Private ranges (RFC1918):** `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`.
- **CIDR/mask:** `/24` = 256 addrs (254 hosts), `/16` = 65536. Smaller number = bigger network.
- **Special:** `127.0.0.1` loopback, `0.0.0.0` all interfaces, broadcast `x.x.x.255`.

## NAT
Translates private IPs to one public IP. Router rewrites source, keeps a table, maps the reply back.
- **SNAT** — source rewrite (home NAT). **DNAT** — inbound forward (port forwarding). **PAT/Masquerading** — NAT with port rewrite (home, pfSense/OPNsense).

## Key protocols & ports
| Port | Protocol | Use |
|---|---|---|
| 20/21 | FTP | file transfer (plaintext) |
| 22 | SSH | remote access (encrypted) |
| 23 | Telnet | remote access (plaintext) |
| 25 | SMTP | send mail |
| 53 | DNS | name → IP (TCP+UDP) |
| 67/68 | DHCP | auto IP |
| 80 | HTTP | web |
| 110/143 | POP3/IMAP | receive mail |
| 161/162 | SNMP | device monitoring |
| 389/636 | LDAP(S) | directory (AD) |
| 443 | HTTPS | web TLS |
| 445 | SMB | Windows files |
| 3306 | MySQL | DB |
| 3389 | RDP | Windows remote desktop |
| 5985/5986 | WinRM | Windows remote mgmt |

## ICMP
Diagnostic protocol: ping, errors, route control. Carries no application data.

## ARP
Maps IP ↔ MAC on the LAN. Vulnerable to **ARP spoofing** (MITM): attacker sends forged ARP claiming the gateway's IP. Detect by tracking MAC↔IP changes (see 11-python-for-security / Scapy).

## DNS
Name → IP. Records: **A** (IPv4), **AAAA** (IPv6), **CNAME** (alias), **MX** (mail), **NS**, **TXT** (SPF), **PTR** (reverse). Attacks: subdomain enum, zone transfer, DNS poisoning.

## VLANs & segmentation
Logical split of one physical network. Factory example: `VLAN10 office · VLAN20 production · VLAN30 servers · VLAN40 guest`. Segmentation + firewall rules = an attack in one segment can't reach others (same principle as the SOC lab). Inter-VLAN routing via a router/L3 switch.

## Routing
- **Static** — manual routes. **Dynamic** — OSPF (internal), BGP (internet), RIP (legacy).
- **Default gateway** — where unknown-network traffic goes.

## Firewall / filtering
- **Stateful** — tracks connections (OPNsense, pf). **Rules** — allow/deny by src/dst IP, port, protocol. Principle: **default-deny**. **WAF** — application-layer firewall (SQLi/XSS/cmd injection).

## Sniffing (why it matters)
Packet capture: analyse attacks (reverse shell), find malware (exfiltration, C2 beacons), incident response, leaks (plaintext creds), VPN/SSH analysis, read non-TLS (HTTP/FTP/POP3/Telnet). Tools: **Wireshark** (GUI, Follow→TCP Stream), **tcpdump** (CLI), **Scapy** (Python).

## Quiet scanning
`nmap -sS` (SYN scan) never completes the handshake → looks like a dropped SYN in logs, quieter than `-sT`. Modern IPS still see it.

## VPN
Encrypted tunnel over a public network. **WireGuard** (fast, modern), **OpenVPN**, **IPSec**. Site-to-site and remote-access.

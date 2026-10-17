# Servers & services

## Web servers
- **nginx** — light, reverse proxy, static (`/etc/nginx/`).
- **Apache** — modular (`/etc/apache2/`, `.htaccess`).
- **Reverse proxy** — nginx in front of an app (TLS, LB, cache).
Attacks: directory traversal, exposed `.git`, misconfig (listing, backups), server-status.

## Databases
| DB | Port | Notes |
|---|---|---|
| MySQL/MariaDB | 3306 | LAMP/LEMP, SQLi target |
| PostgreSQL | 5432 | |
| MSSQL | 1433 | Windows/AD, `xp_cmdshell` → RCE |
| MongoDB | 27017 | often no auth |
| Redis | 6379 | no auth → RCE via key write |
| Oracle | 1521 | |
Attacks: SQLi, default/weak creds, unauthenticated access.

## Mail
SMTP (25/587), POP3 (110), IMAP (143). Audit with **swaks**, check open relay, SPF/DKIM/DMARC. Phishing: King Phisher, GoPhish.

## File services
- **SMB** (445) — `smbclient`, `smbmap`, enum4linux; EternalBlue (MS17-010).
- **NFS** (2049) — `showmount -e <ip>`. **FTP** (21) — anonymous, plaintext.

## Remote access
- **SSH** (22) — keys/passwords, tunnels/pivoting (`-L/-R/-D`).
- **RDP** (3389) — BlueKeep (CVE-2019-0708). **WinRM** (5985/5986) — evil-winrm.

## DNS server
BIND/dnsmasq. Attacks: zone transfer (`dig axfr`), poisoning, subdomain enum.

## Containers & orchestration
- **Docker** — risks: privileged container, mounted `docker.sock` → host escape, secrets in images.
- **Kubernetes** — risks: exposed API (6443/10250), RBAC misconfig, secrets in etcd.

## Web apps / frameworks
Fingerprint by favicon, headers, Wappalyzer. WordPress (wpscan), Drupal, Laravel, Flask/Django. Common holes: outdated versions (CVE), default admin panels, exposed configs (`.env`).

## Monitoring / SIEM (Blue Team)
**Wazuh / Splunk / Elastic**, **Suricata/Snort** (IDS/IPS), **Zeek**. Agents → manager → rules → alerts (as in the SOC lab).

## Hardening
Minimal open ports, default-deny firewall, timely patches, no default creds, least privilege, service accounts, TLS everywhere, logging + monitoring, backups.

# CompTIA notes (A+ / Security+ / PenTest+)

## CompTIA A+ — IT foundation
Hardware (CPU, RAM, storage, PSU, GPU), OS (Windows/Linux/macOS, CLI, permissions), networking basics (IP, DNS, DHCP, ports, OSI), troubleshooting methodology, security basics (malware, social engineering, backups).
> This maps to `01-computer-fundamentals` in your repo.

## CompTIA Security+ — infosec foundation (key for starting)
Domains (SY0-701):
1. **General Security Concepts** — CIA triad, AAA, zero trust, control types (preventive/detective/corrective; technical/administrative/physical).
2. **Threats, Vulnerabilities & Mitigations** — malware (virus, worm, trojan, ransomware, rootkit), attacks (phishing, MITM, DDoS, injection), social engineering.
3. **Security Architecture** — segmentation, zero trust, cloud, firewall, VPN, IDS/IPS, DLP.
4. **Security Operations** — monitoring, SIEM, incident response, forensics, hardening, patching.
5. **Program Management** — risk management, policies, compliance (GDPR, PCI DSS, HIPAA), audit.

Key concepts:
- **AAA:** Authentication · Authorization · Accounting.
- **Control types:** preventive (firewall), detective (IDS/logs), corrective (backup), deterrent, compensating.
- **Crypto:** symmetric (AES) vs asymmetric (RSA), hashing (SHA-256), PKI, digital signatures, TLS.
- **Risk** = likelihood × impact. Responses: accept / avoid / transfer / mitigate.
- **Frameworks:** NIST, ISO 27001, CIS Controls.

## CompTIA PenTest+ — offensive
1. Planning & Scoping (legal, rules of engagement).
2. Information Gathering & Vuln Scanning (OSINT, nmap, Nessus/OpenVAS).
3. Attacks & Exploits (network, web/OWASP, wireless, social engineering, AD).
4. Reporting & Communication (report, remediation, business risk).
5. Tools & Code Analysis (Metasploit, Burp, Python/Bash).

Maps to repo folders: methodology → 08, recon → 09, vulns → 07, commands → 02, practice → `tryhackme-walkthroughs`.

## Study path
1. **Security+** first (broad theory, HR filter).
2. Practice on **TryHackMe/HackTheBox** in parallel.
3. **PenTest+** once confident with tools.
4. A+ if you need a formal IT base (helpdesk/junior).

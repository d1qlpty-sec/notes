# Recon & OSINT

## OSINT
Gathering from open sources without touching the target. Tools: **theHarvester**, **Recon-ng**, **Maltego**, **SpiderFoot**, Google Dorking.

## Passive recon (no touch)
- **crt.sh** — Certificate Transparency logs → subdomains (every TLS cert is public).
- **Wayback Machine** (archive.org) — old/deleted pages, leaked data.
- **Wappalyzer** — site tech (CMS, framework, server).
- **Favicon hash** — framework icon reveals the engine (OWASP favicon DB).
- **Shodan / Censys** — exposed services/devices on the internet.
- **Google Dorking:** `site:*.domain.com -site:www.domain.com`, `filetype:pdf`, `intitle:index.of`.

## Active recon (scanning)
### Ports & services — nmap
```bash
nmap -sC -sV -p- -oN scan.nmap <ip>
nmap -sS -Pn -T4 -A <ip>
nmap -sU --top-ports 50 <ip>
```
### DNS / subdomains
```bash
dig / nslookup domain
dig axfr @ns domain.com            # zone transfer
dnsrecon -d domain.com -D wl.txt -t brt   # brute (noisy)
sublist3r -d domain.com            # passive: search engines, CT, DNS (quiet, fast)
```
### Web content
```bash
gobuster dir -u http://<ip> -w common.txt -x php,txt,html
ffuf -u http://<ip>/FUZZ -w wordlist.txt
```
### Virtual hosts (Host header fuzzing)
```bash
gobuster vhost -u http://domain.thm -w subdomains.txt
ffuf -w names.txt -H "Host: FUZZ.domain.thm" -u http://<ip> -fs <size>
# -fs filters false positives by response size
```
### Tech fingerprint
```bash
whatweb http://<ip>
nikto -h http://<ip>
```

## Finding repos/leaks on target
```bash
find / -type d -name ".git" 2>/dev/null
git-dumper http://site/.git/ out/
# grep history for API keys / passwords
```

## Important
- Stay in **scope**. Passive recon is silent; active (dnsrecon brute, nmap) is visible to IDS.

# CompTIA — конспекты (A+ / Security+ / PenTest+)

Три сертификации по возрастанию. Термины на английском (экзамен английский).

## CompTIA A+ — фундамент IT
База железа/ОС/сетей. Что знать:
- **Hardware:** CPU, RAM, storage (HDD/SSD/NVMe), motherboard, PSU, GPU, периферия.
- **OS:** Windows/Linux/macOS — установка, настройка, командная строка, права.
- **Networking basics:** IP, DNS, DHCP, порты, Wi-Fi, кабели, OSI.
- **Troubleshooting:** методика диагностики (identify → theory → test → fix → document).
- **Security basics:** malware, социнженерия, физбезопасность, бэкапы.
> Это «01-computer-fundamentals» в твоём репо — расширяй его под A+.

## CompTIA Security+ — база инфобеза (ключевая для старта)
Домены (SY0-701):
1. **General Security Concepts** — CIA triad, AAA (Authentication/Authorization/Accounting), zero trust, типы контролей (preventive/detective/corrective; technical/administrative/physical).
2. **Threats, Vulnerabilities & Mitigations** — malware (virus, worm, trojan, ransomware, rootkit), атаки (phishing, MITM, DDoS, injection), social engineering, уязвимости.
3. **Security Architecture** — сегментация, zero trust, cloud, firewall, VPN, IDS/IPS, DLP.
4. **Security Operations** — мониторинг, SIEM, incident response, forensics, hardening, patch management.
5. **Program Management & Oversight** — risk management, политики, compliance (GDPR, PCI DSS, HIPAA), аудит.

Ключевые понятия Security+:
- **AAA:** Authentication (кто ты) · Authorization (что можешь) · Accounting (логи).
- **Типы контролей:** preventive (firewall), detective (IDS/логи), corrective (бэкап), deterrent, compensating.
- **Криптография:** симметричная (AES) vs асимметричная (RSA), hashing (SHA-256), PKI, цифровые подписи, TLS.
- **Risk:** risk = likelihood × impact. Ответы: accept / avoid / transfer / mitigate.
- **Frameworks:** NIST, ISO 27001, CIS Controls.

## CompTIA PenTest+ — наступательная
Домены:
1. **Planning & Scoping** — правила, legal, scope, rules of engagement.
2. **Information Gathering & Vulnerability Scanning** — OSINT, nmap, Nessus/OpenVAS.
3. **Attacks & Exploits** — network, web (OWASP), wireless, social engineering, AD.
4. **Reporting & Communication** — отчёт, ремедиация, бизнес-риск.
5. **Tools & Code Analysis** — Metasploit, Burp, скрипты (Python/Bash).

Связь с остальными папками репо:
- методологии/scope → `08-methodology-and-frameworks`
- recon → `09-recon-and-osint`
- уязвимости → `07-vulnerabilities`
- команды → `02-pentest-commands`
- практика → репо `tryhackme-walkthroughs`

## Как готовиться
1. **Security+** первым (база, HR-фильтр, широкая теория).
2. Параллельно практика на **TryHackMe / HackTheBox** (твои writeups).
3. **PenTest+** когда уверенно с инструментами.
4. A+ — если нужен формальный IT-фундамент (helpdesk/junior).

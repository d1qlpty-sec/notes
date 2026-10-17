# Серверы и сервисы

## Веб-серверы
- **nginx** — лёгкий, reverse-proxy, статика. Конфиг `/etc/nginx/`.
- **Apache (httpd)** — модульный. Конфиг `/etc/apache2/`. `.htaccess`.
- **Reverse proxy** — nginx перед приложением (TLS, балансировка, кэш).
Атаки: directory traversal, exposed `.git`, misconfig (listing, backup-файлы), server-status.

## Базы данных
| БД | Порт | Заметки |
|---|---|---|
| MySQL/MariaDB | 3306 | веб-стек LAMP/LEMP, цель SQLi |
| PostgreSQL | 5432 | |
| MSSQL | 1433 | Windows/AD, `xp_cmdshell` → RCE |
| MongoDB | 27017 | часто без аутентификации |
| Redis | 6379 | без auth → RCE через запись ключей |
| Oracle | 1521 | |
Атаки: SQLi, дефолтные/слабые креды, незащищённый доступ, дамп через утечку.

## Почта
- **SMTP** (25/587) отправка, **POP3** (110) / **IMAP** (143) приём.
- Аудит: **swaks** (тест SMTP), проверка open relay, SPF/DKIM/DMARC.
- Фишинг-фреймворк: **King Phisher**, GoPhish.

## Файловые сервисы
- **SMB** (445) — Windows-шары. `smbclient`, `smbmap`, enum4linux. Анонимный доступ, EternalBlue (MS17-010).
- **NFS** (2049) — Unix-шары. `showmount -e <ip>`.
- **FTP** (21) — анонимный вход, plaintext.

## Удалённый доступ
- **SSH** (22) — ключи/пароли, туннели/pivoting (`ssh -L/-R/-D`).
- **RDP** (3389) — Windows GUI. BlueKeep (CVE-2019-0708).
- **WinRM** (5985/5986) — PowerShell remoting, evil-winrm.
- **Telnet/VNC** — устаревшее/plaintext.

## DNS-сервер
BIND/dnsmasq. Атаки: zone transfer (`dig axfr`), poisoning, subdomain enum.

## Контейнеры и оркестрация
- **Docker** — изоляция приложений. Риски: привилегированный контейнер, монтирование `/var/run/docker.sock` → escape to host, образы с секретами.
- **Kubernetes** — оркестрация. Риски: exposed API (6443/10250 kubelet), RBAC misconfig, секреты в etcd.

## Веб-приложения / фреймворки
- Определение по favicon, заголовкам, Wappalyzer. WordPress (wpscan), Drupal, Joomla, Laravel, Flask/Django.
- Типовые дыры: устаревшие версии (CVE), дефолтные админки (`/admin`, `/wp-admin`), exposed конфиги (`config.php`, `.env`).

## Мониторинг / SIEM (Blue Team)
- **Wazuh / Splunk / Elastic (ELK)** — сбор логов, детект, дашборды.
- **Suricata / Snort** — IDS/IPS (сетевые сигнатуры).
- **Zeek** — анализ сетевого трафика.
- Агенты собирают логи → менеджер → правила → алерты (как в SOC-лабе).

## Защита серверов (hardening)
- Минимум открытых портов, firewall default-deny.
- Патчи вовремя, убрать дефолтные креды.
- Принцип наименьших привилегий, отдельные сервис-аккаунты.
- TLS везде, логирование и мониторинг, бэкапы.

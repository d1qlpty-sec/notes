# Разведка и OSINT

## OSINT (Open-Source Intelligence)
Сбор данных из открытых источников без «касания» цели. Соцсети, сайты, утечки, метаданные.
Инструменты: **theHarvester** (почты/домены/хосты), **Recon-ng**, **Maltego**, **SpiderFoot**, **Google Dorking**.

## Пассивная разведка (не трогаем цель)
- **crt.sh** — поиск по Certificate Transparency логам → поддомены (каждый TLS-серт публичен).
- **Wayback Machine** (archive.org) — старые версии сайта, удалённые страницы/файлы, утёкшие данные.
- **Wappalyzer** — определить технологии сайта (CMS, фреймворк, сервер).
- **Favicon hash** — иконка фреймворка выдаёт движок (OWASP favicon database).
- **Shodan / Censys** — поиск открытых сервисов/устройств в интернете.
- **Google Dorking:** `site:*.domain.com -site:www.domain.com` (поддомены), `filetype:pdf`, `intitle:index.of`.

## Активная разведка (сканирование)
### Порты и сервисы — nmap
```bash
nmap -sC -sV -p- -oN scan.nmap <ip>      # скрипты + версии, все порты
nmap -sS -Pn -T4 -A <ip>                 # SYN stealth, агрессивно
nmap -sU --top-ports 50 <ip>             # UDP
```

### DNS / поддомены
```bash
dig / nslookup domain              # базовые запросы
dig axfr @ns domain.com            # zone transfer (если разрешён)
dnsrecon -d domain.com -D wl.txt -t brt   # брут поддоменов (шумно)
sublist3r -d domain.com            # пассивно: поисковики, CT, DNS-базы (тихо, быстро)
```

### Веб-контент (директории/файлы)
```bash
gobuster dir -u http://<ip> -w common.txt -x php,txt,html
ffuf -u http://<ip>/FUZZ -w wordlist.txt
dirb http://<ip>
```

### Виртуальные хосты (Host header fuzzing)
Непубличные поддомены (dev, admin) работают только с правильным Host-заголовком:
```bash
gobuster vhost -u http://domain.thm -w subdomains.txt
ffuf -w names.txt -H "Host: FUZZ.domain.thm" -u http://<ip> -fs <size>
# -fs <size> отсекает ложные ответы по размеру страницы
```

### Определение технологий
```bash
whatweb http://<ip>        # фингерпринт
nikto -h http://<ip>       # быстрый vuln-скан
```

## Поиск репозиториев/утечек на цели
```bash
find / -type d -name ".git" 2>/dev/null     # выставленный git
git-dumper http://site/.git/ out/            # сдампить историю
# грепать историю на секреты: API keys, пароли
```

## Важно
- **Список исключений (scope)** — не выходить за рамки разрешённого.
- Пассивная разведка не шумит в логах; активная (dnsrecon брут, nmap) — заметна IDS.

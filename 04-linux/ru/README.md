# Linux — для пентеста и SOC

## Файловая система
```
/        корень            /etc    конфиги (passwd, shadow, hosts)
/home    домашние папки    /var    логи (/var/log), веб (/var/www)
/tmp     временное         /opt    доп. софт
/root    дом root          /bin,/usr/bin  исполняемые
/proc    инфо о процессах/ядре     /dev  устройства
```
Важные файлы: `/etc/passwd` (пользователи), `/etc/shadow` (хеши паролей, root-only), `/etc/hosts` (статич. DNS), `/etc/crontab` (задачи по расписанию), `~/.bash_history`, `~/.ssh/`.

## Права (permissions)
```
-rwxr-xr--  =  owner rwx | group r-x | others r--
chmod 755 file    # rwx r-x r-x (число: r=4 w=2 x=1)
chown user:group file
```
- **SUID** (`chmod +s`, `rws`) — файл запускается с правами владельца (часто root). `find / -perm -4000 2>/dev/null` — ищем для privesc.
- **SGID**, **sticky bit** (`/tmp`).

## Пользователи и права
```bash
whoami ; id              # кто я, UID/GID/группы
sudo -l                  # что могу запускать от root (ключ для privesc)
cat /etc/passwd          # список пользователей
su - user                # сменить пользователя
```

## Базовые команды
```bash
ls -la ; cd ; pwd              # навигация
cat/less/head/tail file       # просмотр
grep "text" file -r           # поиск по тексту
find / -name "*.conf" 2>/dev/null   # поиск файлов
ps aux ; top                  # процессы
netstat -tulpn / ss -tulpn    # открытые порты
systemctl status/start/stop   # сервисы (systemd)
crontab -l                    # задачи по расписанию
```

## Сеть в Linux
```bash
ip a / ifconfig          # интерфейсы, IP
ip route / route -n      # таблица маршрутов
cat /etc/resolv.conf     # DNS-серверы
curl / wget URL          # HTTP-запросы, скачивание
nc -lvnp 4444            # netcat listener (reverse shell)
ssh user@ip -p port      # SSH
scp file user@ip:/path   # копирование по SSH
```

## Повышение привилегий (Linux PrivEsc)
Чеклист после получения шелла:
```bash
sudo -l                           # 1. sudo-права → GTFOBins
find / -perm -4000 2>/dev/null    # 2. SUID-бинарники → GTFOBins
uname -a ; cat /etc/os-release    # 3. версия ядра → kernel exploit?
cat /etc/crontab ; ls -la /etc/cron*   # 4. cron от root с доступным скриптом
cat ~/.bash_history ; env         # 5. пароли в истории/переменных
getcap -r / 2>/dev/null           # 6. capabilities
ls -la /home/*/                   # 7. чужие файлы, ключи SSH
```
**GTFOBins** (gtfobins.github.io) — база «как через легитимный бинарник получить root». Примеры:
- `sudo vim -c ':!/bin/bash'` — редактор → shell.
- `sudo find . -exec /bin/sh \;` — find → shell.
- SUID-bash: `chmod +s /bin/bash` затем `bash -p`.
- `fail2ban-client` с sudo → переопределить actionban.

Автоматизация: **linPEAS**, **LinEnum**, **pspy** (мониторинг процессов/cron без root).

## Логи (для SOC)
```
/var/log/auth.log    входы/sudo/SSH (Debian/Ubuntu)
/var/log/secure      то же (RHEL/CentOS)
/var/log/syslog      системные события
/var/log/apache2/    веб-логи (access/error)
journalctl -u <svc>  логи сервиса (systemd)
```

## Стабилизация шелла (после reverse shell)
```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'
# затем: Ctrl-Z ; stty raw -echo ; fg ; export TERM=xterm
```

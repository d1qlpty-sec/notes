# Linux — for pentest & SOC

## Filesystem
```
/        root            /etc    configs (passwd, shadow, hosts)
/home    user dirs       /var    logs (/var/log), web (/var/www)
/tmp     temporary       /opt    extra software
/root    root home       /bin,/usr/bin  binaries
/proc    process/kernel info     /dev  devices
```
Key files: `/etc/passwd` (users), `/etc/shadow` (password hashes, root-only), `/etc/hosts`, `/etc/crontab`, `~/.bash_history`, `~/.ssh/`.

## Permissions
```
-rwxr-xr--  =  owner rwx | group r-x | others r--
chmod 755 file    # r=4 w=2 x=1
chown user:group file
```
- **SUID** (`rws`) — runs as the owner (often root). `find / -perm -4000 2>/dev/null` for privesc.
- **SGID**, **sticky bit** (`/tmp`).

## Users & privileges
```bash
whoami ; id              # who am I, UID/GID/groups
sudo -l                  # what I can run as root (key for privesc)
cat /etc/passwd
su - user
```

## Core commands
```bash
ls -la ; cd ; pwd
cat/less/head/tail file
grep "text" file -r
find / -name "*.conf" 2>/dev/null
ps aux ; top
ss -tulpn / netstat -tulpn
systemctl status/start/stop
crontab -l
```

## Networking
```bash
ip a / ifconfig ; ip route
cat /etc/resolv.conf
curl / wget URL
nc -lvnp 4444
ssh user@ip -p port ; scp file user@ip:/path
```

## Privilege escalation (Linux)
Checklist after a shell:
```bash
sudo -l                           # 1. sudo rights → GTFOBins
find / -perm -4000 2>/dev/null    # 2. SUID → GTFOBins
uname -a ; cat /etc/os-release    # 3. kernel → exploit?
cat /etc/crontab ; ls -la /etc/cron*   # 4. root cron with writable script
cat ~/.bash_history ; env         # 5. creds in history/env
getcap -r / 2>/dev/null           # 6. capabilities
ls -la /home/*/                   # 7. other users' files, SSH keys
```
**GTFOBins** (gtfobins.github.io) — abuse legit binaries for root:
- `sudo vim -c ':!/bin/bash'` — editor → shell.
- `sudo find . -exec /bin/sh \;`.
- SUID bash: `chmod +s /bin/bash` then `bash -p`.
- `fail2ban-client` with sudo → override actionban.
Automation: **linPEAS**, **LinEnum**, **pspy** (watch cron/processes without root).

## Logs (for SOC)
```
/var/log/auth.log    logins/sudo/SSH (Debian/Ubuntu)
/var/log/secure      same (RHEL)
/var/log/syslog      system events
/var/log/apache2/    web access/error
journalctl -u <svc>  systemd service logs
```

## Shell stabilisation
```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'
# then: Ctrl-Z ; stty raw -echo ; fg ; export TERM=xterm
```

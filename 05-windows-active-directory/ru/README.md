# Windows и Active Directory

## Windows — основы
- **Пользователи/группы:** локальные (SAM) и доменные (AD). Важные группы: Administrators, Domain Admins, Enterprise Admins.
- **Хеши паролей:** NTLM (хранятся в SAM / LSASS в памяти). **Pass-the-Hash** — аутентификация по хешу без пароля.
- **UAC** — контроль учётных записей (запрос прав админа).
- **Реестр (registry):** `HKLM`, `HKCU` — конфиги, автозапуск (`...\Run`).
- **Важное:** `C:\Windows\System32\config\SAM` (хеши), `SYSTEM`, LSASS-процесс (дамп через mimikatz).

## Команды Windows (recon)
```cmd
whoami /priv /groups        # права и группы
net user ; net localgroup   # пользователи/группы
systeminfo                  # версия ОС, патчи (для exploit)
ipconfig /all ; arp -a      # сеть
netstat -ano                # порты + PID
tasklist                    # процессы
```
PowerShell: `Get-LocalUser`, `Get-Process`, `Get-Service`, `Get-ADUser` (в домене).

## Active Directory (AD) — что это
Централизованный каталог для управления пользователями/компьютерами в домене Windows. Компоненты:
- **Domain Controller (DC)** — сервер, хранит базу AD (NTDS.dit), выдаёт аутентификацию.
- **Domain / Forest / OU** — домен / лес (несколько доменов) / организационные единицы.
- **LDAP** (389/636) — протокол запросов к каталогу.
- **Kerberos** (88) — протокол аутентификации (билеты TGT/TGS).
- **SMB** (445), **WinRM** (5985) — файлы и удалённое управление.

## Kerberos — как работает (упрощённо)
1. Клиент → KDC: запрос **TGT** (Ticket Granting Ticket), подтверждая пароль.
2. Клиент → KDC: TGT → получает **TGS** (Service Ticket) для конкретного сервиса.
3. Клиент → сервис: TGS → доступ.

## Атаки на AD (ключевые)
| Атака | Суть |
|---|---|
| **Kerberoasting** | запросить TGS сервис-аккаунта → офлайн-брут хеша (SPN) |
| **AS-REP Roasting** | для юзеров без preauth → получить хеш для брута |
| **Pass-the-Hash (PtH)** | аутентификация по NTLM-хешу без пароля |
| **Pass-the-Ticket** | переиспользование Kerberos-билета |
| **Golden Ticket** | подделка TGT с ключом krbtgt → полный доступ к домену |
| **Silver Ticket** | подделка TGS для конкретного сервиса |
| **DCSync** | имитация DC → выгрузка хешей всех юзеров (нужны права репликации) |
| **LLMNR/NBT-NS poisoning** | перехват хешей в сети (Responder) |
| **ACL abuse** | злоупотребление правами (GenericWrite, WriteDACL) |

## Инструменты AD
- **BloodHound** / SharpHound — граф атак, поиск путей к Domain Admin.
- **Impacket** — набор скриптов (secretsdump, psexec, GetUserSPNs, wmiexec).
- **mimikatz** — дамп хешей/билетов из LSASS, PtH, Golden Ticket.
- **CrackMapExec (nxc)** — массовое тестирование SMB/WinRM/LDAP.
- **Responder** — перехват LLMNR/NBT-NS.
- **evil-winrm** — шелл по WinRM.

## Типичная цепочка атаки AD
```
初始 foothold (phishing/web) → LLMNR poisoning (Responder) → хеш юзера
  → crack / PtH → enum (BloodHound) → Kerberoasting → сервис-аккаунт
  → путь к Domain Admin (ACL/delegation) → DCSync → хеши всех → Golden Ticket
```

## Защита (Blue Team)
- Сильные пароли сервис-аккаунтов (против Kerberoasting), gMSA.
- Отключить LLMNR/NBT-NS, SMB signing.
- LAPS (уникальные пароли локальных админов), tiered admin model.
- Мониторинг: события 4624/4625 (входы), 4769 (TGS-запросы), 4768, DCSync-аномалии.

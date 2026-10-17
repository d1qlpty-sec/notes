# Windows & Active Directory

## Windows basics
- **Users/groups:** local (SAM) and domain (AD). Key: Administrators, Domain Admins, Enterprise Admins.
- **Password hashes:** NTLM (in SAM / LSASS memory). **Pass-the-Hash** — auth with the hash, no password.
- **UAC**, **Registry** (`HKLM`/`HKCU`, autostart `...\Run`).
- Key targets: `C:\Windows\System32\config\SAM`, LSASS (mimikatz dump).

## Recon commands
```cmd
whoami /priv /groups
net user ; net localgroup
systeminfo              # OS version, patches (for exploits)
ipconfig /all ; arp -a
netstat -ano ; tasklist
```
PowerShell: `Get-LocalUser`, `Get-Process`, `Get-ADUser` (domain).

## Active Directory
Centralised directory for a Windows domain. Components:
- **Domain Controller (DC)** — holds the AD DB (NTDS.dit), does auth.
- **Domain / Forest / OU**.
- **LDAP** (389/636), **Kerberos** (88), **SMB** (445), **WinRM** (5985).

## Kerberos (simplified)
1. Client → KDC: request **TGT** (proves password).
2. Client → KDC: TGT → **TGS** (service ticket).
3. Client → service: TGS → access.

## AD attacks
| Attack | Idea |
|---|---|
| **Kerberoasting** | request a service account TGS → crack SPN hash offline |
| **AS-REP Roasting** | users without preauth → crackable hash |
| **Pass-the-Hash** | auth with NTLM hash, no password |
| **Pass-the-Ticket** | reuse a Kerberos ticket |
| **Golden Ticket** | forge TGT with krbtgt key → full domain |
| **Silver Ticket** | forge TGS for one service |
| **DCSync** | impersonate a DC → dump all hashes |
| **LLMNR/NBT-NS poisoning** | capture hashes on the LAN (Responder) |
| **ACL abuse** | abuse rights (GenericWrite, WriteDACL) |

## AD tools
**BloodHound**/SharpHound (attack graph), **Impacket** (secretsdump, psexec, GetUserSPNs), **mimikatz** (dump hashes/tickets, PtH, Golden Ticket), **CrackMapExec/nxc**, **Responder**, **evil-winrm**.

## Typical AD kill chain
```
foothold → LLMNR poisoning (Responder) → user hash → crack/PtH
  → enum (BloodHound) → Kerberoasting → service account
  → path to Domain Admin (ACL/delegation) → DCSync → all hashes → Golden Ticket
```

## Defence (Blue Team)
Strong service-account passwords / gMSA, disable LLMNR/NBT-NS, SMB signing, LAPS, tiered admin. Monitor events 4624/4625 (logins), 4769 (TGS), DCSync anomalies.

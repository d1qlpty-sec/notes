# Methodologies & frameworks

## Pentest phases
1. **Information Gathering** — OSINT, no active scanning.
2. **Enumeration/Scanning** — ports, services, versions.
3. **Exploitation** — use vulns for access.
4. **Privilege Escalation** — horizontal (other user) / vertical (root/SYSTEM).
5. **Post-exploitation** — persistence, loot, pivoting, cleanup, reporting.

## Methodologies
| Framework | Scope | Idea |
|---|---|---|
| **OSSTMM** | networks, telecom, physical, human | scientific, deep, complex |
| **OWASP** | web apps | Top 10, test guides, simple & current |
| **NIST CSF** | organisational cyber risk | Identify, Protect, Detect, Respond, Recover |
| **NCSC CAF** | critical infrastructure (UK) | 14 principles |
| **PTES / MITRE ATT&CK** | attack techniques | tactics & techniques matrix |

## Testing scope
- **Black-box** — zero info, like an external hacker; heavy recon.
- **Grey-box** — partial info (creds, docs); most common commercially.
- **White-box** — full access (code, configs); deepest, code audit.

## CIA Triad
- **Confidentiality** — only authorised see data (encryption, auth, access control, DLP).
- **Integrity** — data not altered unnoticed (hashes, signatures, access control, audit).
- **Availability** — accessible when needed (redundancy, failover, anti-DDoS, backups; SLA/uptime).

## Access control
- **PIM** — "who you are": accounts, roles, identity.
- **PAM** — "what you can do and how it's audited": rights, sessions, passwords, audit.
- **Least privilege (PoLP)** — minimal rights for the task.
- Models: **RBAC** (roles), **ABAC** (attributes), **MAC/DAC**.

## Security models
- **Bell–LaPadula** (confidentiality): No Read Up, No Write Down.
- **Biba** (integrity): No Read Down, No Write Up.

## Threat modelling + STRIDE
Cycle: Preparation → Identification → Mitigations → Review.
| Letter | Threat | Violates | Defence |
|---|---|---|---|
| **S**poofing | identity spoof | authentication | MFA, signatures, keys |
| **T**ampering | data change | integrity | hashes, signatures |
| **R**epudiation | deny actions | accountability | logs, audit |
| **I**nformation Disclosure | leak | confidentiality | encryption, ACL |
| **D**enial of Service | outage | availability | rate-limit, anti-DDoS |
| **E**levation of Privilege | priv gain | authorization | PoLP, patches, segmentation |

## Incident Response — 6 phases
1. Preparation 2. Identification 3. Containment 4. Eradication 5. Recovery 6. Lessons Learned.
Classify by urgency (attack type) and impact (system/business). Team = **CSIRT**.

## MITRE ATT&CK
Matrix of tactics (why) and techniques (how). E.g. T1046 (Service Discovery), T1110 (Brute Force), T1078 (Valid Accounts). Used to map SOC detections.

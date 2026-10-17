# Cloud security

## Models
- **IaaS / PaaS / SaaS**.
- **Shared Responsibility** — provider secures the cloud; you secure what's in it.
- Top cause of breaches: **misconfiguration** (open S3, weak IAM, exposed ports).

## AWS essentials
- **IAM** — users/roles/policies. Risks: over-permissive, old keys, root without MFA.
- **S3** — storage. Risk: public buckets (leaks).
- **EC2** — VMs. Risk: open Security Groups (SSH/RDP to internet), metadata service (SSRF → creds).
- **Security Groups** — instance-level firewall.

## Cloud audit tools
| Tool | Role |
|---|---|
| **AWS Trusted Advisor** | built-in AWS: security/cost/performance recommendations |
| **Scout Suite** | multi-cloud config audit (AWS/Azure/GCP), HTML report |
| **Prowler** | AWS scanner vs CIS Benchmark / compliance |
```bash
prowler aws          # full AWS CIS audit
scout aws            # audit → HTML report
```

## Checklist
- IAM: MFA for all, no root keys, least privilege, key rotation.
- S3: no public buckets, encryption on.
- Network: SGs not `0.0.0.0/0` on sensitive ports.
- Logging: CloudTrail on, anomaly monitoring.
- Encryption: at-rest and in-transit.

## Other
- **Azure** (Entra ID / AD), **GCP**, **Kubernetes** (see 06), **DevSecOps** (secure CI/CD, image scanning, secrets management).

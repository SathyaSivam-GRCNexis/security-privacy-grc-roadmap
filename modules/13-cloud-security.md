# Module 13 — Cloud Security Basics

> **Audience:** 🔴 primary · 🟡 for concepts · **Time:** ~90 min · **Prereqs:** Modules 2–4

## Why this matters

Almost every workload now runs on AWS, Azure, or GCP. Most cloud breaches come not from the provider's platform being broken — it very rarely is — but from **customer misconfiguration**. This module covers what you, as the customer, are actually responsible for and how to do it well.

---

## 13.1 Shared Responsibility — the foundational model

Every cloud provider publishes a shared-responsibility diagram. It always ends in the same conclusion: **you own your data, identities, and configuration, always.**

| Layer | IaaS (e.g., EC2) | PaaS (e.g., RDS) | SaaS (e.g., Salesforce) |
|-------|-----------------|-----------------|-----------------------|
| Physical / facilities | Provider | Provider | Provider |
| Host OS, hypervisor | Provider | Provider | Provider |
| Guest OS | **Customer** | Provider | Provider |
| Runtime, middleware | **Customer** | Provider | Provider |
| Application | **Customer** | **Customer** | Provider |
| **Data** | **Customer** | **Customer** | **Customer** |
| **Identity & access config** | **Customer** | **Customer** | **Customer** |
| **Logging / monitoring enablement** | **Customer** | **Customer** | **Customer** |
| **Network config (VPC, SG)** | **Customer** | **Customer** | Partly |

Internalise this or every cloud conversation will confuse you.

### Misreading the model

Developers sometimes say "AWS is secure, we don't need to worry." They're confusing **security *of* the cloud** (AWS's responsibility) with **security *in* the cloud** (yours). Major public cloud breaches (Capital One, Twitch, T-Mobile Snowflake-channel, etc.) mostly trace to customer misconfig.

---

## 13.2 Cloud IAM deep-dive

Cloud IAM is where most cloud breaches start.

### Entities

- **Users / principals** — humans and service accounts.
- **Roles** — sets of permissions assumed temporarily.
- **Groups** — collections of users.
- **Policies** — documents granting/denying permissions.
- **Service principals / managed identities** — non-human identities for workloads.

### Patterns that work

- **SSO → Cloud** — log in once via IdP (Okta, Entra), assume roles in cloud. No long-lived keys for humans.
- **Least privilege** — start with zero and add. Use policy simulators and access analysers.
- **Short-lived credentials** — STS / federation tokens, not static keys.
- **Workload identity** — EC2 instance profile / IRSA for Kubernetes / GCP Workload Identity — no secrets in pods.
- **Permission boundaries, SCPs, Azure policies** — set organisational guardrails.
- **Break-glass** — emergency access, separate MFA, alerting, rarely used.

### Anti-patterns that cause breaches

- Static access keys committed to Git.
- `*:*` admin roles attached to every workload "for convenience."
- Wide cross-account trust without external-id.
- Shared service-account credentials across envs.
- No MFA on root / global admin.
- Forgotten legacy IAM users from ex-employees.

### Root / master accounts

- MFA with hardware key; password in sealed envelope.
- Never use for day-to-day work.
- No programmatic keys on root.
- Alert on every login.
- Billing contact separate from security contact.

---

## 13.3 Network security in cloud

- **VPC / VNet** — virtual network.
- **Subnets** — public (route to internet) vs private.
- **Security Groups (AWS/Azure) / Firewall rules (GCP)** — stateful, instance-level.
- **NACLs** — stateless, subnet-level (AWS).
- **Private endpoints / PrivateLink / Private Service Connect** — access services without traversing public internet.
- **Egress control** — often forgotten; restrict outbound to known destinations to prevent exfil.
- **NAT Gateway vs Internet Gateway** — know the difference.
- **VPC peering vs Transit Gateway vs VPN vs Direct Connect/ExpressRoute.**

### Default-closed networking

Start with no public access. Open specific ports to specific CIDRs for specific services. Any design that says "open to 0.0.0.0/0 for now" is a misconfig waiting to happen.

---

## 13.4 Encryption and key management in cloud

- **Cloud KMS** — AWS KMS, Azure Key Vault, GCP KMS, GCP Cloud HSM.
- **Provider-managed keys** — fine for most.
- **Customer-managed keys (CMK)** — your KMS key, but still inside provider.
- **BYOK** — you generate the master key and import it.
- **HYOK / External Key Store (AWS XKS)** — key stays outside cloud, cloud calls out to use it.

Enable encryption at rest **by default**: S3 default encryption, EBS default encryption, RDS encryption, Azure Storage encryption, GCS CMEK. Encryption in transit: TLS 1.2+ on every endpoint.

**Key rotation** — cloud-managed keys rotate automatically on schedule; CMKs need a rotation policy.

---

## 13.5 Logging, monitoring, and cloud-native detection

- **CloudTrail (AWS) / Azure Activity Log / GCP Audit Logs** — every API action. Enable org-wide, send to a secure account/project, **make immutable** (S3 Object Lock, Azure Immutable Blob, GCS retention lock).
- **VPC Flow Logs / NSG Flow Logs / VPC Flow Logs.**
- **App logs** — structured, centralised.
- **Cloud-native detection:** GuardDuty, Azure Defender, GCP Security Command Center. Enable at org level.
- **Config recording:** AWS Config, Azure Policy, GCP Asset Inventory — know what's running.

Retention: 1 year hot + 7 years cold typical for regulated workloads; minimum 90 days for small SaaS.

---

## 13.6 Posture tools — the acronym zoo, demystified

- **CSPM (Cloud Security Posture Management)** — continuously audits configuration against best practices (CIS, benchmarks). Examples: Wiz, Prisma Cloud, Orca, Lacework, AWS Security Hub, Defender for Cloud, open-source Prowler/ScoutSuite.
- **CWPP (Cloud Workload Protection Platform)** — runtime protection for VMs / containers / serverless.
- **CIEM (Cloud Infrastructure Entitlement Management)** — analyses IAM permissions for least-privilege gaps.
- **CNAPP (Cloud-Native Application Protection Platform)** — unified CSPM + CWPP + CIEM + container + sometimes code scanning.
- **KSPM** — Kubernetes-specific posture.
- **ASPM** — Application Security Posture Management.
- **SSPM** — SaaS Security Posture Management (for SaaS apps like Okta, Salesforce, Google Workspace).

Pick one CNAPP as soon as you have meaningful cloud spend; free tools (Prowler, ScoutSuite, KubeBench, Cloudsplaining) cover a huge portion of the value for free.

---

## 13.7 Containers, Kubernetes, and serverless

### Containers

- **Image security:** trusted base images, minimal (distroless), SBOM, vulnerability scan at build + runtime.
- **Registry:** private, immutable tags, signed images (Sigstore/Cosign).
- **Runtime:** drop capabilities, read-only fs, non-root, seccomp, AppArmor.

### Kubernetes

- **RBAC** — scoped; no cluster-admin tokens mounted in every pod.
- **Namespaces** — soft multi-tenancy.
- **Network Policies** — default-deny east-west.
- **Admission controllers** — Pod Security Standards (restricted), OPA/Gatekeeper, Kyverno policies.
- **Secrets** — external secrets (AWS/Azure/GCP) with CSI driver, not plain K8s Secrets (which are base64, not encrypted by default at etcd level unless configured).
- **Supply chain:** SLSA, image signing, admission verification.

### Serverless

Minimise function permissions (per-function IAM role). Validate inputs. Watch cold-start secrets handling.

---

## 13.8 Data security in cloud

- **S3 / blob / GCS buckets** — the classic leak point. Default-deny public; block-public-access account-wide; bucket policies reviewed; object-level ACLs disabled where possible.
- **Pre-signed URLs** — generate short-lived ones for download; never hard-code long TTLs.
- **DB access** — private subnets, IAM auth where available, parameterised queries at app layer.
- **Multi-tenancy** — per-tenant keys, row-level security, or separate databases; always tested against cross-tenant leakage.

---

## 13.9 DevSecOps in cloud

- **IaC (Terraform, Bicep, Pulumi, CDK)** scanned pre-merge (Checkov, tfsec, Terrascan).
- **Policy-as-code** (OPA/Rego, Kyverno).
- **Secrets scanning** on every commit (gitleaks, trufflehog, GitHub secret scanning).
- **Image scanning** on build (Trivy, Grype).
- **SBOM** generation and storage (Syft, cyclonedx).
- **Signed artefacts** (Sigstore/Cosign, SLSA provenance).
- **Runtime attestation** for high-assurance.

---

## 13.10 Cloud incidents — the quick playbook

If a cloud account is suspected compromised:

1. **Identify the blast radius** — which account, which role, what actions did they take?
2. **Quarantine** — revoke/rotate credentials; detach IAM policies; isolate resources.
3. **Snapshot evidence** — disk snapshots, memory dumps (where possible), logs preserved.
4. **Rebuild, don't clean** — assume the attacker left persistence.
5. **Rotate every credential** — access keys, service-account keys, OAuth tokens, SSH keys, TLS certs where required.
6. **Review CloudTrail/Activity Log** for post-compromise actions.
7. **Check IAM changes** — new users, roles, trust policies added by attacker.
8. **Check data stores** — replication enabled? Cross-account copies? Bucket policies modified?
9. **Notify** per regulatory clocks.
10. **Postmortem & harden** — add detections to prevent the specific technique.

---

## 13.11 Go deeper

- 🏛 [AWS Security Pillar (Well-Architected)](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html) · [AWS Shared Responsibility](https://aws.amazon.com/compliance/shared-responsibility-model/)
- 🏛 [Azure Shared Responsibility](https://learn.microsoft.com/en-us/azure/security/fundamentals/shared-responsibility) · [Cloud Adoption Framework — security](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/secure/)
- 🏛 [GCP Shared Responsibility / Shared Fate](https://cloud.google.com/architecture/framework/security/shared-responsibility-shared-fate) · [Google Cloud Security best practices center](https://cloud.google.com/security/best-practices)
- 🏛 [CIS Benchmarks — AWS/Azure/GCP/K8s (free PDFs)](https://www.cisecurity.org/cis-benchmarks)
- 🏛 [OWASP Kubernetes Top 10](https://owasp.org/www-project-kubernetes-top-ten/)
- 🧪 [flaws.cloud](http://flaws.cloud/) · [flaws2.cloud](http://flaws2.cloud/) · [CloudGoat (AWS vulnerable env)](https://github.com/RhinoSecurityLabs/cloudgoat) · [Kubernetes Goat](https://github.com/madhuakula/kubernetes-goat)
- 🧪 [Prowler (free CSPM)](https://github.com/prowler-cloud/prowler) · [ScoutSuite](https://github.com/nccgroup/ScoutSuite) · [Kube-bench](https://github.com/aquasecurity/kube-bench)
- 📰 [Cloud Security Alliance (CSA)](https://cloudsecurityalliance.org/)
- 🎥 [AWS re:Invent security sessions — free on YouTube](https://www.youtube.com/@AWSEventsChannel)

## Module 13 — Glossary recap

Shared Responsibility, IaaS/PaaS/SaaS, IAM, Principal, Role, Policy, STS, Workload identity, SCP, Permission boundary, Break-glass, VPC, Subnet, Security Group, NACL, Private endpoint, Egress control, KMS, CMK, BYOK, HYOK, XKS, CloudTrail, Activity Log, Cloud Audit Logs, VPC Flow Logs, GuardDuty, Defender for Cloud, Security Command Center, AWS Config, Azure Policy, CSPM, CWPP, CIEM, CNAPP, KSPM, ASPM, SSPM, SBOM, Sigstore/Cosign, SLSA, OPA/Rego, Kyverno, Pod Security Standards, Admission controller, Pre-signed URL, IaC scanning, Policy-as-code.

→ Next: [Module 14 — Application Security (AppSec)](14-appsec.md)

# Module 13: Cloud Security Basics

> **Audience:** 🔴 primary · 🟡 for concepts · **Time:** ~90 min · **Prereqs:** Modules 2–4

## Why this matters

Nearly every workload now runs on AWS, Azure, or GCP. Most cloud breaches do not happen because the provider's platform broke. They happen because someone left a bucket open, attached an over-privileged role, or committed a static access key to a public repo. The pattern is so consistent it is almost boring.

This module covers what you, the customer, are actually on the hook for, and how to do it without learning the same lessons everyone else has.

---

## 13.1 Shared responsibility: the foundational model, and how it gets misread

Every provider publishes a shared-responsibility diagram. They all say the same thing in the end: you own your data, your identities, and your configuration. Always.

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

Internalise this table or every cloud conversation will confuse you.

### How developers get this wrong

"AWS is secure, so we are secure" is the most common sentence I have heard in this job. It is wrong, and the confusion is between security *of* the cloud (the provider's job) and security *in* the cloud (yours). The big public-cloud incidents almost all trace back to customer misconfiguration: a public bucket, an over-permissive role, MFA missing on root, credentials committed to Git. The provider was fine. The customer's controls were not.

For SaaS, the responsibility line shifts even more onto you for configuration. SaaS posture problems (Okta admin without MFA, Salesforce guest-user exposure, M365 sharing defaults) are now their own category of incident.

---

## 13.2 Cloud IAM deep-dive

Cloud IAM is where most cloud breaches actually start. Get this right and you have removed half the risk.

### Entities

- Users and principals: humans and service accounts.
- Roles: sets of permissions assumed temporarily.
- Groups: collections of users.
- Policies: documents granting or denying permissions.
- Service principals and managed identities: non-human identities for workloads.

### Patterns that work

- SSO into cloud. Log in once via the IdP (Okta, Entra), then assume roles. No long-lived keys for humans.
- Least privilege. Start at zero and add. Use policy simulators and access analysers.
- Short-lived credentials. STS, federation tokens, not static keys.
- Workload identity. EC2 instance profile, IRSA for Kubernetes, GCP Workload Identity. No secrets in pods.
- Permission boundaries, SCPs, Azure policies. Organisational guardrails that survive individual mistakes.
- Break-glass account: separate MFA, alerting on every use, rarely used.

### Anti-patterns that cause breaches

- Static access keys in Git. The most common root cause I have personally seen.
- `*:*` admin roles attached to workloads "for convenience."
- Wide cross-account trust without external-id.
- Shared service-account credentials across environments.
- No MFA on root or global admin.
- Forgotten IAM users from ex-employees who left two years ago.

### Root and master accounts

- MFA with a hardware key. Password in a sealed envelope in a safe.
- Never use for day-to-day work.
- No programmatic keys on root.
- Alert on every login.
- Billing contact and security contact should be different people.

---

## 13.3 Network security in cloud

- VPC and VNet: the virtual network.
- Subnets: public (route to internet) versus private.
- Security Groups in AWS and Azure, firewall rules in GCP: stateful, instance-level.
- NACLs: stateless, subnet-level (AWS).
- Private endpoints, PrivateLink, Private Service Connect: access provider services without traversing the public internet.
- Egress control. Often forgotten. Restrict outbound to known destinations and you have killed a large class of exfiltration paths.
- NAT Gateway versus Internet Gateway. Know the difference before you draw an architecture diagram.
- VPC peering versus Transit Gateway versus VPN versus Direct Connect / ExpressRoute.

### Default-closed networking

Start with no public access. Open specific ports to specific CIDRs for specific services. Any design with "open to 0.0.0.0/0 for now" is a misconfiguration waiting to happen. "For now" tends to become "forever."

---

## 13.4 Encryption and key management in cloud

- Cloud KMS: AWS KMS, Azure Key Vault, GCP KMS, GCP Cloud HSM.
- Provider-managed keys. Fine for most workloads.
- Customer-managed keys (CMK). Your KMS key, still inside the provider.
- BYOK. You generate the master key and import it.
- HYOK or External Key Store (AWS XKS). Key stays outside the cloud; the cloud calls out to use it.

Enable encryption at rest by default: S3 default encryption, EBS default encryption, RDS encryption, Azure Storage encryption, GCS CMEK. Encryption in transit: TLS 1.2 or later on every endpoint.

Key rotation: cloud-managed keys rotate automatically. CMKs need a rotation policy that someone actually owns.

In practice, the BYOK and HYOK conversation with enterprise customers is more about trust optics than meaningful risk reduction. Be honest about what each option gives you and what it costs in operational pain.

---

## 13.5 Logging, monitoring, and cloud-native detection

- CloudTrail (AWS), Azure Activity Log, GCP Audit Logs. Every API action. Enable at org level, ship to a secure account or project, and make immutable (S3 Object Lock, Azure Immutable Blob, GCS retention lock).
- VPC Flow Logs, NSG Flow Logs.
- Application logs: structured and centralised.
- Cloud-native detection: GuardDuty, Azure Defender, GCP Security Command Center. Enable at org level on day one. They are not free, but they are cheaper than the incident.
- Config recording: AWS Config, Azure Policy, GCP Asset Inventory. Know what is running.

Retention: usually 1 year hot plus 7 years cold for regulated workloads; 90 days minimum for small SaaS. Check what the relevant regulator actually requires before signing up to expensive storage.

---

## 13.6 Posture tools: the acronym zoo, demystified

- **CSPM (Cloud Security Posture Management)**: continuously audits configuration against best practices (CIS benchmarks, vendor-specific rules). Examples: Wiz, Prisma Cloud, Orca, Lacework, AWS Security Hub, Defender for Cloud, and open-source Prowler / ScoutSuite.
- **CWPP (Cloud Workload Protection Platform)**: runtime protection for VMs, containers, serverless.
- **CIEM (Cloud Infrastructure Entitlement Management)**: analyses IAM permissions for least-privilege gaps.
- **CNAPP (Cloud-Native Application Protection Platform)**: unified CSPM + CWPP + CIEM + container + sometimes code scanning.
- **KSPM**: Kubernetes-specific posture.
- **ASPM**: Application Security Posture Management.
- **SSPM**: SaaS Security Posture Management (Okta, Salesforce, Google Workspace, M365).

### CSPM versus process

The trap is buying a CNAPP and thinking the problem is solved. The tool will surface 4,000 findings. Without a process to triage, prioritise, and route to the right owner, the dashboard becomes wallpaper.

What works, in my experience: pick the top 20 to 30 rules that map to your real risk (exposed S3, public-IP databases, IAM users with old keys, missing MFA, root activity). Wire those to ticketing with named owners and SLAs. Ignore the rest until that pipeline is healthy. Free tools (Prowler, ScoutSuite, Kube-bench, Cloudsplaining) cover a huge portion of the value without a license fee.

---

## 13.7 Containers, Kubernetes, and serverless

### Containers

- Image security: trusted base images, minimal or distroless, SBOM, vulnerability scan at build and at runtime.
- Registry: private, immutable tags, signed images via Sigstore/Cosign.
- Runtime: drop capabilities, read-only filesystem, non-root user, seccomp, AppArmor.

### Kubernetes

- RBAC scoped tightly. No cluster-admin tokens mounted in every pod.
- Namespaces for soft multi-tenancy.
- Network policies, default-deny east-west.
- Admission controllers: Pod Security Standards (restricted), OPA/Gatekeeper, Kyverno.
- Secrets: external secrets via the CSI driver (AWS, Azure, GCP). Plain Kubernetes Secrets are base64, not encrypted at the etcd level unless you configured it. People forget this.
- Supply chain: SLSA, image signing, admission verification.

### Serverless

Per-function IAM role. Validate inputs. Watch cold-start secrets handling. Lambdas with `*:*` are an accident waiting to happen.

---

## 13.8 Data security in cloud

- S3, blob, GCS buckets are the classic leak point. Default-deny public. Block-public-access at the account level. Bucket policies reviewed by a human, not just by a tool. Object-level ACLs disabled where the platform allows it.
- Pre-signed URLs: short-lived. Hard-coded long TTLs are a finding waiting to happen.
- Database access: private subnets, IAM auth where available, parameterised queries at the app layer.
- Multi-tenancy: per-tenant keys, row-level security, or separate databases. Test for cross-tenant leakage; do not assume.

---

## 13.9 DevSecOps in cloud

- IaC (Terraform, Bicep, Pulumi, CDK) scanned pre-merge (Checkov, tfsec, Terrascan).
- Policy-as-code (OPA/Rego, Kyverno).
- Secrets scanning on every commit (gitleaks, trufflehog, GitHub secret scanning).
- Image scanning on build (Trivy, Grype).
- SBOM generation and storage (Syft, cyclonedx).
- Signed artefacts (Sigstore/Cosign, SLSA provenance).
- Runtime attestation for high-assurance workloads.

A note on rollout. Turning these on in blocking mode from day one is how you make engineering hate security. Run in advisory mode first, fix the noise, then start blocking. Trust is rebuilt slowly.

---

## 13.10 Cloud incidents: the quick playbook

If a cloud account is suspected compromised:

1. Identify the blast radius. Which account, which role, what actions did they take? CloudTrail is your friend.
2. Quarantine. Revoke and rotate credentials. Detach IAM policies. Isolate resources.
3. Snapshot evidence. Disk snapshots, memory dumps where possible, logs preserved before retention chews them up.
4. Rebuild, do not clean. Assume the attacker left persistence.
5. Rotate every credential: access keys, service-account keys, OAuth tokens, SSH keys, TLS certs where required.
6. Review CloudTrail and Activity Log for post-compromise actions.
7. Check IAM changes. New users, roles, trust policies added by the attacker.
8. Check data stores. Replication enabled? Cross-account copies? Bucket policies modified?
9. Notify per regulatory clocks.
10. Postmortem and harden. Add detections for the specific technique used.

---

## 13.11 Go deeper

- 🏛 [AWS Security Pillar (Well-Architected)](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html) · [AWS Shared Responsibility](https://aws.amazon.com/compliance/shared-responsibility-model/)
- 🏛 [Azure Shared Responsibility](https://learn.microsoft.com/en-us/azure/security/fundamentals/shared-responsibility) · [Cloud Adoption Framework: security](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/secure/)
- 🏛 [GCP Shared Responsibility / Shared Fate](https://cloud.google.com/architecture/framework/security/shared-responsibility-shared-fate) · [Google Cloud Security best practices center](https://cloud.google.com/security/best-practices)
- 🏛 [CIS Benchmarks: AWS/Azure/GCP/K8s (free PDFs)](https://www.cisecurity.org/cis-benchmarks)
- 🏛 [OWASP Kubernetes Top 10](https://owasp.org/www-project-kubernetes-top-ten/)
- 🧪 [flaws.cloud](http://flaws.cloud/) · [flaws2.cloud](http://flaws2.cloud/) · [CloudGoat (AWS vulnerable env)](https://github.com/RhinoSecurityLabs/cloudgoat) · [Kubernetes Goat](https://github.com/madhuakula/kubernetes-goat)
- 🧪 [Prowler (free CSPM)](https://github.com/prowler-cloud/prowler) · [ScoutSuite](https://github.com/nccgroup/ScoutSuite) · [Kube-bench](https://github.com/aquasecurity/kube-bench)
- 📰 [Cloud Security Alliance (CSA)](https://cloudsecurityalliance.org/)
- 🎥 [AWS re:Invent security sessions: free on YouTube](https://www.youtube.com/@AWSEventsChannel)

## Module 13: Glossary recap

Shared Responsibility, IaaS/PaaS/SaaS, IAM, Principal, Role, Policy, STS, Workload identity, SCP, Permission boundary, Break-glass, VPC, Subnet, Security Group, NACL, Private endpoint, Egress control, KMS, CMK, BYOK, HYOK, XKS, CloudTrail, Activity Log, Cloud Audit Logs, VPC Flow Logs, GuardDuty, Defender for Cloud, Security Command Center, AWS Config, Azure Policy, CSPM, CWPP, CIEM, CNAPP, KSPM, ASPM, SSPM, SBOM, Sigstore/Cosign, SLSA, OPA/Rego, Kyverno, Pod Security Standards, Admission controller, Pre-signed URL, IaC scanning, Policy-as-code.

→ Next: [Module 14: Application Security (AppSec)](14-appsec.md)

# Path 🔴 — Tech Professional

> *You are a software engineer, SRE, cloud engineer, DBA, QA, or platform engineer. You know systems. You want to move into security engineering, cloud security, AppSec, DevSecOps, detection engineering, privacy engineering, or GRC engineering. You can skim the foundations fast and spend most of your time on the technical and framework-rich modules.*

**Estimated time:** 100–130 hours over 10–14 weeks (8–10 hours per week).
**End state:** ready to apply for **Security Engineer**, **Cloud Security Engineer**, **AppSec Engineer**, **DevSecOps Engineer**, **Detection Engineer**, **Privacy Engineer**, **GRC Engineer**, or **Security Architect** (with seniority).

---

## What you already know vs what you don't

You probably know:

- How HTTP, TLS, DNS, and TCP work end-to-end.
- IAM concepts in at least one cloud provider.
- CI/CD pipelines, infrastructure-as-code, container orchestration.
- Logging, monitoring, alerting, on-call discipline.
- Code-review and threat-of-bugs intuition.

You probably don't know (and this is what hiring managers test):

- Why your designs need to map to **specific framework controls**, and which ones.
- How to write a **risk** statement that an executive will fund.
- How to read a **SOC 2 report** and tell whether a vendor is actually safe.
- How **GDPR Art. 28** changes your data-processing contracts and engineering.
- How **DPDP Act 2023** + **CERT-In Directions** change incident response in India.
- How to translate **OWASP LLM Top 10** and **MITRE ATLAS** into a real AI feature gate.
- How a **STRIDE** threat model lands in front of an architecture review board.

The work below closes those gaps without re-teaching you what a TCP handshake is.

---

## Skim plan (one focused weekend, ~10 hours)

These you can read once, fast, then reference later:

- Module 0 — read in 60 minutes; jump to §0.4 (risk vocabulary) and §0.5 (policy literacy).
- Module 1 — read for the **CIA triad**, **STRIDE**, **defence-in-depth**, **Zero Trust** vocabulary.
- Module 2 — table-scan; you know most. Confirm you can talk about **FIPS 203/204/205** (post-quantum) at interview level.
- Module 3 — skim everything except **OAuth 2.1 + OIDC + SAML** comparison and **JIT / PIM** patterns.
- Module 4 — skim. You probably know it. Confirm vocabulary on **CISA Zero Trust Maturity Model 2.0**.

After skimming, if you can answer the interview traps in those modules unprompted, you're done with foundations.

---

## Deep plan (7–10 weeks)

| Week | Focus | What you will produce |
|------|-------|----------------------|
| 1 | Modules 5 + 6 | A data-flow map for one of your services, lawful basis annotated |
| 2 | Module 7 | A control-to-framework cross-map for your service (10 rows, 4 frameworks) |
| 3 | Module 8 | Quantified risk statements (FAIR-style) for 3 risks you actually own |
| 4 | Module 9 | An evidence-as-code design: how your CI/CD emits audit evidence automatically |
| 5 | Module 10 | A policy you wrote that an engineer would actually follow (not vendor template) |
| 6 | Module 11 | An IR runbook with **CERT-In 6-hour clock** baked in for an India-touching service |
| 7 | Module 12 | A vendor-risk decision framework you'd run as the engineering lead |
| 8 | Module 13 (deep) | Shared-responsibility matrix + threat model for one cloud workload |
| 9 | Module 14 (deep) | Custom Semgrep rule + secure-by-default IaC module + a real CVE remediation write-up |
| 10 | Module 15 | A **go / no-go memo** for an AI feature using OWASP LLM Top 10 + MITRE ATLAS + EU AI Act categorisation |

---

## Capstone scenarios you should do (Module 17)

Pick at least **four** from the technical-leaning set:

- **Scenario 3 — BYOK rollout** (Modules 2, 13). Architect, threat-model, customer-facing FAQ.
- **Scenario 7 — AI feature launch gate** (Modules 5, 6, 15). DPIA, threat model, vendor checklist, monitoring plan.
- **Scenario 8 — 3 AM breach: credential-stuffing + data exposure** (Modules 3, 9, 11). Minute-by-minute IR log; CERT-In + DPDP + GDPR notification plan.
- **Scenario 10 — Pen-test report lands with 4 Criticals** (Modules 11, 14). Triage, compensating controls, SDLC fix.

Plus at least one from the GRC-heavy set so you can speak both languages:

- **Scenario 1 — Pre-audit evidence scramble** (Modules 7, 9, 10). The artefact that shows you can interface with auditors without rolling your eyes.

---

## What hiring managers ask you specifically

You will not get the benefit of the doubt on framework knowledge. The questions land harder on you:

- *"Walk me through how you'd design a logging pipeline that satisfies SOC 2 CC7.2, ISO 27001 A.8.15/A.8.16, and supports a DFIR investigation."* The bad answer is "we use Datadog." The good answer covers retention, integrity, access, search latency, time-sync, log shipping resilience, and how it maps to specific control criteria.
- *"Your service handles EU personal data via a US sub-processor. What changed for you after Schrems II?"* Bad: "we have SCCs." Good: SCCs + TIA + supplementary technical measures + ROPA update + DPA refresh + customer notification path.
- *"You're shipping an AI feature using a third-party LLM. Defend the design to a sceptical privacy lawyer."* Bad: "we don't store prompts." Good: redact-then-summarise, training opt-out, zero-retention mode, prompt logging policy, OWASP LLM Top 10 mitigations, monitoring for hallucination/harmful output, EU AI Act risk categorisation, ISO 42001 alignment.
- *"How does your CI/CD pipeline emit audit evidence?"* Bad: "we have screenshots." Good: signed artifact provenance, change records linked to tickets, approval gates, logged deploy events, evidence stored in an immutable bucket with retention, all queryable for an auditor without manual screenshotting.

If you can answer these four well, you are competitive for senior security engineering roles in 2026.

---

## Certifications worth your time

Per [Module 16.2](../modules/16-careers-and-interviews.md):

- **AWS Security Specialty** OR **Azure SC-100/SC-200** OR **GCP Professional Cloud Security Engineer** — pick the one that matches your stack.
- **CCSP** (ISC2) — if you want a management-leaning cloud cert later.
- **OSCP** (Offensive Security) — only if you want offensive-leaning roles.
- **CISSP** (ISC2) — wait until 5+ years' security experience; it's a ceiling-raiser.
- **CISA** (ISACA) — useful if you want to bridge into security architecture or GRC engineering leadership.

You do **not** need: 5 vendor certs stacked on each other, OSCP if you don't want offensive work, CISSP before you have the experience.

---

## Salary, India 2025–26 (INR LPA)

From [interview-prep/india-market-snapshot.md](../interview-prep/india-market-snapshot.md):

- AppSec / Cloud Security Engineer (3–6y): **25–55**.
- Security Architect (8–12y): **45–90**.
- Detection Engineer / Senior SOC L3 (4–7y): **20–45**.
- GCC senior security engineering roles often top India bands.
- Remote-first global roles: 1.8–3× India-local.

---

## One trap to avoid

Do not assume that because you're technically strong you can wing the GRC and privacy modules. The most common interview failure for tech professionals moving into security is the framework-vocabulary gap. You will be asked to map your design to **specific control criteria**. The candidate who says "this maps to CC6.1, CC7.2, A.8.15, A.8.16, and PR.AA-01" gets the offer over the candidate who says "this is secure because of MFA and logging." Same controls, different career outcomes.

---

→ Open **[Module 0 — Foundation Pack](../modules/00-foundation-pack.md)**. Skim in 60 minutes. Tomorrow, start Module 5.

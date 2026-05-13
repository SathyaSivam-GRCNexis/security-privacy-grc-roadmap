# Path: Tech Professional

> *Software engineer, SRE, cloud engineer, DBA, QA, or platform engineer. You know systems. You want to move into security engineering, cloud security, AppSec, DevSecOps, detection engineering, privacy engineering, or GRC engineering. You can skim the foundations in a weekend. The work is in the framework-rich and governance-heavy modules you have been avoiding for years.*

**Estimated time:** 100 to 130 hours over 10 to 14 weeks (8 to 10 hours per week).
**End state:** ready to apply for **Security Engineer**, **Cloud Security Engineer**, **AppSec Engineer**, **DevSecOps Engineer**, **Detection Engineer**, **Privacy Engineer**, **GRC Engineer**, or, with seniority, **Security Architect**.

---

## How hard is this, honestly?

The pivot itself is not hard. You will be hireable in three to four months if you put the time in. What trips up most tech professionals is something else: the modules you find boring (governance, audit lifecycle, vendor risk) are the ones the interview turns on. You can answer Kubernetes RBAC questions fluently and still lose the offer because you could not name three SOC 2 Common Criteria.

Honest hedging: if you are coming from a pure-backend role with no on-call, no production access, and no IAM work, this is closer to 18 weeks than 12. If you are an SRE or platform engineer who has been pulled into audits or vendor reviews before, you can probably do it in 10.

---

## What wastes time vs. what compounds

The classic tech-professional trap: spending the first month re-reading foundations because they feel familiar. Do not do this. You are not going to learn anything new about TCP or TLS at this point. Read once, confirm vocabulary, move on.

What wastes time:

- Re-reading Modules 0 to 4 carefully. Skim and go.
- Doing every AppSec lab when you already write the kind of code that gets AppSec-reviewed.
- Stacking three cloud security certs before you have written a single risk statement an executive would fund.
- Pretending governance modules do not matter. They are the entire interview for the senior bands.

What compounds:

- Module 7 (frameworks) done properly with a real cross-map from a service you actually own.
- Module 8 (risk) with FAIR-style quantification on three risks tied to systems you ship.
- Module 12 (vendor risk) and Module 10 (policy) done from the engineering side. Most engineers cannot write a policy another engineer will follow. If you can, you are a Security Architect candidate, not just an engineer.
- One capstone scenario that combines IR + framework + privacy law (Scenario 8 is the canonical one). Doing this end-to-end is worth more than three certifications.

---

## What you already know vs. what you do not

You probably know:

- How HTTP, TLS, DNS, and TCP work end to end.
- IAM concepts in at least one cloud provider.
- CI/CD pipelines, infrastructure-as-code, container orchestration.
- Logging, monitoring, alerting, on-call discipline.
- Code review and threat-of-bugs intuition.

You probably do not know, and this is what hiring managers test:

- Why your designs need to map to specific framework controls, and which ones.
- How to write a risk statement that an executive will fund.
- How to read a SOC 2 report and tell whether a vendor is actually safe, or just polished.
- How GDPR Art. 28 changes your data-processing contracts and engineering.
- How DPDP Act 2023 and CERT-In Directions change incident response in India.
- How to translate OWASP LLM Top 10 and MITRE ATLAS into a real AI feature gate.
- How a STRIDE threat model lands in front of an architecture review board.

The plan below closes those gaps without re-teaching you what a TCP handshake is.

---

## Skim plan (one focused weekend, around 10 hours)

Read once, fast, then reference later:

- Module 0. 60 minutes. Jump to §0.4 (risk vocabulary) and §0.5 (policy literacy).
- Module 1. CIA triad, STRIDE, defence in depth, Zero Trust vocabulary.
- Module 2. Table scan. Confirm you can talk about FIPS 203, 204, 205 (post-quantum) at interview level. That is the only part of crypto that gets asked in 2026.
- Module 3. Skim everything except OAuth 2.1 + OIDC + SAML comparison and JIT / PIM patterns. Those two come up constantly.
- Module 4. Skim. Confirm vocabulary on CISA Zero Trust Maturity Model 2.0.

If you can answer the interview traps in those modules unprompted after the weekend, you are done with foundations. Move on.

---

## Deep plan (7 to 10 weeks)

| Week | Focus | What you will produce |
|------|-------|----------------------|
| 1 | Modules 5 + 6 | A data-flow map for one of your services, lawful basis annotated |
| 2 | Module 7 | A control-to-framework cross-map for your service (10 rows, 4 frameworks) |
| 3 | Module 8 | Quantified risk statements (FAIR-style) for 3 risks you actually own |
| 4 | Module 9 | An evidence-as-code design: how your CI/CD emits audit evidence automatically |
| 5 | Module 10 | A policy you wrote that an engineer would actually follow (not a vendor template) |
| 6 | Module 11 | An IR runbook with the CERT-In 6-hour clock baked in, for an India-touching service |
| 7 | Module 12 | A vendor-risk decision framework you would run as the engineering lead |
| 8 | Module 13 (deep) | Shared-responsibility matrix + threat model for one cloud workload |
| 9 | Module 14 (deep) | Custom Semgrep rule + secure-by-default IaC module + a real CVE remediation write-up |
| 10 | Module 15 | A go / no-go memo for an AI feature using OWASP LLM Top 10 + MITRE ATLAS + EU AI Act categorisation |

The week-4 evidence-as-code design is the single artefact I would push you to make excellent. It is the artefact that makes the difference between "security engineer" and "GRC engineer" titles, and the GRC engineer band pays better at the moment in India because there are roughly four of them in the country.

---

## Capstone scenarios you should do (Module 17)

Pick at least four from the technical-leaning set:

- **Scenario 3: BYOK rollout** (Modules 2, 13). Architect, threat-model, customer-facing FAQ.
- **Scenario 7: AI feature launch gate** (Modules 5, 6, 15). DPIA, threat model, vendor checklist, monitoring plan.
- **Scenario 8: 3 AM breach, credential stuffing + data exposure** (Modules 3, 9, 11). Minute-by-minute IR log; CERT-In + DPDP + GDPR notification plan.
- **Scenario 10: Pen-test report lands with 4 Criticals** (Modules 11, 14). Triage, compensating controls, SDLC fix.

Plus at least one from the GRC-heavy set so you can speak both languages:

- **Scenario 1: Pre-audit evidence scramble** (Modules 7, 9, 10). The artefact that shows you can interface with auditors without rolling your eyes. Senior hiring managers screen for this in the room. They have seen too many engineers who treat auditors as obstacles.

---

## What hiring managers ask you specifically

You will not get the benefit of the doubt on framework knowledge. The questions land harder on you than on a GRC-background candidate, because you are expected to translate engineering into controls without prompting.

- *"Walk me through how you would design a logging pipeline that satisfies SOC 2 CC7.2, ISO 27001 A.8.15 and A.8.16, and supports a DFIR investigation."* Bad answer: "we use Datadog." Good answer covers retention, integrity, access, search latency, time sync, log-shipping resilience, and how each property maps to specific control criteria.
- *"Your service handles EU personal data via a US sub-processor. What changed for you after Schrems II?"* Bad: "we have SCCs." Good: SCCs plus TIA plus supplementary technical measures plus ROPA update plus DPA refresh plus customer notification path.
- *"You are shipping an AI feature using a third-party LLM. Defend the design to a sceptical privacy lawyer."* Bad: "we do not store prompts." Good: redact-then-summarise, training opt-out, zero-retention mode, prompt-logging policy, OWASP LLM Top 10 mitigations, monitoring for hallucination or harmful output, EU AI Act risk categorisation, ISO 42001 alignment.
- *"How does your CI/CD pipeline emit audit evidence?"* Bad: "we have screenshots." Good: signed artifact provenance, change records linked to tickets, approval gates, logged deploy events, evidence stored in an immutable bucket with retention, all queryable for an auditor without manual screenshotting.

If you can answer these four well, you are competitive for senior security engineering roles in 2026.

---

## Certifications worth your time

Per [Module 16.2](../modules/16-careers-and-interviews.md):

- **AWS Security Specialty**, **Azure SC-100 or SC-200**, or **GCP Professional Cloud Security Engineer**. Pick the one that matches your stack. Stop at one.
- **CCSP** (ISC2). Only if you want a management-leaning cloud cert later.
- **OSCP** (Offensive Security). Only if you want offensive-leaning roles. It is brutal and irrelevant to GRC engineering.
- **CISSP** (ISC2). Wait until you have five or more years of security experience. Ceiling-raiser, not entry ticket.
- **CISA** (ISACA). Genuinely useful if you want to bridge into security architecture or GRC engineering leadership. Underused by engineers and that is precisely why it is a differentiator.

You do not need: five vendor certs stacked on top of each other, OSCP if you do not want offensive work, or CISSP before you have the experience.

---

## Salary, India 2025 to 26 (INR LPA)

From [interview-prep/india-market-snapshot.md](../interview-prep/india-market-snapshot.md):

- AppSec or Cloud Security Engineer (3 to 6 years): **25 to 55**.
- Security Architect (8 to 12 years): **45 to 90**.
- Detection Engineer or Senior SOC L3 (4 to 7 years): **20 to 45**.
- GCC senior security engineering roles often top India bands.
- Remote-first global roles: 1.8x to 3x India-local.

---

## One trap to avoid

Do not assume that because you are technically strong you can wing the GRC and privacy modules. The most common interview failure for tech professionals moving into security is the framework-vocabulary gap. You will be asked to map your design to specific control criteria. The candidate who says "this maps to CC6.1, CC7.2, A.8.15, A.8.16, and PR.AA-01" gets the offer over the candidate who says "this is secure because of MFA and logging." Same controls. Different career outcomes.

Bring your engineering credibility into policy work. A policy written by someone who has actually been paged at 3 AM reads differently from one written by a consultant. That difference is the reason you will be hired.

---

→ Open **[Module 0: Foundation Pack](../modules/00-foundation-pack.md)**. Skim in 60 minutes. Tomorrow, start Module 5.

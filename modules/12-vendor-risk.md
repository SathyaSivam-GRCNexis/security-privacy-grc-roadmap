# Module 12 — Third-Party / Vendor Risk

> **Audience:** 🟡 🔴 · **Time:** ~60 min · **Prereqs:** Modules 5–7

## Why this matters

Your customers trust you; you trust your vendors; your vendors trust *their* vendors. A single weak link in that chain creates headline breaches (SolarWinds, Kaseya, MOVEit, Okta, Snowflake). Every audit and regulator asks: **"How do you manage third-party risk?"** This module gives you the actual answer.

---

## 12.1 Types of third parties

- **Software vendors** (SaaS, on-prem).
- **Cloud & infrastructure providers.**
- **Professional services** (consultants, auditors, lawyers).
- **Outsourced operations** (BPO, customer support, payroll).
- **Data processors / sub-processors.**
- **Resellers & partners.**
- **Open-source dependencies** (software-supply chain — covered more in Module 14).

The **fourth party** is your vendor's vendor. The **fifth party** is beyond that. Real programs look at least two layers deep for critical vendors.

---

## 12.2 The TPRM lifecycle

1. **Onboarding (intake & tiering).**
2. **Due diligence.**
3. **Contracting.**
4. **Ongoing monitoring.**
5. **Offboarding.**

### Tiering — not every vendor deserves the same scrutiny

Assess by:
- **Data access** (none / internal only / personal / sensitive / regulated).
- **System access** (none / limited / privileged).
- **Business criticality** (non-critical / important / critical).
- **Spend and volume.**

Typical tiers:

| Tier | Example | Depth of DD |
|------|---------|-------------|
| Tier 1 (critical) | IdP, cloud provider, core DB, payroll processor | Full SIG, SOC 2/ISO, DPIA, on-site or deep review, executive approval, annual review |
| Tier 2 (important) | CRM, analytics, email provider | SIG-Lite, SOC 2, DPA, annual review |
| Tier 3 (low) | Stationery supplier, office coffee | Minimal questions, contract on file |

---

## 12.3 Due diligence — the DDQ universe

### Common questionnaires

- **SIG (Shared Assessments)** — full (1000+ questions) and **SIG-Lite** (~300). Most comprehensive; industry standard.
- **CAIQ (Cloud Security Alliance)** — cloud-provider focused.
- **HECVAT** — education sector.
- **VSAQ (Google)** — simpler web-app focus.
- **Proprietary** — many large enterprises use their own.

### What to collect

- Completed DDQ.
- **SOC 2 Type II** report (latest).
- ISO 27001 certificate + SoA.
- Pen-test summary (most vendors won't share full report).
- Evidence of relevant certifications (HIPAA BAA, PCI AoC, HDS, FedRAMP).
- DPA with processor's sub-processor list.
- Privacy notice, retention policy.
- BCP/DR summary, last test date.
- Financial health (for critical vendors).
- Insurance (cyber liability, E&O).
- OFAC / sanctions check (regulated industries).

### Reviewing a SOC 2 — the pro tips

- Read the **auditor's opinion** first. Qualified opinion is a red flag.
- Check the **period covered**; not stale.
- Check **subservice organisation handling** — inclusive (covered) vs carve-out (vendor's own vendors not tested). Plan accordingly.
- Check **exceptions** in Section 4. One exception is normal; clusters in same area are not.
- Check **CUECs** (Complementary User Entity Controls) — things *you* must do for the vendor's controls to work. These are implicit commitments you inherit.
- Scope mismatch — is the product you're buying actually in-scope?

---

## 12.4 Contracting — make it binding

Security and privacy in contracts:

- **Data Processing Agreement (DPA)** — required under GDPR Art. 28.
- **Business Associate Agreement (BAA)** — required under HIPAA when PHI is involved.
- **Standard Contractual Clauses (SCCs)** — for cross-border transfers.
- **Sub-processor list** — with notification rights and right to object.
- **Security addendum** — specific controls (encryption, MFA, logging, response times).
- **Breach notification SLA** — typically 24–72 hours to you.
- **Right to audit** — annually + after incidents.
- **Termination & data return/destruction** — with certificate.
- **Liability & indemnity** — negotiated; cyber-incident carve-outs.
- **Insurance** — minimum coverage.
- **Ethics / sanctions / modern-slavery** — in regulated industries.

---

## 12.5 Ongoing monitoring

Vendors don't stay safe forever.

- **Continuous monitoring ratings** — SecurityScorecard, Bitsight, UpGuard produce external risk scores from exposed surface.
- **SOC 2 / ISO annual refresh** — chase reports before they expire.
- **Breach alerts & news monitoring** — when a vendor is in the news, act.
- **Quarterly / annual review** — tier-dependent.
- **Event triggers** — M&A, leadership change, significant incident, new sub-processor.
- **Sub-processor change notices** — evaluate before they go live.

### Concentration risk

Too many critical vendors in one region, one cloud, or one provider creates systemic fragility. Know your exposure: what % of critical workloads ride on AWS us-east-1? Track in risk register.

---

## 12.6 Offboarding

Often forgotten, often audited.

- **Revoke access** — SSO integrations, API keys, VPN, SaaS admin panels.
- **Data return or destruction** — get a **certificate of destruction**.
- **Close the account** in the vendor's platform.
- **Update sub-processor list** and communicate to customers if public.
- **Record retention** — evidence of offboarding kept per policy.

---

## 12.7 Building a Trust Center (the other side)

You're not just a vendor-assessor; customers assess you. Make it easy:

- **Public Trust Center** — security page (not the marketing fluff kind): summary, certifications, SOC 3, pen-test attestation, sub-processor list, incident disclosures, status page, DPA template, security whitepaper.
- **NDA-gated room** for SOC 2 Type II, ISO SoA, detailed pen-test, DDQ responses.
- **Pre-answered library** — SIG-Lite, CAIQ, HECVAT already filled; send in minutes, not weeks.
- **Sub-processor notification** — email list or RSS; publish policy (e.g., "30-day notice before new sub-processor").

Tools: SafeBase, Vanta Trust Center, Drata Trust Center, Whistic, Conveyor.

---

## 12.8 Worked example — evaluating a new analytics vendor

Proposed: a product-analytics SaaS that ingests event data, potentially including user IDs and IPs.

Your steps:

1. **Intake ticket** — product team requests onboarding.
2. **Data assessment** — user IDs + IPs → personal data; Tier 2.
3. **Send SIG-Lite + request SOC 2 Type II + DPA template.**
4. **Check**: where do they host? US-only → SCC + TIA required (if EU users).
5. **Sub-processor list** — any you can't accept?
6. **DPIA** — collaborate with product: what's collected, why, retention, user rights.
7. **Security review** — IAM, encryption, SSO support, export formats.
8. **Contract negotiations** — 72-hour breach notice, audit rights, data destruction on termination.
9. **Approval** — document decision + residual risks.
10. **Register in vendor inventory** with next review date.
11. **Train** product teams on safe configuration (data minimisation in events).

---

## 12.9 Common beginner mistakes

- Treating the DDQ as the assessment. It's an input; read it, verify claims against evidence.
- No tiering → over-auditing low-risk vendors, under-auditing criticals.
- Ignoring sub-processor chains.
- No offboarding discipline — zombie access persists.
- Procurement going around security. Fix with a mandatory intake gate in the purchase-request flow.

---

## 12.10 Go deeper

- 🏛 [Shared Assessments (SIG)](https://sharedassessments.org/sig/)
- 🏛 [CSA CAIQ v4](https://cloudsecurityalliance.org/research/cloud-controls-matrix)
- 🏛 [AICPA SOC report user guidance (free)](https://www.aicpa-cima.com/resources/download/soc-for-service-organizations-trust-services-criteria)
- 📰 [Vanta / Drata / Secureframe TPRM guides (free)](https://www.vanta.com/resources)
- 📰 [Whistic / Conveyor content libraries](https://www.whistic.com/resources/)
- 🏛 [HECVAT (higher ed)](https://www.ren-isac.net/hecvat/)
- 🏛 [NIST SP 800-161 — Supply Chain Risk Management](https://csrc.nist.gov/pubs/sp/800/161/r1/final)

## Module 12 — Glossary recap

Third party, Fourth party, Sub-processor, TPRM lifecycle, Tiering, SIG, SIG-Lite, CAIQ, HECVAT, VSAQ, DPA, BAA, SCC, CUEC, Subservice org (inclusive vs carve-out), Qualified opinion, Sub-processor notice, Right to audit, Certificate of destruction, Continuous monitoring rating, Concentration risk, Trust Center, NDA-gated room, Pre-answered DDQ library.

→ Next: [Module 13 — Cloud Security Basics](13-cloud-security.md)

# Module 7 — GRC Frameworks (SOC, ISO, NIST, and friends)

> **Audience:** 🟡 🔴 · **Time:** ~2 hours · **Prereqs:** Modules 0–6

## Why this matters

Frameworks are the **vocabulary of GRC**. Every customer questionnaire, every board slide, every auditor conversation references one of them. If you know SOC 2, ISO 27001, and NIST CSF well, you can read almost any other standard at 80% speed. This is also where career-switchers make their first impression — a candidate who can articulate "SOC 2 CC6.1 means…" is immediately credible.

We'll cover, in depth:

1. **SOC 1 vs SOC 2 vs SOC 3** — the AICPA suite.
2. **ISO/IEC 27001:2022** and its family (27002, 27017, 27018, 27701, 22301, 9001).
3. **NIST Cybersecurity Framework 2.0.**
4. **NIST SP 800-53 & 800-171, CMMC.**
5. **COSO, COBIT** — governance frameworks you'll hear referenced.
6. **Framework cross-mapping** — the single most useful GRC skill.
7. **Which one should we start with?**

---

## 7.1 SOC 1 vs SOC 2 vs SOC 3 — the AICPA suite

"SOC" = **System and Organization Controls**. Issued under AICPA standards (SSAE 18). Performed by licensed **CPA firms**.

### SOC 1 — controls over **financial reporting**

Purpose: assurance for **your customers' auditors**. If your service could affect a customer's financial statements (payroll, billing, ERP, transaction processing), their auditor needs assurance on your controls.

- Criteria: ICFR (Internal Controls over Financial Reporting). *Not* a fixed checklist — you define the control objectives relevant to financial reporting.
- Audience: customer's external auditors.
- Often confused with SOC 2 — quite different in purpose.

### SOC 2 — controls over the **Trust Services Criteria**

The most famous in SaaS. Five **Trust Services Criteria (TSC)**:

- **Security** (mandatory — the "Common Criteria", CC).
- **Availability** (optional).
- **Processing Integrity** (optional).
- **Confidentiality** (optional).
- **Privacy** (optional).

Most SaaS start with Security + Availability + Confidentiality.

#### SOC 2 "Common Criteria" (CC) series

| Series | Theme |
|--------|-------|
| CC1 | Control Environment |
| CC2 | Communication and Information |
| CC3 | Risk Assessment |
| CC4 | Monitoring Activities |
| CC5 | Control Activities |
| CC6 | Logical & Physical Access |
| CC7 | System Operations |
| CC8 | Change Management |
| CC9 | Risk Mitigation |

Individual criteria (e.g., **CC6.1** — "logical access security software, infrastructure, and architectures") are what you provide evidence against.

#### Type I vs Type II

- **Type I** — design of controls at a point in time. Cheap, fast, weak signal.
- **Type II** — design *and* operating effectiveness **over a period** (typically 3–12 months; most customers demand 6–12). The report to aim for.

#### What's in the SOC 2 report

- Section 1 — Independent auditor's report (opinion).
- Section 2 — Management's assertion.
- Section 3 — System description (your architecture, processes, commitments).
- Section 4 — Trust Services Criteria, Controls, Tests performed, **Results (including any exceptions)**.
- Section 5 — Other information (optional).

When someone sends you their SOC 2 report, **Section 4** is where the real signal is.

### SOC 3

Public-facing summary of SOC 2. No detailed test results — suitable for marketing pages. Many vendors publish a SOC 3 in lieu of sharing a full SOC 2 under NDA.

### SOC for Cybersecurity and SOC for Supply Chain

Niche variants — know the names.

### How to kick off a SOC 2

1. **Define scope** — which product, environments, supporting functions.
2. **Gap assessment** — compare current controls to TSC.
3. **Remediation** — close gaps (policies, tooling, training).
4. **Select auditor** — licensed CPA firm; cost $20k–$80k typical.
5. **Readiness assessment** (optional but wise).
6. **Observation period** (Type II, 3–12 months).
7. **Fieldwork + reporting.**
8. **Post-audit** — share with customers; plan next year.

---

## 7.2 ISO/IEC 27001:2022 + family

### ISO 27001 — the global ISMS standard

An **Information Security Management System** (ISMS) is a documented system of policies, processes, roles, and controls for managing information security. ISO 27001 describes the *management system*; Annex A lists *controls*.

#### 2013 → 2022 update

- 93 controls (down from 114), reorganised into **4 themes**: **Organisational (37), People (8), Physical (14), Technological (34)**.
- Added controls: threat intel, ICT for BCP, data leakage prevention, cloud services, secure coding, etc.
- Existing certifications had a transition period; by 2025 most are on the 2022 version.

#### ISMS clauses (the management-system part)

Clauses 4–10 define the PDCA:

- Clause 4 — Context of the organisation.
- Clause 5 — Leadership.
- Clause 6 — Planning (incl. risk assessment).
- Clause 7 — Support (resources, competence, awareness).
- Clause 8 — Operation.
- Clause 9 — Performance evaluation (internal audit, management review).
- Clause 10 — Improvement.

You need evidence against each clause, not just against Annex A.

#### Statement of Applicability (SoA)

A document listing all 93 Annex A controls, whether each **applies** to you, its **justification**, and the **implementation status**. The SoA is the backbone of an ISO 27001 audit.

#### Getting certified

- Stage 1 audit — document review.
- Stage 2 audit — implementation evidence.
- **Certification valid 3 years.**
- Surveillance audits years 1 and 2; recertification year 3.

### The ISO 27000 family you'll meet

- **ISO 27002** — implementation guidance for Annex A controls.
- **ISO 27017** — cloud-security extension.
- **ISO 27018** — protection of PII in public clouds (processor perspective).
- **ISO 27701** — Privacy Information Management System (PIMS). Extends 27001; GDPR-aligned.
- **ISO 27005** — risk management guidance.
- **ISO 27035** — incident management.
- **ISO 27036** — supplier relationships.
- **ISO 27040** — storage security.
- **ISO 27701** — privacy (again, worth repeating).
- **ISO 22301** — Business Continuity Management System (BCMS).
- **ISO 9001** — Quality Management System (QMS). Not security, but you'll see it stacked with ISO 27001 in mature orgs.
- **ISO/IEC 42001** — AI Management System (new, 2023).

---

## 7.3 NIST Cybersecurity Framework 2.0

A **voluntary framework**, originally for US critical infrastructure; globally adopted. Not a certification — a **common language**.

### The six Functions (CSF 2.0, 2024)

| Function | What it's about |
|----------|-----------------|
| **GOVERN** (new in 2.0) | Cybersecurity strategy, risk management, roles, policies, supply-chain governance. |
| **IDENTIFY** | Asset management, risk assessment, business environment. |
| **PROTECT** | Access control, data security, awareness & training, platform security, maintenance. |
| **DETECT** | Anomalies, continuous monitoring, detection processes. |
| **RESPOND** | Response planning, communications, analysis, mitigation, improvements. |
| **RECOVER** | Recovery planning, improvements, communications. |

Each Function → **Categories** → **Subcategories** → **Implementation Examples**.

### Using CSF — Tiers and Profiles

- **Tiers (1–4)** — maturity of the risk-management *processes*: Partial, Risk-Informed, Repeatable, Adaptive.
- **Profiles** — your current state, your target state, plans to close the gap.

CSF is often layered *on top of* 27001/SOC 2 — as a board-friendly narrative.

---

## 7.4 NIST SP 800-53 & 800-171, CMMC

### SP 800-53 Rev. 5

The **master catalog** of security and privacy controls, originally for US federal systems but widely referenced. Controls grouped into **20 families** (AC, AU, CM, IA, SC, SI, etc.). Baselines: Low / Moderate / High.

### SP 800-171

For **CUI (Controlled Unclassified Information)** handled by US federal contractors. 110 security requirements.

### CMMC (Cybersecurity Maturity Model Certification)

US DoD programme: contractors must certify to levels (1–3) to bid. Builds on 800-171. Relevant if you sell to US defence supply chain.

---

## 7.5 COSO, COBIT, ITIL (governance you'll hear about)

### COSO

**Committee of Sponsoring Organizations** — issues the ERM (Enterprise Risk Management) and internal-controls frameworks. Underpins SOX (Sarbanes-Oxley) compliance in US public companies.

### COBIT (ISACA)

Governance framework for **enterprise IT**. Used by big enterprises and often by IT auditors (CISA exam leans on it).

### ITIL

IT service management (incident, change, problem, release). Strongly overlaps with SOC 2's change-management and incident-management criteria.

### SOX

**Sarbanes-Oxley Act** (US) — requires public companies to attest to internal controls over financial reporting. **SOX IT controls** (Section 404) → often where SOC 1 reports land.

---

## 7.6 Other frameworks and hybrids you'll meet

- **CIS Controls v8** — 18 prioritised controls; excellent **where to start** if you have no framework. Mapped to CSF, ISO, PCI.
- **NIST SP 800-30 / 37 / 39** — risk frameworks (covered in Module 8).
- **CSA CCM / CAIQ** — Cloud Security Alliance's Cloud Controls Matrix; vendor questionnaire.
- **FedRAMP** — US federal cloud authorisation (built on 800-53).
- **HITRUST CSF** — healthcare-centric unified framework.
- **TISAX** — automotive-industry assessment.
- **IRAP (Australia)**, **ISMAP (Japan)**, **C5 (Germany)** — country-specific cloud assurances.
- **Essential Eight (ASD)** — Australian baseline; great for small orgs.
- **NIS2 / DORA** — EU regulations with security requirements for essential services and financial entities.
- **ISO 42001** — AI Management System.
- **SEBI CSCRF, RBI Cyber Security Framework** — India sectoral.

---

## 7.7 Framework cross-mapping — the killer GRC skill

The reason mature orgs don't collapse under audits is that they build controls **once** and map them to many frameworks. Your job in GRC often is producing and maintaining this map.

### Sample cross-map (illustrative)

| Topic | SOC 2 (CC) | ISO 27001:2022 | NIST CSF 2.0 | NIST 800-53 Rev 5 | PCI-DSS v4 | HIPAA Security |
|-------|-----------|---------------|--------------|---------------------|-----------|---------------|
| Access control policy | CC6.1 | A.5.15 | PR.AA-01 | AC-1 | Req 7 | §164.308(a)(4) |
| MFA | CC6.1 | A.8.5 | PR.AA-03 | IA-2(1) | Req 8.4 | §164.312(a)(2)(i) |
| Encryption at rest | CC6.1, CC6.7 | A.8.24 | PR.DS-01 | SC-28 | Req 3 | §164.312(a)(2)(iv) |
| Encryption in transit | CC6.7 | A.8.24 | PR.DS-02 | SC-8 | Req 4 | §164.312(e)(1) |
| Vulnerability mgmt | CC7.1 | A.8.8 | ID.RA-01 | RA-5 | Req 11.3 | §164.308(a)(1)(ii)(B) |
| Patch mgmt | CC7.1 | A.8.8 | PR.PS-02 | SI-2 | Req 6.3 | §164.308(a)(5)(ii)(B) |
| Logging & monitoring | CC7.2 | A.8.15, A.8.16 | DE.CM-01 | AU-2, AU-6 | Req 10 | §164.312(b) |
| Incident response | CC7.3–7.5 | A.5.24–A.5.28 | RS.MA, RS.AN | IR-4, IR-6 | Req 12.10 | §164.308(a)(6) |
| BCP / DR | CC9.1 | A.5.29, A.5.30 | RC.RP | CP-2, CP-9 | Req 12.10.1 | §164.308(a)(7) |
| Vendor risk | CC9.2 | A.5.19–A.5.22 | GV.SC | SR-3 | Req 12.8 | §164.308(b) |
| Risk assessment | CC3.1–3.4 | Clause 6.1 + A.5.4 | ID.RA, GV.RM | RA-3 | Req 12.3 | §164.308(a)(1) |
| Secure development | CC8.1 | A.8.25, A.8.28 | PR.PS-06 | SA-11, SA-15 | Req 6.2 | n/a |

Keep this map in a spreadsheet. When a new framework is added, append a column. When a control changes, update one cell, and it propagates to evidence across audits.

### Use the free NIST CSF informative references

NIST publishes **Informative References** that cross-walk CSF to 800-53, ISO 27001, COBIT, and more. Use these; don't rebuild.

---

## 7.8 Which framework should we start with?

### Pattern for an early-stage SaaS

1. **CIS Controls v8** as your baseline implementation.
2. **SOC 2 Type II** (Security + Availability + Confidentiality) as your first customer-facing audit (US-centric buyers).
3. **ISO 27001:2022** as your second (required by European, Indian, ME/APAC buyers).
4. Add **ISO 27701** for privacy extension (demonstrates GDPR/DPDP alignment).
5. Add **PCI-DSS SAQ A** if you touch payments (usually solved by outsourcing to Stripe/Razorpay).
6. Add **HIPAA** if US health data; **HDS** if French health data.
7. Track **NIST CSF 2.0** for board narrative and internal improvement.

### Pattern for an India-HQ SaaS with India customers

1. CIS Controls + **DPDP readiness**.
2. **ISO 27001:2022** as the flagship.
3. **CERT-In compliance** (log retention, 6-hour breach reporting).
4. **SOC 2 Type II** for export.
5. **RBI / SEBI sectoral controls** if BFSI customers.
6. Add GDPR alignment for EU customers.

---

## 7.9 Common beginner mistakes

- Treating SOC 2 as a "checklist." It's not prescriptive — it's a framework of criteria, with your designed controls mapped to them.
- Chasing logos (SOC 2, ISO, PCI, HIPAA) before you have basic security hygiene. You'll pass the audit with ugly compensations but real risk remains.
- Ignoring the management-system clauses of ISO 27001 and focusing only on Annex A. Major NCs come from missed Clauses 4–10.
- Buying a GRC tool before you know your controls. The tool automates evidence; it doesn't invent controls.
- Letting vendors "self-certify" instead of demanding a SOC 2 Type II, ISO certificate, or equivalent assurance.

---

## 7.10 Interview traps

- **Q:** "Difference between SOC 2 Type I and Type II?" Type I = design at a point in time; Type II = design **and** operating effectiveness over a period.
- **Q:** "Is ISO 27001 a certification of security?" No — it's a certification of your **ISMS**. You have a *system* for managing security, not perfect security.
- **Q:** "How many Annex A controls in ISO 27001:2022?" 93 controls in 4 themes.
- **Q:** "Name the five SOC 2 TSC." Security, Availability, Processing Integrity, Confidentiality, Privacy. Security is mandatory.
- **Q:** "What's the difference between NIST CSF and NIST 800-53?" CSF = high-level, outcome-based, voluntary framework. 800-53 = detailed catalog of ~1000 specific controls.

---

## 7.11 Mini-exercise (45 min)

Take the cross-map table above. Pick 5 rows. For each:
1. Write, in one sentence, what "good" looks like.
2. List two evidence artefacts an auditor would accept.

This is the *actual output* a GRC analyst produces every week.

---

## 7.12 Go deeper

- 🏛 [AICPA SOC for Service Organizations](https://www.aicpa-cima.com/topic/audit-assurance/audit-and-assurance-greater-than-soc-2)
- 🏛 [AICPA Trust Services Criteria (2017, revised)](https://www.aicpa-cima.com/resources/download/soc-for-service-organizations-trust-services-criteria)
- 🏛 [ISO.org — 27001:2022](https://www.iso.org/standard/27001)
- 🏛 [NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework) — free PDF + quick-start guides.
- 🏛 [NIST SP 800-53 Rev 5](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final) · [SP 800-171](https://csrc.nist.gov/pubs/sp/800/171/r3/final)
- 🏛 [CIS Controls v8](https://www.cisecurity.org/controls)
- 🏛 [CSA CCM / CAIQ](https://cloudsecurityalliance.org/research/cloud-controls-matrix)
- 📰 [Vanta SOC 2 guide (free)](https://www.vanta.com/collection/soc-2) · [Secureframe SOC 2 Hub](https://secureframe.com/hub/soc-2) · [Drata GRC Central](https://drata.com/grc-central)
- 📰 [Advisera free ISO 27001 toolkit samples](https://advisera.com/27001academy/free-downloads/) · [ISMS.online Annex A guide](https://www.isms.online/iso-27001/annex-a/)
- 🏛 [CSF Informative References (cross-mappings)](https://www.nist.gov/cyberframework/informative-references)
- 📰 [Secure Controls Framework (free cross-map)](https://securecontrolsframework.com/)

---

## Module 7 — Glossary recap

SOC 1, SOC 2, SOC 3, SSAE 18, AICPA, Trust Services Criteria (TSC), Common Criteria (CC), Type I vs Type II, CPA, System description, Section 4 exceptions, ISO 27001:2022, ISMS, Annex A themes (Organisational/People/Physical/Technological), Clauses 4–10, PDCA, Statement of Applicability (SoA), Stage 1 / Stage 2 audits, Surveillance audit, ISO 27002, 27017, 27018, 27701, 22301, 9001, 42001, NIST CSF 2.0, Govern/Identify/Protect/Detect/Respond/Recover, Tiers, Profiles, NIST SP 800-53 Rev 5, 800-171, CMMC, COSO, COBIT, ITIL, SOX, CIS Controls v8, CSA CCM, CAIQ, FedRAMP, HITRUST, TISAX, IRAP, ISMAP, C5, Essential Eight, NIS2, DORA, SEBI CSCRF, RBI CSF, Framework cross-map.

→ Next: [Module 8 — Risk Management in Practice](08-risk-management.md)

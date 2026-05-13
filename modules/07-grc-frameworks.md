# Module 7: GRC Frameworks (SOC, ISO, NIST, and friends)

> **Audience:** 🟡 🔴 · **Time:** ~2 hours · **Prereqs:** Modules 0–6

## Why this matters

Frameworks are the vocabulary of GRC. Every customer questionnaire, every board slide, every auditor conversation references one of them. If you know SOC 2, ISO 27001, and NIST CSF, you can read almost any other standard at speed.

A strong opinion to start with: **stop collecting certifications**. Pick the one your customers ask for. Add the next one only when a deal hinges on it. I have watched seed-stage startups burn six figures chasing three frameworks in parallel and then fail their first real customer audit because they never built the underlying controls properly.

We'll cover:

1. **SOC 1 vs SOC 2 vs SOC 3.**
2. **ISO/IEC 27001:2022** and family (27002, 27017, 27018, 27701, 22301, 9001, 42001).
3. **NIST Cybersecurity Framework 2.0.**
4. **NIST SP 800-53 & 800-171, CMMC.**
5. **COSO, COBIT, ITIL, SOX.**
6. **Framework cross-mapping**: the single most useful GRC skill.
7. **Which one should we start with?**

---

## 7.1 SOC 1 vs SOC 2 vs SOC 3: the AICPA suite

"SOC" stands for **System and Organization Controls**. Issued under AICPA standards (SSAE 18). Performed by licensed **CPA firms**.

### SOC 1: controls over **financial reporting**

For your customers' auditors. If your service affects a customer's financial statements (payroll, billing, ERP, transaction processing), their auditor needs assurance on your controls.

- Criteria: ICFR. Not a fixed checklist. You define the control objectives relevant to financial reporting.
- Audience: customer's external auditors.
- Often confused with SOC 2. Different audience, different purpose.

### SOC 2: controls over the **Trust Services Criteria**

The famous one. Five **Trust Services Criteria**:

- **Security** (mandatory, the "Common Criteria").
- **Availability** (optional).
- **Processing Integrity** (optional).
- **Confidentiality** (optional).
- **Privacy** (optional).

Most SaaS start with Security plus Availability plus Confidentiality. Don't add Privacy unless you actually want SOC 2 to be your privacy story. Usually GDPR/DPDP work covers it better.

#### Common Criteria (CC) series

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

Individual criteria like **CC6.1** are what you provide evidence against.

#### Type I vs Type II

- **Type I**: design of controls at a point in time. Cheap and fast. Weak signal. Customers know this.
- **Type II**: design *and* operating effectiveness over a period. 3 to 12 months. Most customers demand 6 to 12. This is the one to aim for.

Going Type I first is fine if a customer asks specifically. Otherwise skip it and plan straight for Type II. You'll save money and look more credible.

#### What's in the SOC 2 report

- Section 1: Independent auditor's report (the opinion).
- Section 2: Management's assertion.
- Section 3: System description.
- Section 4: Trust Services Criteria, Controls, Tests performed, **Results (including exceptions)**.
- Section 5: Other information (optional).

When someone sends you their SOC 2 report, **Section 4** is where the real signal is. Most people read Section 1 and stop. Don't.

### SOC 3

Public-facing summary of SOC 2. No detailed test results. Useful for marketing pages and to dodge NDA dances with smaller prospects. Most mature vendors publish a SOC 3 alongside the SOC 2.

### SOC for Cybersecurity and SOC for Supply Chain

Niche variants. Know the names. You'll rarely meet them outside large enterprise deals.

### How to kick off a SOC 2

1. **Scope**: which product, environments, supporting functions.
2. **Gap assessment**: current controls vs TSC.
3. **Remediation**: close gaps. Policies, tooling, training.
4. **Select auditor**: licensed CPA firm. Typical cost $20k to $80k.
5. **Readiness assessment**: optional, usually worth it for first audits.
6. **Observation period**: 3 to 12 months for Type II.
7. **Fieldwork plus reporting.**
8. **Post-audit**: share with customers, plan next year.

The biggest mistake at step 1 is over-scoping. Include only what customers actually ask about. Adding "all internal IT" to your scope inflates cost without giving customers anything they wanted.

---

## 7.2 ISO/IEC 27001:2022 + family

### ISO 27001: the global ISMS standard

An **Information Security Management System** is a documented system of policies, processes, roles, and controls. ISO 27001 describes the *management system*. Annex A lists the *controls*.

#### 2013 → 2022 update

- 93 controls (down from 114), reorganised into **4 themes**: **Organisational (37), People (8), Physical (14), Technological (34)**.
- Added: threat intel, ICT for BCP, data leakage prevention, cloud services, secure coding.
- By 2025 most certificates are on the 2022 version. If you see a 2013 certificate, ask when they're transitioning.

#### ISMS clauses (the management-system part)

Clauses 4 to 10 define the PDCA loop:

- 4: Context of the organisation.
- 5: Leadership.
- 6: Planning (incl. risk assessment).
- 7: Support.
- 8: Operation.
- 9: Performance evaluation (internal audit, management review).
- 10: Improvement.

You need evidence against each clause, not just Annex A. This is where most first-time ISO audits get Major NCs. Teams pour effort into Annex A, then have nothing to show on internal audit or management review minutes.

#### Statement of Applicability (SoA)

Lists all 93 Annex A controls. For each: applies, justification, implementation status. The SoA is the backbone of the audit. Auditors will work from it. Keep it current; outdated SoAs are the most common minor NC.

#### Getting certified

- Stage 1: document review.
- Stage 2: implementation evidence.
- **Certificate valid 3 years.**
- Surveillance audits years 1 and 2. Recertification year 3.

### The ISO 27000 family you'll meet

- **ISO 27002**: implementation guidance for Annex A controls.
- **ISO 27017**: cloud-security extension.
- **ISO 27018**: protection of PII in public clouds (processor perspective).
- **ISO 27701**: Privacy Information Management System (PIMS). Extends 27001. GDPR-aligned.
- **ISO 27005**: risk management guidance.
- **ISO 27035**: incident management.
- **ISO 27036**: supplier relationships.
- **ISO 27040**: storage security.
- **ISO 22301**: Business Continuity Management System.
- **ISO 9001**: Quality Management System. Not security, but you'll see it stacked with 27001 in mature orgs.
- **ISO/IEC 42001**: AI Management System (2023). New, increasingly asked about.

---

## 7.3 NIST Cybersecurity Framework 2.0

A voluntary framework, originally for US critical infrastructure, now globally adopted. Not a certification. A common language.

### The six Functions (CSF 2.0, 2024)

| Function | What it's about |
|----------|-----------------|
| **GOVERN** (new in 2.0) | Strategy, risk management, roles, policies, supply-chain governance. |
| **IDENTIFY** | Asset management, risk assessment, business environment. |
| **PROTECT** | Access control, data security, training, platform security, maintenance. |
| **DETECT** | Anomalies, continuous monitoring, detection processes. |
| **RESPOND** | Response planning, communications, analysis, mitigation, improvements. |
| **RECOVER** | Recovery planning, improvements, communications. |

Each Function → Categories → Subcategories → Implementation Examples.

### Using CSF: Tiers and Profiles

- **Tiers (1–4)**: maturity of the risk-management *processes*: Partial, Risk-Informed, Repeatable, Adaptive.
- **Profiles**: current state, target state, plan to close the gap.

CSF works well as the board-facing narrative layered on top of 27001 or SOC 2. Boards understand "we are Tier 2 moving to Tier 3" better than "we have an A.8.16 control."

---

## 7.4 NIST SP 800-53 & 800-171, CMMC

### SP 800-53 Rev. 5

The master catalog of security and privacy controls. US federal origin, widely referenced. Controls grouped into 20 families (AC, AU, CM, IA, SC, SI, etc.). Baselines: Low, Moderate, High.

### SP 800-171

For **CUI** handled by US federal contractors. 110 security requirements.

### CMMC

US DoD programme. Contractors certify to levels 1 to 3 to bid. Builds on 800-171. Relevant only if you sell to the US defence supply chain.

---

## 7.5 COSO, COBIT, ITIL, SOX

### COSO

**Committee of Sponsoring Organizations**. Issues the ERM and internal-controls frameworks. Underpins SOX compliance in US public companies.

### COBIT (ISACA)

Governance framework for enterprise IT. Big enterprises and IT auditors use it. The CISA exam leans on it heavily.

### ITIL

IT service management. Incident, change, problem, release. Overlaps heavily with SOC 2's change-management and incident-management criteria.

### SOX

**Sarbanes-Oxley Act** (US). Public companies attest to internal controls over financial reporting. SOX IT controls under Section 404 often end up scoped into SOC 1.

---

## 7.6 Other frameworks and hybrids you'll meet

- **CIS Controls v8**: 18 prioritised controls. The cleanest "where do I start" if you have nothing. Maps to CSF, ISO, PCI.
- **NIST SP 800-30 / 37 / 39**: risk frameworks (Module 8).
- **CSA CCM / CAIQ**: Cloud Security Alliance Cloud Controls Matrix. Vendor questionnaire.
- **FedRAMP**: US federal cloud authorisation.
- **HITRUST CSF**: healthcare-centric unified framework. Expensive, but US health customers love it.
- **TISAX**: automotive-industry assessment.
- **IRAP (Australia)**, **ISMAP (Japan)**, **C5 (Germany)**: country-specific cloud assurances.
- **Essential Eight (ASD)**: Australian baseline. Good for small orgs.
- **NIS2 / DORA**: EU regulations for essential services and financial entities. DORA in particular bites if you're in the EU financial supply chain.
- **ISO 42001**: AI Management System.
- **SEBI CSCRF, RBI Cyber Security Framework**: India sectoral.

---

## 7.7 Framework cross-mapping: the killer GRC skill

Mature orgs don't collapse under audits because they build controls **once** and map them to many frameworks. Your job in GRC is often producing and maintaining this map.

### Sample cross-map (illustrative)

| Topic | SOC 2 (CC) | ISO 27001:2022 | NIST CSF 2.0 | NIST 800-53 Rev 5 | PCI-DSS v4.0.1 | HIPAA Security |
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

Keep this map in a spreadsheet. Add a column when a framework joins. Update one cell when a control changes; evidence propagates everywhere.

### Use the free NIST CSF informative references

NIST publishes **Informative References** that cross-walk CSF to 800-53, ISO 27001, COBIT, and more. Use these. Don't rebuild what NIST has already published for free.

---

## 7.8 Which framework should we start with?

Pick by who's buying.

### Pattern for an early-stage SaaS

1. **CIS Controls v8** as your implementation baseline.
2. **SOC 2 Type II** (Security + Availability + Confidentiality) as the first customer-facing audit if your buyers are US-centric.
3. **ISO 27001:2022** as the second, often required by European, Indian, ME, and APAC buyers.
4. Add **ISO 27701** for privacy extension only if customers ask. Otherwise GDPR/DPDP work covers it.
5. **PCI-DSS SAQ A** if you touch payments. Outsource to Stripe or Razorpay and stay in SAQ A.
6. **HIPAA** only when US health data shows up. **HDS** only when French health data shows up.
7. **NIST CSF 2.0** as the internal narrative for the board.

### Pattern for an India-HQ SaaS with India customers

1. CIS Controls plus **DPDP readiness**.
2. **ISO 27001:2022** as the flagship.
3. **CERT-In compliance** (log retention, 6-hour breach reporting). This is non-negotiable.
4. **SOC 2 Type II** when you start exporting to the US.
5. **RBI / SEBI** controls if BFSI customers are in scope. These are heavier than DPDP.
6. GDPR alignment when EU customers appear.

---

## 7.9 Common beginner mistakes

- Treating SOC 2 as a checklist. It is criteria, not a checklist. You design controls and map them.
- Chasing logos before basic hygiene exists. You can pass an audit with ugly compensations and still have real risk on the books.
- Ignoring ISO 27001 management-system clauses (4–10) and focusing only on Annex A. Most Major NCs come from missed clauses.
- Buying a GRC tool before knowing your controls. The tool collects evidence. It doesn't invent controls.
- Letting vendors self-certify instead of demanding a SOC 2 Type II, ISO certificate, or equivalent.
- Running three audits in parallel with a two-person team. You'll fail one of them.

---

## 7.10 Interview traps

- **Q:** "Difference between SOC 2 Type I and Type II?" Type I = design at a point in time. Type II = design **and** operating effectiveness over a period.
- **Q:** "Is ISO 27001 a certification of security?" No. It certifies your **ISMS**. You have a system for managing security, not perfect security.
- **Q:** "How many Annex A controls in ISO 27001:2022?" 93 in 4 themes.
- **Q:** "Name the five SOC 2 TSC." Security, Availability, Processing Integrity, Confidentiality, Privacy. Security is mandatory.
- **Q:** "Difference between NIST CSF and NIST 800-53?" CSF is high-level, outcome-based, voluntary. 800-53 is the detailed catalog of around 1000 specific controls.

---

## 7.11 Mini-exercise (45 min)

Take the cross-map table above. Pick 5 rows. For each:
1. Write, in one sentence, what "good" looks like.
2. List two evidence artefacts an auditor would accept.

This is the actual output a GRC analyst produces every week.

---

## 7.12 Go deeper

- 🏛 [AICPA SOC for Service Organizations](https://www.aicpa-cima.com/topic/audit-assurance/audit-and-assurance-greater-than-soc-2)
- 🏛 [AICPA Trust Services Criteria (2017, revised)](https://www.aicpa-cima.com/resources/download/soc-for-service-organizations-trust-services-criteria)
- 🏛 [ISO.org: 27001:2022](https://www.iso.org/standard/27001)
- 🏛 [NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework)
- 🏛 [NIST SP 800-53 Rev 5](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final) · [SP 800-171](https://csrc.nist.gov/pubs/sp/800/171/r3/final)
- 🏛 [CIS Controls v8](https://www.cisecurity.org/controls)
- 🏛 [CSA CCM / CAIQ](https://cloudsecurityalliance.org/research/cloud-controls-matrix)
- 📰 [Vanta SOC 2 guide (free)](https://www.vanta.com/collection/soc-2) · [Secureframe SOC 2 Hub](https://secureframe.com/hub/soc-2) · [Drata GRC Central](https://drata.com/grc-central)
- 📰 [Advisera free ISO 27001 toolkit samples](https://advisera.com/27001academy/free-downloads/) · [ISMS.online Annex A guide](https://www.isms.online/iso-27001/annex-a/)
- 🏛 [CSF Informative References (cross-mappings)](https://www.nist.gov/cyberframework/informative-references)
- 📰 [Secure Controls Framework (free cross-map)](https://securecontrolsframework.com/)

---

## Module 7: Glossary recap

SOC 1, SOC 2, SOC 3, SSAE 18, AICPA, Trust Services Criteria (TSC), Common Criteria (CC), Type I vs Type II, CPA, System description, Section 4 exceptions, ISO 27001:2022, ISMS, Annex A themes (Organisational/People/Physical/Technological), Clauses 4–10, PDCA, Statement of Applicability (SoA), Stage 1 / Stage 2 audits, Surveillance audit, ISO 27002, 27017, 27018, 27701, 22301, 9001, 42001, NIST CSF 2.0, Govern/Identify/Protect/Detect/Respond/Recover, Tiers, Profiles, NIST SP 800-53 Rev 5, 800-171, CMMC, COSO, COBIT, ITIL, SOX, CIS Controls v8, CSA CCM, CAIQ, FedRAMP, HITRUST, TISAX, IRAP, ISMAP, C5, Essential Eight, NIS2, DORA, SEBI CSCRF, RBI CSF, Framework cross-map.

→ Next: [Module 8: Risk Management in Practice](08-risk-management.md)

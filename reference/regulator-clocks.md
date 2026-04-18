# Regulator Clocks — Every Notification Deadline in One Table

> *When something goes wrong, you do not have time to look up the law. Print this. Pin it. Memorise the ones that apply to you.*

**Last verified:** 17 April 2026.

---

## Breach / incident notification clocks

| Regulator / Law | Clock | Notify whom | Trigger | Source / module |
|---|---|---|---|---|
| **CERT-In Directions 2022 (India)** | **6 hours** | CERT-In (`incident@cert-in.org.in`, Annexure I) | Any of 20 listed cyber incident types being noticed | [Module 6.3](../modules/06-privacy-laws.md), [Module 11.2](../modules/11-incident-response-bcp.md) |
| **GDPR Art. 33 (EU)** | **72 hours** from awareness | Lead Supervisory Authority (One-Stop-Shop) | Personal data breach likely to result in risk to rights and freedoms | [Module 6.1](../modules/06-privacy-laws.md) |
| **GDPR Art. 34 (EU)** | "Without undue delay" | Affected data subjects | Breach **high risk** to rights and freedoms | [Module 6.1](../modules/06-privacy-laws.md) |
| **UK GDPR + DPA 2018** | **72 hours** | ICO (Information Commissioner's Office) | As GDPR | [Module 6.2](../modules/06-privacy-laws.md) |
| **DPDP Act 2023 (India)** | Per DPDP Rules 2025 (operationalising) | Data Protection Board of India (DPBI) + affected Data Principals | Personal data breach | [Module 6.3](../modules/06-privacy-laws.md) |
| **HIPAA Breach Notification Rule (US)** | **60 days** | Affected individuals + HHS OCR; media if > 500 residents of a state | Unsecured PHI breach | [Module 6.5](../modules/06-privacy-laws.md) |
| **PCI-DSS v4.0.1** | "Immediately" | Acquirer + card brands | Suspected/confirmed cardholder data compromise | [Module 6.7](../modules/06-privacy-laws.md) |
| **SEC Cybersecurity Disclosure (US public cos.)** | **4 business days** after materiality determination | SEC via Form 8-K Item 1.05 | Material cybersecurity incident | [Module 11.2](../modules/11-incident-response-bcp.md) |
| **CCPA / CPRA (California)** | "Most expedient time, without unreasonable delay" | Affected residents; AG notice if > 500 CA residents | Breach of unencrypted personal info | [Module 6.4](../modules/06-privacy-laws.md) |
| **PIPL (China)** | "Immediately" / "without delay" | CAC (Cyberspace Administration of China) + affected individuals | Personal info breach | [Module 6.6](../modules/06-privacy-laws.md) |
| **LGPD (Brazil)** | "Reasonable time" (ANPD guidance evolving) | ANPD + affected data subjects | Security incident with risk/relevant damage | [Module 6.6](../modules/06-privacy-laws.md) |
| **US state AGs (general)** | Varies; commonly 30–60 days | State Attorney General, sometimes consumer reporting agencies | State-defined PII breach | [Module 6.4](../modules/06-privacy-laws.md) |
| **NIS2 (EU, essential / important entities)** | Early warning **24h**, incident notification **72h**, final report **1 month** | National CSIRT / competent authority | Significant incident | [Module 7.6](../modules/07-grc-frameworks.md) |
| **DORA (EU financial entities)** | Per RTS (initial, intermediate, final reports on tight timelines) | Lead competent authority | Major ICT-related incident | [Module 7.6](../modules/07-grc-frameworks.md) |
| **RBI Cyber Security Framework (India banks)** | Per RBI circulars (typically 2–6 hours per incident type) | RBI + IDRBT + CERT-In | Cyber incident in regulated entity | [Module 6.3](../modules/06-privacy-laws.md) |
| **SEBI CSCRF (India listed entities)** | Per SEBI circular (operationalised 2024+) | SEBI + relevant exchange | Cyber incident at regulated entity | [Module 6.3](../modules/06-privacy-laws.md) |

---

## Data subject request (DSR) response clocks

| Law | Standard SLA | Extension | Notes |
|---|---|---|---|
| **GDPR Art. 12** | **1 month** | + 2 months for complex requests, with notice | Free of charge except manifestly unfounded/excessive |
| **UK GDPR** | 1 month | + 2 months | As GDPR |
| **DPDP Act 2023** | Per DPDP Rules (operationalising) | n/a | Grievance redressal mechanism mandatory |
| **CCPA / CPRA** | **45 days** | + 45 days with notice | Up to 12-month lookback |
| **HIPAA** | **30 days** access right | + 30 days once with notice | |
| **LGPD** | **15 days** for confirmation/access | n/a | Other rights "reasonable time" |
| **PIPEDA (Canada)** | **30 days** | + 30 days with notice | |

---

## Audit and certification cycle clocks (for context, not incidents)

| Activity | Frequency | Notes |
|---|---|---|
| **SOC 2 Type II observation period** | 3–12 months | Most customers expect 6–12 month period |
| **ISO 27001 certification cycle** | 3 years | Surveillance audits years 1 and 2; recertification year 3 |
| **PCI-DSS** | Annual SAQ or ROC | Quarterly ASV scans for SAQ A-EP/D |
| **HITRUST CSF** | 2 years (i1) or 2 years (r2) | Interim assessment year 1 |
| **Internal audit (ISO 27001 Clause 9.2)** | At least annual | Risk-based scope |
| **Management review (ISO 27001 Clause 9.3)** | At least annual | Documented outputs |
| **DPIA review** | On material change to processing | Plus at least annual review for high-risk |
| **BCP / DR testing** | At least annual | Document scope, scenario, gaps, corrective actions |

---

## CERT-In Directions 2022 — operational specifics

The single most under-known operational clock for India-touching companies:

- **6-hour** notification of any of the 20 listed incident types to CERT-In.
- **180 days** of log retention as a baseline.
- VPN and data-centre obligations (KYC, log retention).
- Pre-drafted **Annexure I** reporting template — keep it ready.

Practical advice from [Module 11.2](../modules/11-incident-response-bcp.md): **build a regulator-notification matrix keyed to data categories and jurisdictions, and pre-draft notification templates.** When the clock starts, you should be filling in blanks, not drafting from scratch.

---

## A note on "the clock starts when…"

Read the law carefully. Different regulators define the trigger differently:

- **GDPR:** clock starts when the controller becomes **aware** of the breach (not when it happened).
- **CERT-In:** clock starts when the incident is **noticed** by the entity.
- **SEC 8-K Item 1.05:** clock starts after **materiality is determined**, not after the incident itself.
- **HIPAA:** 60 days from **discovery** (which can include constructive discovery).

The difference between "occurred" and "aware/noticed/determined" is often days. Document carefully when awareness happened, who confirmed, and how — that timeline is the first thing the regulator asks for.

---

> *In an incident, you will not be reading this for the first time. Read it now, while it is calm.*

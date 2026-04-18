# Module 6 — Privacy Laws Deep Dive

> **Audience:** 🟡 🔴 · **Time:** ~2 hours · **Prereqs:** Module 5

## Why this matters

Privacy laws are *where the money is* in compliance. Fines, litigation, and market-access restrictions all trace back to which laws apply, to whom, and how. You don't need to be a lawyer — you need to be the person in the room who can translate a regulation into an engineering or policy decision.

This module covers, in order:

1. **GDPR** — the world's most cited law.
2. **UK GDPR** — essentially GDPR with UK-specific variations.
3. **DPDP Act 2023 (India)** — the newest and India-critical.
4. **CCPA / CPRA (California)** — the US leader.
5. **US sectoral laws** — HIPAA (health), GLBA (finance), COPPA (kids), FERPA (education).
6. **LGPD (Brazil), PIPL (China), PIPEDA (Canada), PDPA (Singapore/Thailand), POPIA (South Africa), APP (Australia).**
7. **Sectoral rules you'll run into** — PCI-DSS, HDS (France).
8. **The DPO role and supervisory authorities.**

---

## 6.1 GDPR — EU General Data Protection Regulation (in effect since May 2018)

### Scope (the extraterritorial bite)

GDPR applies to:
- Any organisation **established** in the EU processing personal data.
- Any organisation **outside the EU** that either (a) offers goods/services to people in the EU, or (b) monitors their behaviour.

So a US-based SaaS selling to EU customers is in scope. An Indian e-commerce site with "ship to Germany" is in scope.

### Roles

- **Data Subject** — the individual.
- **Controller** — the org that decides *why* and *how* data is processed.
- **Processor** — processes data on behalf of the controller (most SaaS vendors).
- **Sub-processor** — the processor's processor (e.g., AWS for your SaaS).
- **Joint controllers** — share decision-making.
- **DPO (Data Protection Officer)** — mandatory for public authorities, large-scale systematic monitoring, or large-scale special-category processing.

**Key implication:** when you buy a SaaS tool, *you* are the controller, *they* are the processor — and you need a **Data Processing Agreement (DPA)** covering Art. 28 requirements.

### Core principles (Art. 5) — every auditor checks these

1. Lawfulness, fairness, transparency.
2. Purpose limitation.
3. Data minimisation.
4. Accuracy.
5. Storage limitation.
6. Integrity and confidentiality.
7. Accountability (you must be able to *demonstrate* compliance).

### Important articles

| Article | Topic |
|---------|-------|
| Art. 5 | Principles |
| Art. 6 | Lawful basis |
| Art. 7 | Conditions for consent |
| Art. 9 | Special-category data |
| Art. 12–23 | Data subject rights |
| Art. 25 | Data protection by design and default |
| Art. 28 | Processor contracts |
| Art. 30 | Records of processing activities (ROPA) |
| Art. 32 | Security of processing |
| Art. 33 | Breach notification to authority (**within 72 hours**) |
| Art. 34 | Breach notification to data subject |
| Art. 35 | DPIA |
| Art. 37–39 | DPO |
| Art. 44–49 | International transfers |

### Enforcement

- Supervisory Authorities (one per member state). **Lead Authority** for multi-country processing via the One-Stop-Shop.
- **Fines**: up to **€20 million** or **4% of global annual turnover**, whichever is higher.
- Notable fines: Meta (€1.2B, 2023 — transfers); Amazon (€746M, 2021); WhatsApp (€225M, 2021 — transparency); LinkedIn (€310M, 2024 — behavioural ads).

### Practical company checklist

- ROPA (records of processing) — current.
- Lawful basis documented per processing purpose.
- Privacy notice meeting Art. 13/14 content requirements.
- DSR process with timelines.
- Data Processing Agreements with all processors.
- SCCs + TIAs for non-adequate transfers.
- Breach notification playbook (72-hour clock).
- DPIAs on high-risk processing.
- DPO appointed if required; otherwise a privacy lead.
- Staff training; records of training.

---

## 6.2 UK GDPR + Data Protection Act 2018

Post-Brexit, the UK kept GDPR as "UK GDPR," supplemented by the Data Protection Act 2018. Near-identical to EU GDPR. Regulator: **Information Commissioner's Office (ICO)** — produces some of the **best free guidance** in the world (highly recommended reading even if you're not targeting the UK).

The UK's Data Protection and Digital Information (DPDI) proposals are an evolving reform track — watch for changes.

---

## 6.3 DPDP Act 2023 (India) — critical and under-covered

The **Digital Personal Data Protection Act, 2023** is India's first comprehensive personal data law, notified in August 2023. Rules are being operationalised; the **Data Protection Board of India (DPBI)** is the regulator.

### Scope

- Processing of digital personal data in India.
- Processing of digital personal data *outside* India if it relates to offering goods/services to people in India.

### Key terms

- **Data Principal** — the individual (India's term for data subject).
- **Data Fiduciary** — controller-equivalent.
- **Data Processor** — same as GDPR.
- **Significant Data Fiduciary (SDF)** — designated by government based on volume, sensitivity, risk. Extra obligations (DPO, DPIA, independent audit).
- **Consent Manager** — DPDP's novel concept: registered intermediaries that help individuals manage consents across fiduciaries.

### Principles and requirements

- **Consent** is the default basis; clear, informed, specific.
- **Legitimate uses** — limited category (employment, public interest, medical emergencies).
- **Notice** — at or before collection, in English or any of the 22 scheduled languages.
- **Purpose limitation** + **data minimisation**.
- **Children** (under 18) — verifiable parental consent; no tracking/targeted ads.
- **Persons with disabilities** — consent via lawful guardian.
- **Data principal rights** — access, correction, erasure, grievance redressal, nomination (right to pass on).
- **Breach notification** — to DPBI and affected individuals (timelines per Rules).
- **Cross-border transfers** — permitted unless the transferring country is on a government-notified restricted list.

### Penalties (financial)

- Up to **₹250 crore** per instance for failure to protect data.
- ₹200 crore for failure to notify breach.
- ₹50 crore for failure re children's data.

### India-specific layers you must also consider

- **CERT-In Directions (2022)** — cyber incident reporting to CERT-In **within 6 hours** of noticing; 180-day log retention; specific VPN/data-centre obligations.
- **IT Act 2000 + IT Rules 2011** — pre-DPDP, still relevant for SPDI, intermediary guidelines.
- **RBI** — Cyber Security Framework, Digital Payment Security Controls, outsourcing guidelines, data-localisation for payment-system data.
- **SEBI CSCRF 2024** — Cyber Security and Cyber Resilience Framework for regulated entities.
- **IRDAI**, **TRAI**, **NHA/ABDM** for health data in India.
- **DPDP Rules 2025** (under finalisation) — operational details.

---

## 6.4 CCPA / CPRA — California's leader

### Applicability

Applies to for-profit businesses processing California residents' data that meet **one** of:
- Annual gross revenue > $25M.
- Buys, sells, or shares personal info of ≥ 100,000 consumers/households.
- Derives ≥ 50 % of revenue from selling or sharing personal info.

### Key consumer rights

- **Right to know** (access).
- **Right to delete.**
- **Right to correct.**
- **Right to opt out of sale or sharing** (sharing for cross-context behavioural advertising is specifically called out).
- **Right to limit use of sensitive personal info (SPI).**
- **Right to non-discrimination** for exercising rights.

### Operational hooks

- **"Do Not Sell or Share My Personal Information"** link on homepage.
- **Global Privacy Control (GPC)** signal — honoured as opt-out.
- **Service provider / contractor / third-party** distinctions in contracts.
- **California Privacy Protection Agency (CPPA)** enforces, plus the Attorney General.

### Other US states (growing)

Virginia (VCDPA), Colorado (CPA), Connecticut (CTDPA), Utah (UCPA), Texas (TDPSA), Oregon (OCPA), Montana, Iowa, Indiana, Tennessee, Delaware, New Jersey, New Hampshire, Minnesota, Maryland, Rhode Island (more every year). Most mirror CCPA concepts but differ on: private right of action, cure periods, sensitive data, universal opt-out signals. Track [IAPP's US State Privacy Legislation Tracker](https://iapp.org/resources/article/us-state-privacy-legislation-tracker/).

---

## 6.5 HIPAA, GLBA, COPPA, FERPA (US sectoral)

### HIPAA

- Covers **Protected Health Information (PHI)** held by **Covered Entities** (providers, plans, clearinghouses) and **Business Associates** (their vendors).
- Three rules: **Privacy Rule**, **Security Rule** (administrative + physical + technical safeguards), **Breach Notification Rule** (notify affected individuals + HHS within **60 days**).
- Requires **Business Associate Agreements (BAAs)** with vendors handling PHI.
- Enforced by HHS OCR.

### GLBA (Gramm-Leach-Bliley)

Financial privacy: requires financial institutions to provide privacy notices and to implement safeguards for customer financial data. Safeguards Rule updated 2021 to modernise.

### COPPA

Children's Online Privacy Protection — verifiable parental consent for children under 13. Applies to sites directed at children or with actual knowledge of child users.

### FERPA

Education records — protects student records in federally-funded schools. Important for EdTech platforms selling into US schools.

---

## 6.6 Other major regimes (short tour)

- **LGPD (Brazil)** — GDPR-like; regulator ANPD; fines up to 2% of Brazilian revenue capped at R$50M per violation.
- **PIPL (China)** — strict; data localisation; separate consent for sensitive data; cross-border transfer requires security assessment or certification; separate **Data Security Law** and **Cybersecurity Law**.
- **PIPEDA (Canada)** — federal; some provinces have their own (Quebec's Law 25 is notably strict).
- **PDPA (Singapore)** — clear rules; PDPC enforces.
- **PDPA (Thailand)** — GDPR-inspired.
- **POPIA (South Africa)** — GDPR-inspired; Information Regulator.
- **APP (Australia)** — Australian Privacy Principles under the Privacy Act 1988.

### The compare-by-topic trick

Instead of memorising each law, build a **topic × law matrix**:

| Topic | GDPR | DPDP | CCPA | PIPL | LGPD |
|-------|------|------|------|------|------|
| Extraterritorial? | Yes | Yes | Yes (CA residents) | Yes | Yes |
| Consent-by-default? | No (6 bases) | Mostly yes | No | Yes | Yes |
| Breach notice | 72h to DPA | To DPBI | AG notice if > 500 residents | CAC notice | to ANPD "reasonable time" |
| Children threshold | 16 (variable) | 18 | Sensitive treatment | 14 | 12 |
| Cross-border | SCC/BCR/adequacy | Negative list | No specific mechanism | Security assessment | SCC/consent |

When you need to answer "can we do X in country Y?" go straight to this matrix.

---

## 6.7 PCI-DSS (a technical standard, not a law — but audited like one)

Set by the **PCI Security Standards Council** (card brands). Required by contract if you **store, process, or transmit cardholder data**.

Current version: **v4.0** (v4.0.1 updates). Twelve high-level requirements:

1. Install and maintain network security controls.
2. Apply secure configurations.
3. Protect stored cardholder data.
4. Protect cardholder data with strong cryptography during transmission.
5. Protect against malicious software.
6. Develop and maintain secure systems and software.
7. Restrict access to system components and cardholder data by business need-to-know.
8. Identify users and authenticate access.
9. Restrict physical access.
10. Log and monitor all access.
11. Test security of systems and networks regularly.
12. Maintain an information security policy.

### Scoping — the secret to cheap PCI compliance

**Cardholder Data Environment (CDE)** — everywhere card data lives or can be influenced. Minimise CDE. Tokenisation and hosted payment pages (Stripe Checkout, Razorpay Checkout) move you off PCI-DSS entirely for most fields — aim for **SAQ A**.

### SAQ types

- **SAQ A** — fully outsourced (recommended for most startups).
- **SAQ A-EP** — you keep some control over the payment page.
- **SAQ D** — everything else / full requirements.
- **ROC** (Report on Compliance) — for large merchants / service providers, QSA-assessed.

---

## 6.8 HDS — Hébergeur de Données de Santé (France)

A French certification required to host health data of French citizens. Required of cloud providers (AWS, Azure, OVH have HDS), and of services offering health data processing. Built on ISO 27001 + 27018 plus HDS-specific requirements.

If you touch French health data, you need an HDS-certified hosting layer. Most global EdTech platforms won't hit this — but if you ever sell into French clinics, telehealth, or hospitals, it's mandatory.

---

## 6.9 The DPO role

**Data Protection Officer** — mandatory under GDPR Art. 37 in certain cases; mandatory for SDFs under DPDP; recommended regardless at scale.

- Independent; reports to highest management; cannot be dismissed for performing DPO duties.
- Tasks: inform & advise, monitor compliance, advise on DPIAs, cooperate with supervisory authority.
- Can be internal or external (outsourced DPO is common for SMBs).
- **Conflict of interest** — DPO can't also be, say, the CTO who decides processing purposes.

---

## 6.10 Putting it into practice — a multi-jurisdiction decision tree

When designing a new feature:

1. **Who are the users (or data subjects)?** Find the jurisdictions.
2. **What data are we processing?** Check special-category / health / children / financial.
3. **For each jurisdiction:**
   - What's the lawful basis?
   - What notice / consent is required?
   - What rights must be honoured, and within what SLA?
   - What are the cross-border constraints?
   - What breach-notification clocks apply?
4. **Document** lawful basis, DPIA if high-risk, retention, transfer mechanism.
5. **Engineer** consent capture, opt-outs, deletion paths, logging.
6. **Train** frontline staff who handle DSRs.

A feature that's GDPR-safe is usually DPDP-safe is usually CCPA-safe. The hardest jurisdictions in 2026: PIPL (China), DPDP Rules (India — still evolving), post-Schrems-II EU transfers to US.

---

## 6.11 Go deeper

- 🏛 [GDPR-info.eu](https://gdpr-info.eu/) — full text, article by article.
- 🏛 [EDPB guidelines](https://edpb.europa.eu/edpb_en) · [ICO guide](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/) · [CNIL English guides](https://www.cnil.fr/en)
- 🏛 [MeitY DPDP Act](https://www.meity.gov.in/data-protection-framework) · [DSCI DPDP resources](https://www.dsci.in/) · [CERT-In](https://www.cert-in.org.in/) · [RBI circulars](https://www.rbi.org.in/)
- 🏛 [CPPA (California)](https://cppa.ca.gov/) · [IAPP US State Tracker](https://iapp.org/resources/article/us-state-privacy-legislation-tracker/)
- 🏛 [HHS HIPAA portal](https://www.hhs.gov/hipaa/index.html)
- 🏛 [PCI SSC Document Library](https://www.pcisecuritystandards.org/document_library/)
- 🏛 [esante.gouv.fr — HDS](https://esante.gouv.fr/produits-services/hds)
- 📰 [IAPP Global Privacy Directory](https://iapp.org/resources/global-privacy-directory/) · [IAPP free webinars](https://iapp.org/connect/communities/)
- 📰 [OneTrust DataGuidance (free tier)](https://www.dataguidance.com/)

---

## Module 6 — Glossary recap

GDPR, UK GDPR, Controller, Processor, Sub-processor, DPO, ROPA, DPA, SCC, BCR, TIA, One-Stop-Shop, Schrems II, DPDP Act, Data Principal, Data Fiduciary, Significant Data Fiduciary, Consent Manager, DPBI, CERT-In Directions, RBI CSF, SEBI CSCRF, CCPA, CPRA, CPPA, GPC, VCDPA, CPA, HIPAA, PHI, BAA, GLBA, COPPA, FERPA, LGPD, PIPL, PIPEDA, PDPA (Singapore/Thailand), POPIA, APP, PCI-DSS v4, CDE, SAQ A/A-EP/D, ROC, HDS.

→ Next: [Module 7 — GRC Frameworks](07-grc-frameworks.md)

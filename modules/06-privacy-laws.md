# Module 6: Privacy Laws Deep Dive

> **Audience:** 🟡 🔴 · **Time:** ~2 hours · **Prereqs:** Module 5

## Why this matters

Privacy laws are where the fines actually land. Everything else (notices, DPIAs, ROPAs) exists because regulators can hurt you. You don't need a law degree. You need to be the person in the room who can read a clause and tell engineering what to build, or tell sales what they cannot promise.

In my experience the same three things trip people up: confusing lawful basis with consent, getting cross-border transfers wrong, and missing breach clocks because nobody owned them on paper.

This module covers, in order:

1. **GDPR**: the one everyone copies.
2. **UK GDPR**: GDPR with UK regulator quirks.
3. **DPDP Act 2023 (India)**: newest, and the one most underestimated by India teams.
4. **CCPA / CPRA (California)**: the US baseline.
5. **US sectoral laws**: HIPAA, GLBA, COPPA, FERPA.
6. **LGPD, PIPL, PIPEDA, PDPA (SG/TH), POPIA, APP.**
7. **Sectoral standards you'll run into**: PCI-DSS, HDS.
8. **DPO role and supervisory authorities.**

---

## 6.1 GDPR: EU General Data Protection Regulation (since May 2018)

### Scope (the extraterritorial bite)

GDPR applies to:
- Any organisation **established** in the EU processing personal data.
- Any organisation **outside the EU** that (a) offers goods or services to people in the EU, or (b) monitors their behaviour.

A US-based SaaS selling to EU customers is in scope. An Indian e-commerce site with "ship to Germany" is in scope. The "I'm not in the EU" defence does not work and I've watched sales teams learn this the hard way during procurement reviews.

### Roles

- **Data Subject**: the individual.
- **Controller**: the org that decides *why* and *how*.
- **Processor**: processes on behalf of the controller. Most SaaS vendors sit here for their customers' data.
- **Sub-processor**: the processor's processor. AWS, Stripe, Twilio.
- **Joint controllers**: share decision-making. Rare in practice, abused in contracts.
- **DPO**: mandatory for public authorities, large-scale systematic monitoring, or large-scale special-category processing.

A common misunderstanding: teams assume because they "only host data," they're a processor for everything. The moment your product decides *how* to use that data (analytics, training a model, enrichment), you become a controller for that slice. That's a different DPA, different notice, different liability.

When you buy a SaaS tool, *you* are the controller, *they* are the processor. You need a Data Processing Agreement covering Art. 28 requirements. Read the DPA. Do not let procurement sign whatever the vendor sends.

### Core principles (Art. 5)

1. Lawfulness, fairness, transparency.
2. Purpose limitation.
3. Data minimisation.
4. Accuracy.
5. Storage limitation.
6. Integrity and confidentiality.
7. Accountability. You must be able to *demonstrate* compliance, not just claim it.

That last one is what auditors really check. "We do this" is worthless. "We do this and here is the ROPA, the DPIA, the trained staff, and the access log" is the answer.

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

### Lawful basis: where teams go wrong

Six lawful bases under Art. 6. Consent is one of them. Most product teams default to consent because it feels safest, then end up with consent fatigue and a legal mess because they cannot easily revoke or re-prove it.

Usually you want **contract** (the user needs the feature to use the product they paid for) or **legitimate interest** (with a balancing test on file) for routine product processing. Reserve **consent** for marketing, optional analytics, cookies, special-category data, and anything you cannot justify any other way. Document which basis you chose and why. Auditors ask.

### Cross-border transfers: the honest version

Post-Schrems II, transferring EU data to the US sits on the **EU–US Data Privacy Framework** (since July 2023) for certified importers. For everything else: **Standard Contractual Clauses** plus a **Transfer Impact Assessment**. People treat TIAs as a form-filling exercise. Regulators have been clear they are not.

In practice, if your vendor stack includes US cloud providers, you'll do TIAs, document supplementary measures (encryption, key control, pseudonymisation), and update them when the political weather changes. The DPF can be challenged again. Plan for it.

### Enforcement

- Supervisory Authorities, one per member state. Lead authority via the One-Stop-Shop for multi-country processing.
- **Fines**: up to **€20 million** or **4% of global annual turnover**, whichever is higher.
- Big public fines have hit Meta, Amazon, WhatsApp, and LinkedIn over the last few years. Look them up before any board conversation.

### Practical company checklist

- ROPA, current.
- Lawful basis documented per processing purpose.
- Privacy notice meeting Art. 13/14 content requirements.
- DSR process with timelines and an actual owner.
- DPAs signed with all processors, including the ones procurement bought without telling you.
- SCCs and TIAs for non-adequate transfers.
- Breach playbook with the 72-hour clock owned by a named person.
- DPIAs on high-risk processing, not just on the obvious ones.
- DPO appointed if required, otherwise a named privacy lead.
- Training records.

---

## 6.2 UK GDPR + Data Protection Act 2018

Post-Brexit, the UK kept GDPR as "UK GDPR," supplemented by the DPA 2018. Near-identical to EU GDPR. Regulator: **Information Commissioner's Office (ICO)**, which writes some of the clearest free guidance available. I send the ICO links to engineering teams more than any other regulator's content.

UK reform via the Data Protection and Digital Information track keeps shifting. Watch the divergence rather than panicking about it. For most SaaS, EU GDPR compliance covers UK GDPR.

---

## 6.3 DPDP Act 2023 (India): critical and still underestimated

The **Digital Personal Data Protection Act, 2023** is India's first comprehensive personal data law, notified August 2023. Rules are being operationalised. The **Data Protection Board of India (DPBI)** is the regulator.

A lot of India SaaS teams treated DPDP as "GDPR-lite" and got the details wrong. It is not GDPR. Consent is the default. The list of non-consent lawful bases is short and specific.

### Scope

- Processing of digital personal data in India.
- Processing of digital personal data *outside* India if it relates to offering goods or services to people in India.

### Key terms

- **Data Principal**: the individual.
- **Data Fiduciary**: controller-equivalent.
- **Data Processor**: same as GDPR.
- **Significant Data Fiduciary (SDF)**: designated by government based on volume, sensitivity, risk. Extra obligations: DPO, DPIA, independent audit.
- **Consent Manager**: DPDP's distinctive feature. Registered intermediaries that help individuals manage consents across fiduciaries. Watch how this plays out operationally; nobody fully knows yet.

### Principles and requirements

- **Consent** is the default basis. Clear, informed, specific. Withdrawable as easily as it was given.
- **Legitimate uses**: a closed list (employment, public interest, medical emergencies, certain compliance obligations). Do not stretch this.
- **Notice**: at or before collection, in English or any of the 22 scheduled languages.
- **Purpose limitation** and **data minimisation**.
- **Children** (under 18): verifiable parental consent. No tracking, no behavioural ads. The age threshold being 18, not 13 or 16, catches everyone the first time.
- **Persons with disabilities**: consent via lawful guardian.
- **Data principal rights**: access, correction, erasure, grievance redressal, nomination.
- **Breach notification**: to DPBI and affected individuals, timelines per Rules.
- **Cross-border transfers**: permitted unless the country is on a government-notified restricted list. Opposite logic from GDPR's adequacy approach. List has not been published, so today it's effectively permissive. Plan for that to change.

### Penalties (financial)

- Up to **₹250 crore** per instance for failure to protect data.
- ₹200 crore for failure to notify breach.
- ₹50 crore for children's data failures.

### India-specific layers you must also consider

This is the part outsiders miss. DPDP is not the only thing.

- **CERT-In Directions (2022)**: cyber incident reporting to CERT-In **within 6 hours** of noticing. 180-day log retention. Specific VPN and data-centre obligations. The 6-hour clock is the tightest in the world. Whoever owns IR must know this on day one.
- **IT Act 2000 + IT Rules 2011**: pre-DPDP, still in force for SPDI and intermediary guidelines.
- **RBI**: Cyber Security Framework, Digital Payment Security Controls, outsourcing guidelines, data-localisation for payment-system data.
- **SEBI CSCRF 2024**: Cyber Security and Cyber Resilience Framework for regulated entities. Detailed, prescriptive.
- **IRDAI**, **TRAI**, **NHA/ABDM** for health data.
- **DPDP Rules**: operational details under finalisation.

If you sell into Indian BFSI, the RBI/SEBI obligations will hit harder than DPDP itself.

---

## 6.4 CCPA / CPRA: California's leader

### Applicability

Applies to for-profit businesses processing California residents' data that meet **one** of:
- Annual gross revenue > $25M.
- Buys, sells, or shares personal info of ≥ 100,000 consumers/households.
- Derives ≥ 50% of revenue from selling or sharing personal info.

That last threshold quietly catches ad-tech and data brokers regardless of size.

### Key consumer rights

- Right to know (access).
- Right to delete.
- Right to correct.
- Right to opt out of sale or sharing. "Sharing" specifically calls out cross-context behavioural advertising.
- Right to limit use of sensitive personal info.
- Right to non-discrimination for exercising rights.

### Operational hooks

- **"Do Not Sell or Share My Personal Information"** link on homepage.
- **Global Privacy Control** signal honoured as opt-out. This one trips up engineering. The browser sends a header, your site must respect it, and you need a way to test it.
- Service provider, contractor, and third-party distinctions in contracts. These map to GDPR processor/controller, but not exactly. Read your CCPA addendum, not just the GDPR DPA.
- **CPPA** enforces alongside the Attorney General.

### Other US states

Virginia, Colorado, Connecticut, Utah, Texas, Oregon, Montana, Iowa, Indiana, Tennessee, Delaware, New Jersey, New Hampshire, Minnesota, Maryland, Rhode Island, with more each year. Most mirror CCPA but differ on private right of action, cure periods, sensitive data, and universal opt-out. Track [IAPP's US State Privacy Legislation Tracker](https://iapp.org/resources/article/us-state-privacy-legislation-tracker/) rather than memorising state by state.

In practice, build one DSR pipeline that meets the strictest state's requirement and route everything through it. Trying to split processes per state is how you miss deadlines.

---

## 6.5 HIPAA, GLBA, COPPA, FERPA (US sectoral)

### HIPAA

- **Protected Health Information** held by **Covered Entities** (providers, plans, clearinghouses) and **Business Associates** (their vendors).
- Three rules: **Privacy Rule**, **Security Rule** (administrative, physical, technical safeguards), **Breach Notification Rule** (notify individuals and HHS within **60 days**).
- **Business Associate Agreements** are mandatory with any vendor touching PHI. No BAA, no PHI. End of conversation.
- Enforced by HHS OCR.

The common mistake is treating HIPAA as a security standard. It is broader. The Privacy Rule is where uses-and-disclosures errors live, and they're far more common than security failures.

### GLBA

Financial privacy. Notices and the Safeguards Rule, updated 2021. Hits fintechs and anyone in the customer financial data chain.

### COPPA

Verifiable parental consent for children under 13. Applies to sites directed at children or with actual knowledge of child users. The "actual knowledge" trigger gets companies that thought they were B2B and learned otherwise.

### FERPA

Education records in federally-funded schools. Matters for EdTech selling into US schools. Districts will demand specific contract language.

---

## 6.6 Other major regimes (short tour)

- **LGPD (Brazil)**: GDPR-like. Regulator ANPD. Fines up to 2% of Brazilian revenue, capped at R$50M per violation.
- **PIPL (China)**: strict. Data localisation. Separate consent for sensitive data. Cross-border transfer requires security assessment or certification. Companion laws: **Data Security Law** and **Cybersecurity Law**. This is the hardest jurisdiction in the world right now for international SaaS.
- **PIPEDA (Canada)**: federal. Some provinces have their own. Quebec's Law 25 is the strict one.
- **PDPA (Singapore)**: clear, well-written. PDPC enforces.
- **PDPA (Thailand)**: GDPR-inspired.
- **POPIA (South Africa)**: GDPR-inspired. Information Regulator.
- **APP (Australia)**: Australian Privacy Principles under the Privacy Act 1988. Reform is in progress.

### The compare-by-topic trick

Don't memorise each law. Build a **topic × law matrix** you maintain:

| Topic | GDPR | DPDP | CCPA | PIPL | LGPD |
|-------|------|------|------|------|------|
| Extraterritorial? | Yes | Yes | Yes (CA residents) | Yes | Yes |
| Consent-by-default? | No (6 bases) | Mostly yes | No | Yes | Yes |
| Breach notice | 72h to DPA | To DPBI | AG notice if > 500 residents | CAC notice | to ANPD "reasonable time" |
| Children threshold | 16 (variable) | 18 | Sensitive treatment | 14 | 12 |
| Cross-border | SCC/BCR/adequacy | Negative list | No specific mechanism | Security assessment | SCC/consent |

When someone asks "can we do X in country Y?", go straight to this matrix and then to the specific article.

---

## 6.7 PCI-DSS (a contractual standard, audited like a law)

Set by the **PCI Security Standards Council**. Required by contract if you **store, process, or transmit cardholder data**.

Current version: **v4.0.1**. Twelve high-level requirements:

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

### Scoping: the only PCI decision that matters early

**Cardholder Data Environment** is everywhere card data lives or can be influenced. Minimise it. Tokenisation and hosted payment pages (Stripe Checkout, Razorpay Checkout) take you off PCI-DSS for most fields. Aim for **SAQ A**.

The single biggest mistake I see: engineering "just briefly touches" the PAN somewhere in a log, on a debug field, or in an old DB column nobody cleaned up. That one touch drags the whole stack into SAQ D scope. Hunt these aggressively before any PCI work starts.

### SAQ types

- **SAQ A**: fully outsourced. Aim here.
- **SAQ A-EP**: you keep some control over the payment page.
- **SAQ D**: everything else, full requirements.
- **ROC**: Report on Compliance, for large merchants and service providers, QSA-assessed.

---

## 6.8 HDS: Hébergeur de Données de Santé (France)

A French certification required to host health data of French residents. Built on ISO 27001 plus 27018 plus HDS-specific bits. The big cloud providers (AWS, Azure, OVH) have HDS-certified regions.

You'll only meet HDS if you sell into French clinics, telehealth, or hospitals. When it matters, it's a hard requirement and you cannot retrofit it. Pick an HDS-certified hosting layer from day one.

---

## 6.9 The DPO role

Mandatory under GDPR Art. 37 in certain cases. Mandatory for SDFs under DPDP. Useful regardless once you scale.

- Independent. Reports to highest management. Cannot be dismissed for performing DPO duties.
- Tasks: inform and advise, monitor compliance, advise on DPIAs, cooperate with supervisory authority.
- Can be internal or external. Fractional/outsourced DPO is the norm for SMBs and works fine if the person actually understands your product.
- **Conflict of interest** is a real bar. The CTO cannot be the DPO. Neither can a head of marketing. The regulator has ruled on this.

---

## 6.10 Putting it into practice: a multi-jurisdiction decision tree

When designing a new feature:

1. **Who are the data subjects?** Find the jurisdictions.
2. **What data?** Special-category, health, children, financial?
3. For each jurisdiction:
   - Lawful basis?
   - Notice and consent requirements?
   - Rights and SLAs?
   - Cross-border constraints?
   - Breach clocks?
4. **Document** lawful basis, DPIA if high-risk, retention, transfer mechanism.
5. **Engineer** consent capture, opt-outs, deletion paths, logging.
6. **Train** the frontline staff who'll handle DSRs.

A feature that's GDPR-safe is usually DPDP-safe and usually CCPA-safe. The hardest current jurisdictions: PIPL, evolving DPDP Rules, and EU-to-US transfers if the DPF gets challenged again.

---

## 6.11 Go deeper

- 🏛 [GDPR-info.eu](https://gdpr-info.eu/): full text, article by article.
- 🏛 [EDPB guidelines](https://edpb.europa.eu/edpb_en) · [ICO guide](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/) · [CNIL English guides](https://www.cnil.fr/en)
- 🏛 [MeitY DPDP Act](https://www.meity.gov.in/data-protection-framework) · [DSCI DPDP resources](https://www.dsci.in/) · [CERT-In](https://www.cert-in.org.in/) · [RBI circulars](https://www.rbi.org.in/)
- 🏛 [CPPA (California)](https://cppa.ca.gov/) · [IAPP US State Tracker](https://iapp.org/resources/article/us-state-privacy-legislation-tracker/)
- 🏛 [HHS HIPAA portal](https://www.hhs.gov/hipaa/index.html)
- 🏛 [PCI SSC Document Library](https://www.pcisecuritystandards.org/document_library/)
- 🏛 [esante.gouv.fr: HDS](https://esante.gouv.fr/produits-services/hds)
- 📰 [IAPP Global Privacy Directory](https://iapp.org/resources/global-privacy-directory/) · [IAPP free webinars](https://iapp.org/connect/communities/)
- 📰 [OneTrust DataGuidance (free tier)](https://www.dataguidance.com/)

---

## Module 6: Glossary recap

GDPR, UK GDPR, Controller, Processor, Sub-processor, DPO, ROPA, DPA, SCC, BCR, TIA, One-Stop-Shop, Schrems II, DPDP Act, Data Principal, Data Fiduciary, Significant Data Fiduciary, Consent Manager, DPBI, CERT-In Directions, RBI CSF, SEBI CSCRF, CCPA, CPRA, CPPA, GPC, VCDPA, CPA, HIPAA, PHI, BAA, GLBA, COPPA, FERPA, LGPD, PIPL, PIPEDA, PDPA (Singapore/Thailand), POPIA, APP, PCI-DSS v4.0.1, CDE, SAQ A/A-EP/D, ROC, HDS.

→ Next: [Module 7: GRC Frameworks](07-grc-frameworks.md)

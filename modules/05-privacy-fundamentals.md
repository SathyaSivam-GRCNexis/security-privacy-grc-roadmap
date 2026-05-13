# Module 5, Privacy Fundamentals

> **Audience:** 🟢 🟡 🔴 · **Time:** ~90 min · **Prereqs:** Module 0

## Why this matters

Privacy is not a subset of security. It's a parallel, related, partly-overlapping discipline with its own vocabulary, its own laws and its own career path. If you want to work in GRC in 2026 you need both. Privacy regulation has expanded fast in the last five years: GDPR, the DPDP Act (India), CCPA/CPRA, LGPD (Brazil), PIPL (China), PDPA (Singapore and Thailand). Every product is now supposed to be built with privacy in mind from day one. Most aren't.

Strong opinion up front: **data flows matter more than legal definitions**. You can recite GDPR Article 4 word for word and still build a non-compliant product if you don't know where your data goes. Lawyers care about definitions. Engineers care about diagrams. Your job is to live in both worlds, and the diagram is usually where teams stumble first.

By the end of this module you'll know:
- The categories of personal data and how to classify them.
- The **data lifecycle** and where privacy controls apply.
- **Privacy by Design.** The operating philosophy.
- **Lawful basis, consent and purpose limitation.** The legal backbone, and where teams trip up.
- **DSRs / DSARs.** How individuals exercise rights.
- **DPIA / PIA.** Structured privacy risk assessment.
- **Cross-border transfers, anonymisation, pseudonymisation.**

---

## 5.1 What is personal data?

### The core definitions

- **Personal Data (GDPR Art. 4).** Any information relating to an identified or identifiable natural person. Includes the obvious (name, email, phone) and the less obvious (IP address, device ID, cookie ID, location, biometric data, voice recordings, license plate, employee ID).
- **PII (US, NIST terminology).** Personally Identifiable Information. Similar but narrower than GDPR's "personal data".
- **Sensitive / Special Category Data (GDPR Art. 9).** Race, ethnicity, political opinions, religion, union membership, genetic and biometric data, health, sex life, sexual orientation. Requires extra legal basis, usually explicit consent.
- **PHI (HIPAA).** Protected Health Information. A US-specific subset.
- **PCI data.** Cardholder data (PAN, CVV, expiry). Under PCI-DSS.
- **SPDI (India IT Rules 2011).** Sensitive Personal Data or Information: passwords, financial info, health, biometrics, sexual orientation. Still relevant alongside the DPDP Act.

### Tricky cases

- **An IP address.** Personal data under GDPR (ECJ ruling). Many engineers still assume it isn't.
- **Hashed identifiers.** Usually still personal data. You can re-identify with the original.
- **Business email (`riya@company.com`).** Still personal data.
- **Anonymised data.** If truly anonymised (no realistic way to re-identify), no longer personal. Pseudonymised data (user_id replaced with a token) is still personal data.

### Data classification scheme (every company needs one)

| Level | Examples | Typical controls |
|-------|----------|------------------|
| **Public** | marketing pages | integrity matters, confidentiality doesn't |
| **Internal** | org charts, internal wiki | authenticated access |
| **Confidential** | customer lists, financials | need-to-know, encryption |
| **Restricted / Highly Confidential** | sensitive personal data, secrets, source code | strict ACL, encryption at rest and in transit, logging, DLP |

Label everything. Policies, training and DLP tooling all key off classification labels. Honest hedge: in most organisations classification labels are aspirational. Pick four labels and one mandatory rule per label, and you're ahead of the field.

---

## 5.2 The data lifecycle, where privacy controls live

Every piece of personal data moves through these stages. Privacy controls apply at each.

1. **Collect.** What you ask for, from whom, how.
2. **Process / Use.** Analytics, ML training, business operations.
3. **Store.** Databases, backups, logs, caches.
4. **Share / Transfer.** With processors, partners, other regions.
5. **Retain.** How long, why, where.
6. **Delete / Dispose.** Verifiable destruction.

### Control examples per stage

- Collect: consent capture, minimisation ("do we really need DOB?"), notice.
- Process: purpose limitation, access controls, logging.
- Store: encryption, classification, backup encryption.
- Share: DPA contracts, SCCs, vendor assessment.
- Retain: retention schedule, automated purge jobs.
- Delete: verified erasure, certificate of destruction for physical media.

Common mistake: teams focus on "store" (encryption) and ignore "collect" (minimisation) and "delete" (retention). The biggest privacy wins come from not collecting data in the first place. In my experience, a one-hour review of the signup form removes more risk than a quarter of policy work.

---

## 5.3 Privacy by Design (Cavoukian's 7 principles)

Privacy isn't bolted on. It's designed in.

1. Proactive, not reactive. Preventive, not remedial.
2. Privacy as the default.
3. Privacy embedded into design.
4. Full functionality, positive-sum, not zero-sum. (Privacy doesn't mean bad UX.)
5. End-to-end security, full lifecycle protection.
6. Visibility and transparency.
7. Respect for user privacy, keep it user-centric.

(These are quoted from Cavoukian's original framing.) Since 2018 GDPR codified "Data Protection by Design and by Default" (Art. 25) as a legal obligation.

### Operational patterns

- **Data minimisation.** Only collect what you need for the stated purpose.
- **Purpose limitation.** Use data only for the purpose you told the user.
- **Storage limitation.** Don't keep forever. Retention schedule.
- **Transparency.** Easy-to-find privacy notice. Dashboards for users.
- **User control.** Easy preference centre. Clear consent. Opt-outs.
- **Safe defaults.** Opt-in (not opt-out) for non-essential tracking.

### Worked example, redesigning a signup form with PbD

Original form collects name, email, DOB, phone, gender, PIN code for "personalisation".

Questions:
- Why DOB? If age-gating, collect just an "18+?" checkbox (minimisation).
- Why phone? If only for OTP, use email OTP, or delay the ask until needed.
- Gender? If not functionally required, remove. Or make optional, with "prefer not to say" and third-gender options.
- PIN code? For shipping? Delay until first purchase.

Result: signup is faster, consent is cleaner, regulator risk drops, conversion often rises. Common pattern in SaaS: product managers resist removing fields because "marketing wants them". Bring marketing into the room and ask which field actually drives a decision. Most don't.

---

## 5.4 Lawful basis, the "why can you even have this data?"

Under GDPR Article 6 (mirrored in many other laws), processing personal data requires a lawful basis. Six of them:

1. **Consent.** Freely given, specific, informed, unambiguous, revocable. The most overused basis. Required for special-category data and cookies.
2. **Contract.** Necessary to perform a contract with the data subject (e.g., using address to deliver a purchase).
3. **Legal obligation.** Required by law (e.g., retaining tax invoices).
4. **Vital interests.** Matter of life and death. Rare.
5. **Public task.** Official authority. Public-sector.
6. **Legitimate interests.** The business has a legitimate purpose that isn't overridden by the user's rights. Requires a Legitimate Interest Assessment (LIA) document.

Key insight, and where teams genuinely stumble: you pick **one** lawful basis for **each** processing purpose, and you document it. Most teams write "consent" for everything because it feels safe, but then can't show withdrawal mechanics, can't show what happens when consent is refused, and can't show how processing continues to function. That's worse than picking legitimate interest with an LIA. Switching basis mid-processing is perilous.

### Consent done right

- **Granular.** Separate checkboxes for separate purposes (marketing vs analytics vs profiling).
- **Unbundled.** Can't force "accept all" to use the service.
- **Easy withdrawal.** As easy to revoke as to give.
- **Logged.** Evidence of when, what, how, and which version of the notice was shown.
- **Re-ask** on material change.

Most cookie banners fail on "unbundled" and "as easy to withdraw". European DPAs have been fining specifically on these two points for years.

---

## 5.5 Data Subject Rights (DSR/DSAR)

Individuals have rights over their data. Under GDPR, nine (several regimes mirror them):

1. Right to be informed (privacy notice at collection).
2. Right of access (DSAR), a copy of their data.
3. Right to rectification, correct errors.
4. Right to erasure ("right to be forgotten"), delete on valid request.
5. Right to restrict processing.
6. Right to data portability, machine-readable export.
7. Right to object, stop processing (especially marketing).
8. Rights re: automated decision-making, including profiling that significantly affects them.
9. Right to withdraw consent.

### The DSAR workflow

Typical timeline:

1. **Intake.** A dedicated channel (email, portal).
2. **Identity verification.** Don't hand data to an imposter, but don't over-collect just to verify (ironic privacy risk).
3. **Scoping.** What are they asking for? Which systems hold it?
4. **Retrieval.** Pull from prod, logs, backups, analytics, DWH, vendors.
5. **Review.** Redact third-party personal data (someone else's email in a CC field, for example).
6. **Delivery.** Secure channel, portable format.
7. **Record.** Evidence of completion.
8. **Deadline.** Usually 30 days (extendable). CCPA/CPRA: 45 days.

### Tricky scenarios

- **Backups.** You can't easily edit one row in a backup. Convention: apply the change on restore, or document compensating controls.
- **Shared data** (e.g., a message one user sent to another). Erasure may conflict with the other party's rights.
- **Legal hold.** Litigation obligations override erasure. Document the exception.
- **Child data.** Stricter rules. Consent from parent under a certain age.

### Worked example, GDPR deletion request

User requests deletion. Map every store:
- Production DB. Soft-delete, then hard-delete after 30-day grace.
- Analytics DW. Anonymise the user's rows, or drop.
- Email service (SendGrid etc.). SCIM or delete API.
- CRM. Delete contact.
- Support ticketing. Anonymise ticket requester.
- Logs. Rotate out after retention. If you keep long logs, tokenise.
- Backups. Document that data will age out per retention. Don't restore the user.
- BI cubes and ML training sets. Remove or retrain.

Now record: what was deleted, what was retained under what basis, what the user was told.

Operational reality: most teams underestimate how many systems hold a copy of the user. The first deletion request a new SaaS handles usually surfaces 5–10 systems nobody had inventoried. Don't beat yourself up. Treat it as the start of your data map.

---

## 5.6 DPIA / PIA, structured privacy risk assessment

A Data Protection Impact Assessment (GDPR Art. 35) is mandatory when processing is "likely to result in a high risk". Examples: systematic profiling, large-scale processing of special-category data, systematic monitoring of public areas, AI scoring of individuals.

### The DPIA template (what an auditor expects)

1. Project or feature description.
2. Data flow diagram. Sources, processors, destinations, retention.
3. Data inventory. Fields, volumes, sensitivity, lawful basis.
4. Necessity and proportionality. Could we achieve the purpose with less data?
5. Risks to individuals, scored Likelihood × Impact.
6. Mitigations. Planned controls, residual risk, owner.
7. Stakeholder consultation. DPO, legal, security, engineering, sometimes users.
8. Decision. Proceed, modify, or not proceed. Sign-off.

### Do one early, not late

A DPIA two weeks before launch is a political fight. A DPIA at design stage is a healthy checklist. Integrate with your SDLC intake, ideally as a question in the design-review template. If you only get one process change in your first six months as a privacy lead, make it this one.

### Mini-exercise (20 min)

For an EdTech "resume review" feature (users upload a resume, an AI reviews it, a human coach may view it), draft a one-page DPIA using the template above.

---

## 5.7 Cross-border data transfers

Data rarely stays in one country. Regulators care because other countries may not offer equivalent protection.

### GDPR mechanisms for EU → non-EU transfers

- **Adequacy decisions.** The EU Commission has ruled certain countries "adequate" (UK, Japan, South Korea, etc.). Transfers there are like intra-EU.
- **Standard Contractual Clauses (SCCs).** Pre-approved contract templates between exporter and importer. The default mechanism.
- **Binding Corporate Rules (BCRs).** For intra-group transfers inside multinationals.
- **Transfer Impact Assessment (TIA).** Post-Schrems II, you must also assess the destination country's surveillance laws (especially the US).
- **Derogations.** Explicit consent, contract performance. Narrow, not for systematic transfers.

### India DPDP

Allows transfers to any country except a negative list (yet to be issued at time of writing). Sectoral regulators (RBI, IRDAI) impose stricter localisation, and they will overrule the general rule for their sector. Worth knowing if you're working on Indian fintech or insurance.

### Russia, China, Vietnam and others

Data-localisation regimes. Some data must stay in-country.

Implication: your vendor map must show where each vendor hosts data. A US CRM processing EU employee data needs SCCs plus a TIA. In one SaaS environment I worked in, "where does this vendor host?" was unanswered for half the vendor list. Filling that in took longer than rolling out a new tool.

---

## 5.8 Anonymisation, pseudonymisation, tokenisation

These get conflated. They're very different.

| Technique | Reversible? | Still personal data under GDPR? |
|-----------|------------|---------------------------------|
| **Pseudonymisation** (replace identifiers with tokens, keep mapping) | Yes, with the mapping | **Yes** |
| **Tokenisation** (replace value with unrelated token, mapping in a vault) | Yes, with the vault | **Yes** (within vault holder) |
| **Hashing** (one-way with salt) | Theoretically no. Collisions and dictionary attacks reduce the claim | Usually still personal data |
| **Anonymisation** (irreversible, no one can re-identify) | **No** | **No**, out of scope |

True anonymisation is hard. It must resist re-identification through linkage with external datasets. Techniques:

- **k-anonymity.** Each record is indistinguishable from at least k-1 others on quasi-identifiers (age, zip, gender).
- **l-diversity / t-closeness.** Extensions addressing k-anonymity's weaknesses.
- **Differential privacy.** Add calibrated noise so individual contribution can't be inferred. The gold standard for aggregate analytics, used by Apple and the US Census.
- **Synthetic data.** Generate statistically similar fake data.

### Rule of thumb

If you can re-identify a single person with reasonable effort, it's not anonymised. It's pseudonymised at best, and GDPR still applies. I've seen "anonymised" datasets re-identified in a 20-minute exercise by joining with a public LinkedIn search. Don't bet your compliance posture on labels.

---

## 5.9 Cookies, trackers and consent mode

Cookie banners are the most visible privacy artefact on the web.

- **Strictly necessary cookies.** No consent required (login, load-balancer).
- **Functional.** Remember preferences. Usually requires consent in EU.
- **Analytics.** Consent required in EU/UK.
- **Advertising / cross-site tracking.** Consent absolutely required. Default off.

Tools: OneTrust, Cookiebot, Osano, Didomi. They give you a consent management platform (CMP) and consent records.

**IAB TCF** (Transparency and Consent Framework) is the ad-industry standard consent-string format. Know the name.

Google **Consent Mode v2** changes analytics and ads tag behaviour based on the user's consent state. Marketing teams will ask about this.

---

## 5.10 Go deeper

- 🏛 [GDPR full text, article-by-article](https://gdpr-info.eu/)
- 🏛 [ICO (UK), Guide to GDPR plus DPIA template](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/)
- 🏛 [CNIL (France), English guides plus free PIA software](https://www.cnil.fr/en)
- 🏛 [EDPB Guidelines](https://edpb.europa.eu/edpb_en)
- 🏛 [MeitY, DPDP Act text](https://www.meity.gov.in/data-protection-framework) · [DSCI resources](https://www.dsci.in/)
- 📰 [IAPP Glossary](https://iapp.org/resources/glossary/) · [IAPP Resource Center](https://iapp.org/resources/)
- 🏛 [Cavoukian, 7 Foundational Principles PDF](https://www.ipc.on.ca/wp-content/uploads/resources/7foundationalprinciples.pdf)
- 📰 [Harvard Privacy Tools, Differential Privacy primer](https://privacytools.seas.harvard.edu/differential-privacy)
- 📰 [ENISA, Pseudonymisation techniques](https://www.enisa.europa.eu/publications/pseudonymisation-techniques-and-best-practices)
- 🧪 [CNIL PIA software (free)](https://www.cnil.fr/en/open-source-pia-software-helps-carry-out-data-protection-impact-assesment)

---

## Module 5, Glossary recap

Personal Data, PII, Sensitive / Special Category Data, PHI, PCI, SPDI, Data classification, Data lifecycle, Privacy by Design, Data minimisation, Purpose limitation, Storage limitation, Lawful basis, Consent (granular, unbundled, withdrawable), Legitimate Interest Assessment (LIA), DSR / DSAR, Right to erasure, Right to portability, DPIA / PIA, Data Flow Diagram, Adequacy decision, SCC, BCR, TIA, Schrems II, Data localisation, Anonymisation, Pseudonymisation, Tokenisation, k-anonymity, l-diversity, Differential privacy, Synthetic data, CMP, IAB TCF, Consent Mode.

→ Next: [Module 6, Privacy Laws Deep Dive](06-privacy-laws.md)

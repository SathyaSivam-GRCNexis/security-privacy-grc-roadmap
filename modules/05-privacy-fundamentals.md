# Module 5 — Privacy Fundamentals

> **Audience:** 🟢 🟡 🔴 · **Time:** ~90 min · **Prereqs:** Module 0

## Why this matters

Privacy is **not a subset of security** — it's a parallel, related, partly-overlapping discipline with its own vocabulary, its own laws, and its own career path. If you want to work in GRC in 2026 you need both. Privacy regulation has exploded in the last five years: GDPR, DPDP Act (India), CCPA/CPRA, LGPD (Brazil), PIPL (China), PDPA (Singapore/Thailand). Every product must now be built with privacy in mind from day one.

By the end of this module you'll know:
- The categories of personal data and how to classify them.
- The **data lifecycle** and where privacy controls apply.
- **Privacy by Design** — the operating philosophy.
- **Lawful basis, consent, and purpose limitation** — the legal backbone.
- **DSRs / DSARs** — how individuals exercise rights.
- **DPIA / PIA** — structured privacy risk assessment.
- **Cross-border transfers, anonymisation, pseudonymisation.**

---

## 5.1 What is personal data?

### The core definitions

- **Personal Data (GDPR Art. 4)** — *any information* relating to an identified or identifiable natural person. Includes obvious things (name, email, phone) and less obvious (IP address, device ID, cookie ID, location, biometric data, voice recordings, license plate, employee ID).
- **PII (US / NIST terminology)** — Personally Identifiable Information. Similar but narrower than GDPR's "personal data."
- **Sensitive / Special Category Data (GDPR Art. 9)** — race, ethnicity, political opinions, religion, union membership, genetic/biometric data, health, sex life, sexual orientation. Requires *extra* legal basis (usually explicit consent).
- **PHI (HIPAA)** — Protected Health Information. A US-specific subset.
- **PCI data** — cardholder data (PAN, CVV, expiry). Under PCI-DSS.
- **SPDI (India IT Rules 2011)** — Sensitive Personal Data or Information: passwords, financial info, health, biometrics, sexual orientation.

### Tricky cases

- **An IP address** — personal data under GDPR (ECJ ruling). Many engineers still assume it isn't.
- **Hashed identifiers** — usually still personal data (you can re-identify with the original).
- **Business email (`riya@company.com`)** — still personal data.
- **Anonymised data** — if truly anonymised (no way to re-identify), no longer personal. *Pseudonymised* data (e.g., user_id replaced with a token) **is** still personal data.

### Data classification scheme (one every company needs)

| Level | Examples | Typical controls |
|-------|----------|------------------|
| **Public** | marketing pages | integrity matters, confidentiality doesn't |
| **Internal** | org charts, internal wiki | authenticated access |
| **Confidential** | customer lists, financials | need-to-know, encryption |
| **Restricted / Highly Confidential** | sensitive personal data, secrets, source code | strict ACL, encryption at rest + in transit, logging, DLP |

Label everything. Policies, training, and DLP tooling all key off classification labels.

---

## 5.2 The data lifecycle — where privacy controls live

Every piece of personal data moves through these stages. Privacy controls apply at each stage.

1. **Collect** — what you ask for, from whom, how.
2. **Process / Use** — analytics, ML training, business operations.
3. **Store** — databases, backups, logs, caches.
4. **Share / Transfer** — with processors, partners, other regions.
5. **Retain** — how long, why, where.
6. **Delete / Dispose** — verifiable destruction.

### Control examples per stage

- Collect: consent capture; minimisation ("do we really need DOB?"); notice.
- Process: purpose limitation; access controls; logging.
- Store: encryption; classification; backup encryption.
- Share: DPA contracts; SCCs; vendor assessment.
- Retain: retention schedule; automated purge jobs.
- Delete: verified erasure; certificate of destruction (for physical media).

**Common mistake:** teams focus on "store" (encryption) and ignore "collect" (minimisation) and "delete" (retention). The biggest privacy wins come from *not collecting data in the first place*.

---

## 5.3 Privacy by Design (Cavoukian's 7 principles)

Privacy isn't bolted on; it's designed in.

1. **Proactive, not reactive; preventive, not remedial.**
2. **Privacy as the default.**
3. **Privacy embedded into design.**
4. **Full functionality — positive-sum, not zero-sum.** (Privacy doesn't mean bad UX.)
5. **End-to-end security — full lifecycle protection.**
6. **Visibility and transparency.**
7. **Respect for user privacy — keep it user-centric.**

Since 2018 GDPR codified "Data Protection by Design and by Default" (Art. 25) as a legal obligation.

### Operational patterns

- **Data minimisation** — only collect what you need for the stated purpose.
- **Purpose limitation** — use data only for the purpose you told the user.
- **Storage limitation** — don't keep forever; retention schedule.
- **Transparency** — easy-to-find privacy notice; dashboards for users.
- **User control** — easy preference centre; clear consent; opt-outs.
- **Safe defaults** — opt-in (not opt-out) for non-essential tracking.

### Worked example — redesigning a signup form with PbD

Original: collects name, email, DOB, phone, gender, PIN code for "personalisation."

Questions:
- Why DOB? If age-gating, collect just "18+?" checkbox (minimisation).
- Why phone? If for OTP only, use email OTP (or delay the ask until needed).
- Gender? If not functionally required, remove — or make optional, with "prefer not to say" and third-gender options.
- PIN code? For shipping? Delay until first purchase.

Result: signup is faster, consent is cleaner, regulator risk drops, conversion likely rises.

---

## 5.4 Lawful basis — the "why can you even have this data?"

Under GDPR Article 6 (and mirrored in many other laws), processing personal data requires a **lawful basis**. There are six:

1. **Consent** — freely given, specific, informed, unambiguous, revocable. Most-overused basis. Required for special-category data and cookies.
2. **Contract** — necessary to perform a contract with the data subject (e.g., using address to deliver a purchase).
3. **Legal obligation** — required by law (e.g., retaining tax invoices).
4. **Vital interests** — matter of life and death (rare).
5. **Public task** — official authority (public-sector).
6. **Legitimate interests** — the business has a legitimate purpose that isn't overridden by the user's rights. Requires a Legitimate Interest Assessment (LIA) document.

**Key insight:** you pick *one* lawful basis for each processing purpose, and you document it. Switching basis mid-processing is perilous.

### Consent done right

- **Granular** — separate checkboxes for separate purposes (marketing vs analytics vs profiling).
- **Unbundled** — can't force "accept all" to use the service.
- **Easy withdrawal** — as easy to revoke as to give.
- **Logged** — evidence of *when*, *what*, *how*, *what version* of the notice.
- **Re-ask** on material change.

Most cookie banners fail on "unbundled" and "as easy to withdraw."

---

## 5.5 Data Subject Rights (DSR/DSAR)

Individuals have **rights** over their data. Under GDPR, nine (several regimes mirror them):

1. **Right to be informed** — privacy notice at collection.
2. **Right of access (DSAR)** — a copy of their data.
3. **Right to rectification** — correct errors.
4. **Right to erasure ("right to be forgotten")** — delete on valid request.
5. **Right to restrict processing**.
6. **Right to data portability** — machine-readable export.
7. **Right to object** — stop processing (esp. marketing).
8. **Rights re: automated decision-making** — including profiling that significantly affects them.
9. **Right to withdraw consent**.

### The DSAR workflow

A typical timeline:

1. **Intake** — a dedicated channel (email, portal).
2. **Identity verification** — don't hand data to an imposter, but don't over-collect to verify (ironic privacy risk).
3. **Scoping** — what are they asking for? Which systems hold it?
4. **Retrieval** — pull from prod, logs, backups, analytics, DWH, vendors.
5. **Review** — redact third-party personal data (someone else's email in a CC field, e.g.).
6. **Delivery** — secure channel, portable format.
7. **Record** — evidence of completion.
8. **Deadline** — usually 30 days (extendable). CCPA/CPRA: 45 days.

### Tricky scenarios

- **Backups** — you can't easily edit one row in a backup. Convention: apply the change on restore, or document compensating controls.
- **Shared data** (e.g., a message one user sent to another) — erasure may conflict with the other party's rights.
- **Legal hold** — litigation obligations override erasure; document the exception.
- **Child data** — stricter rules; consent from parent under certain age.

### Worked example — GDPR deletion request

User requests deletion. Map every store:
- Production DB — soft-delete → hard-delete after 30-day grace.
- Analytics DW — anonymise the user's rows or drop.
- Email service (SendGrid etc.) — SCIM/delete API.
- CRM — delete contact.
- Support ticketing — anonymise ticket requester.
- Logs — rotate out after retention; if you keep long logs, tokenise.
- Backups — document that data will age out per retention; don't restore the user.
- BI cubes / ML training sets — remove/retrain.

Now record: what was deleted, what was retained under what basis, what the user was told.

---

## 5.6 DPIA / PIA — structured privacy risk assessment

A **Data Protection Impact Assessment** (GDPR Art. 35) is *mandatory* when processing is "likely to result in a high risk." Examples: systematic profiling, large-scale processing of special-category data, systematic monitoring of public areas, AI scoring of individuals.

### The DPIA template (what an auditor expects)

1. **Project / feature description.**
2. **Data flow diagram** — sources → processors → destinations → retention.
3. **Data inventory** — fields, volumes, sensitivity, lawful basis.
4. **Necessity & proportionality** — could we achieve the purpose with less data?
5. **Risks** — to individuals, scored L × I.
6. **Mitigations** — planned controls, residual risk, owner.
7. **Stakeholder consultation** — DPO, legal, security, engineering, sometimes users.
8. **Decision** — proceed, modify, or not proceed; sign-off.

### Do one early, not late

A DPIA two weeks before launch is a political fight. A DPIA at design stage is a healthy checklist. Integrate with your SDLC intake.

### Mini-exercise (20 min)

For the EdTech platform's "resume review" feature (users upload a resume; an AI reviews it; a human coach may view it), draft a one-page DPIA using the template above.

---

## 5.7 Cross-border data transfers

Data rarely stays in one country. Regulators care because other countries may not offer equivalent protection.

### GDPR mechanisms for EU → non-EU transfers

- **Adequacy decisions** — the EU Commission has ruled certain countries "adequate" (UK, Japan, South Korea, etc.); transfers there are like intra-EU.
- **Standard Contractual Clauses (SCCs)** — pre-approved contract templates between exporter and importer. The default mechanism.
- **Binding Corporate Rules (BCRs)** — for intra-group transfers inside multinationals.
- **Transfer Impact Assessment (TIA)** — post-Schrems II, you must also assess the destination country's surveillance laws (especially the US).
- **Derogations** — explicit consent, contract performance (narrow, not for systematic transfers).

### India DPDP

Allows transfers to any country except a negative list (to be issued). Sectoral regulators (RBI, IRDAI) impose stricter localisation.

### Russia, China, Vietnam, etc.

Data-localisation regimes. Some data *must* stay in-country.

**Implication:** your vendor map must show where each vendor hosts data. A US CRM processing EU employee data needs SCCs plus a TIA.

---

## 5.8 Anonymisation, pseudonymisation, tokenisation

These get conflated; they're very different.

| Technique | Reversible? | Still personal data under GDPR? |
|-----------|------------|---------------------------------|
| **Pseudonymisation** (replace identifiers with tokens; keep mapping) | Yes, with the mapping | **Yes** |
| **Tokenisation** (replace value with unrelated token; mapping in a vault) | Yes, with the vault | **Yes** (within vault holder) |
| **Hashing** (one-way with salt) | Theoretically no; collisions + dictionary attacks reduce claim | Usually still personal data |
| **Anonymisation** (irreversible; no one can re-identify) | **No** | **No** — out of scope |

True anonymisation is **hard**. It must resist re-identification through linkage with external datasets. Techniques:

- **k-anonymity** — each record is indistinguishable from at least k-1 others on quasi-identifiers (age, zip, gender).
- **l-diversity / t-closeness** — extensions addressing k-anonymity's weaknesses.
- **Differential privacy** — add calibrated noise so individual contribution can't be inferred. The gold standard for aggregate analytics (used by Apple, US Census).
- **Synthetic data** — generate statistically similar fake data.

### Rule of thumb

If you can re-identify a single person with reasonable effort, it's not anonymised — it's pseudonymised at best, and GDPR still applies.

---

## 5.9 Cookies, trackers and consent mode

Cookie banners are the most visible privacy artefact on the web.

- **Strictly necessary cookies** — no consent required (login, load-balancer).
- **Functional** — remember preferences — usually requires consent in EU.
- **Analytics** — consent required in EU/UK.
- **Advertising / cross-site tracking** — consent absolutely required; default off.

Tools: OneTrust, Cookiebot, Osano, Didomi. They give you a consent management platform (CMP) and consent records.

**IAB TCF** (Transparency and Consent Framework) is the ad-industry standard consent-string format. Know the name.

Google **Consent Mode v2** changes analytics/ads tags' behaviour based on the user's consent state.

---

## 5.10 Go deeper

- 🏛 [GDPR full text, article-by-article](https://gdpr-info.eu/)
- 🏛 [ICO (UK) — Guide to GDPR + DPIA template](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/)
- 🏛 [CNIL (France) — English guides + free PIA software](https://www.cnil.fr/en)
- 🏛 [EDPB Guidelines](https://edpb.europa.eu/edpb_en)
- 🏛 [MeitY — DPDP Act text](https://www.meity.gov.in/data-protection-framework) · [DSCI resources](https://www.dsci.in/)
- 📰 [IAPP Glossary](https://iapp.org/resources/glossary/) · [IAPP Resource Center](https://iapp.org/resources/)
- 🏛 [Cavoukian — 7 Foundational Principles PDF](https://www.ipc.on.ca/wp-content/uploads/resources/7foundationalprinciples.pdf)
- 📰 [Harvard Privacy Tools — Differential Privacy primer](https://privacytools.seas.harvard.edu/differential-privacy)
- 📰 [ENISA — Pseudonymisation techniques](https://www.enisa.europa.eu/publications/pseudonymisation-techniques-and-best-practices)
- 🧪 [CNIL PIA software (free)](https://www.cnil.fr/en/open-source-pia-software-helps-carry-out-data-protection-impact-assesment)

---

## Module 5 — Glossary recap

Personal Data, PII, Sensitive / Special Category Data, PHI, PCI, SPDI, Data classification, Data lifecycle, Privacy by Design, Data minimisation, Purpose limitation, Storage limitation, Lawful basis, Consent (granular/unbundled/withdrawable), Legitimate Interest Assessment (LIA), DSR / DSAR, Right to erasure, Right to portability, DPIA / PIA, Data Flow Diagram, Adequacy decision, SCC, BCR, TIA, Schrems II, Data localisation, Anonymisation, Pseudonymisation, Tokenisation, k-anonymity, l-diversity, Differential privacy, Synthetic data, CMP, IAB TCF, Consent Mode.

→ Next: [Module 6 — Privacy Laws Deep Dive](06-privacy-laws.md)

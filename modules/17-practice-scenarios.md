# Module 17 — Practice Scenarios & Portfolio Capstones

> **Audience:** 🟢🟡🔴 — all · **Time:** 8–20 hours of practice · **Prereqs:** Any modules relevant to each scenario (noted per scenario)

## How to use this module

Each scenario is a **mini-capstone**: a realistic, ambiguous situation you'd face in a real job. For each, I give you:

- The setup.
- Stakeholders and constraints.
- Your deliverable(s).
- A rubric (how a senior would judge your output).
- Hints (what strong answers cover; what weak answers miss).

**Do not peek at the hints until you've attempted the scenario.** Use these as portfolio pieces.

---

## Scenario 1 — "Pre-audit evidence scramble" 🟢🟡

**Prereqs:** Modules 7, 9, 10.

**Setup.** You join a 90-person B2B SaaS as their first GRC hire. SOC 2 Type II fieldwork starts in 8 weeks. Your predecessor left. The shared drive is chaotic. The last internal audit was never done. The CTO says: "We're mostly ready."

**Stakeholders.** CEO (wants the report on time for deal X), CTO (believes it's fine), auditor (independent), 8 control owners in eng/HR/IT, a customer waiting on the report.

**Constraints.** Eng team bandwidth is limited (1 day/week security work). Budget 2L INR for tools.

**Deliverable.**

1. A **gap-assessment report** (4–6 pages) covering the 9 Common Criteria + the chosen TSCs.
2. A **remediation roadmap** week-by-week for 8 weeks.
3. An **evidence binder index** (control → evidence type → source → owner → frequency).
4. An email to the CTO when you find three controls that are *not* actually operating.

**Rubric.**

- Did you propose realistic prioritisation, or try to fix everything?
- Did you distinguish *design* gaps from *operating* gaps?
- Did you account for the auditor's sample period (Type II requires history)?
- Did your email to the CTO stay professional and evidence-led?

**Hints.** Strong answers surface that 8 weeks is too short to *create* Type II evidence that doesn't exist — recommend splitting into Type I now + Type II later, or delaying fieldwork. Weak answers promise miracles.

---

## Scenario 2 — "GDPR deletion request for an abandoned account" 🟢🟡

**Prereqs:** Modules 5, 6.

**Setup.** User emails on 14 Jan 2026: "Under GDPR Article 17, delete everything you have on me." They signed up in 2019, never paid, account dormant since 2020. Your company is India-based SaaS with EU customers.

**Complications.** Data is in prod DB, backups (7-year retention for finance), a data warehouse, a churned-vendor support ticket system you migrated away from, and marketing emails the user clicked in 2019.

**Deliverable.**

1. A **DSR response workflow** (diagram + narrative) specific to this request.
2. A **timeline** with GDPR clock (30 days + 60-day extension rules).
3. A **response letter** to the user explaining what will and won't be deleted, and why.
4. A **ticket to engineering** specifying what to delete and how to log proof.

**Rubric.**

- Did you identify all storage locations (not just prod)?
- Did you correctly apply the legal-obligation exemption for tax records?
- Did you handle backups (typical industry practice: flag & suppress on restore; not delete from cold backups)?
- Did you verify the requester's identity before acting?
- Did you address the ex-vendor's data and document a contractual gap?

**Hints.** Mention Art. 17(3)(e) (establishment/defence of legal claims) and Art. 12(6) (identity verification). Note the "right to be informed of recipients" (Art. 19).

---

## Scenario 3 — "BYOK rollout" 🔴🟡

**Prereqs:** Modules 2, 13.

**Setup.** Three enterprise prospects demand BYOK before signing. Your app runs on AWS, uses S3, RDS, and DynamoDB. You have 3 months.

**Deliverable.**

1. A **design doc** (2 pages) for the BYOK architecture: per-tenant KMS keys, tenant key management, app integration, what happens if customer rotates/deletes the key.
2. A **threat model** (STRIDE) of the BYOK design.
3. A **customer-facing BYOK FAQ** (1 page).
4. A **rollback plan** if a customer's key becomes unavailable.

**Rubric.**

- Did you address crypto-shredding risk (customer deletes key → data unrecoverable)?
- Did you propose key grants/aliases rather than key-sharing?
- Did you consider performance (KMS API quotas) and cost?
- Did you handle multi-region / DR?
- Did you clarify that BYOK is not HYOK (key still inside AWS)?

**Hints.** Discuss AWS KMS "key policies" + "grants," envelope encryption (DEK/KEK), S3 SSE-KMS per-tenant, CloudTrail visibility, and tenant isolation model. Explicitly address: what does "deletion" mean contractually?

---

## Scenario 4 — "Vendor DDQ nightmare" 🟢🟡

**Prereqs:** Modules 10, 12.

**Setup.** An enterprise prospect sends a 380-question DDQ (modified SIG). Sales wants it back in 48 hours. Previous answers live in 4 Google Docs in varying states of accuracy.

**Deliverable.**

1. A **response strategy memo** (1 page): what to answer, what to punt, what to refuse.
2. **Sample answers** to 15 representative questions across domains (identity, encryption, IR, privacy, backup, BCP, subcontractors).
3. A **"single source of truth" proposal** (Trust Center + question library + RACI) for future DDQs.

**Rubric.**

- Did you mark questions you *shouldn't* answer (overly intrusive, NDA territory)?
- Did you reference your SOC 2 / ISO 27001 report to collapse 50+ questions?
- Did you flag answers that need engineering sign-off before release?
- Did your Trust Center proposal show an ROI (hours saved per deal)?

**Hints.** A strong response library reduces per-DDQ time from 2 weeks to 2 days. Mention HECVAT / CAIQ / SIG and vendor platforms (Whistic, SafeBase, TrustCloud, Vanta Trust Center).

---

## Scenario 5 — "Second-party customer audit next Tuesday" 🟢🟡

**Prereqs:** Modules 9, 10, 12.

**Setup.** A Fortune-500 customer invokes their right-to-audit clause. They're sending two security auditors on-site next Tuesday for 2 days. Scope: your entire ISMS.

**Deliverable.**

1. A **kickoff deck** (8–10 slides) you'll present to them Tuesday 9 am.
2. A **document pack** (list of 15–20 docs you'd share in advance under NDA).
3. An **internal prep checklist** (what to brief each team member on).
4. A **"do not say" list** — common mistakes people make in front of auditors.

**Rubric.**

- Did you establish scope in writing before they arrive?
- Did you have a single "tour guide" / liaison, not 12 people answering ad-hoc?
- Did you prepare the office (no sensitive whiteboards, no dev-mode creds on screens)?
- Did your "do not say" list include speculation, admitting things you're not sure about, or volunteering scope expansions?

**Hints.** Treat like a planned fire drill. Rehearse walkthroughs. Have the control-owner for each domain prepped with their 3-minute narrative. Keep a running **findings log** during the visit so nothing leaves unclarified.

---

## Scenario 6 — "DPDP SDF obligations kicking in" 🟢🟡

**Prereqs:** Module 6.

**Setup.** Your fintech app has 60 M users in India. The government notifies you as a **Significant Data Fiduciary** under DPDP Act 2023. The notification gives 6 months.

**Deliverable.**

1. A **DPDP compliance gap assessment** (5 pages) covering: consent, notice, DPO appointment, DPIA, algorithmic audits, data-localisation implications, cross-border transfer stance, breach-reporting process.
2. A **roadmap** with month-by-month milestones.
3. A **board memo** requesting budget and headcount.

**Rubric.**

- Did you cover SDF-specific duties (appointed DPO in India, periodic DPIA, algorithmic audit)?
- Did you align with CERT-In 6-hour reporting as the operational reality?
- Did you address children's data (verifiable parental consent)?
- Did you propose a Consent Manager integration pathway?

**Hints.** DPDP borrows concepts from GDPR but has distinct duties: DPBI (Data Protection Board of India), no explicit "legitimate interest" basis, cross-border transfers by "negative list" rules, penalties up to ₹250 Cr.

---

## Scenario 7 — "AI feature launch gate" 🔴🟡

**Prereqs:** Modules 5, 6, 15.

**Setup.** Product wants to launch an AI feature that summarises customer support tickets using a third-party LLM API. Tickets contain PII and occasionally SPDI (health info in refund disputes).

**Deliverable.**

1. A **review memo** with a go / no-go / conditions.
2. A **DPIA** (short form).
3. A **threat model** (OWASP LLM Top 10 + MITRE ATLAS relevant techniques).
4. A **contract checklist** for the LLM vendor (DPA, training opt-out, retention, sub-processors).
5. A **monitoring plan** (prompt logging, hallucination rate, harmful output rate, drift).

**Rubric.**

- Did you address the lawful basis for sending existing-ticket personal data to a new purpose?
- Did you require a training opt-out and zero-retention mode?
- Did you propose redaction / anonymisation before sending to the LLM?
- Did you cover prompt injection via ticket content?
- Did you align with EU AI Act or NIST AI RMF categorisation?

**Hints.** Strong memos recommend **redact → summarise → review** with human-in-the-loop for SPDI. Conditional launch behind a feature flag with canary tenants is acceptable to a realistic senior.

---

## Scenario 8 — "3 AM breach: credential-stuffing + data exposure" 🔴🟡

**Prereqs:** Modules 3, 9, 11.

**Setup.** At 02:47 IST, anomaly alert: 14,000 logins/minute from residential IPs worldwide, ~3% success rate. Your SIEM shows successful logins accessing invoice-download endpoints (contain customer PII + bank partial account numbers — SPDI under DPDP).

**Deliverable.**

1. A **minute-by-minute IR log** for the first 6 hours.
2. A **regulatory-notification plan** (CERT-In 6-hour, DPDP DPBI, affected customer bank regulators, GDPR for any EU users among affected).
3. A **containment plan** without breaking legitimate users.
4. A **customer communication** (email + status page).
5. A **postmortem** (lessons learned, 5-whys, action items).

**Rubric.**

- Did you hit CERT-In's 6-hour clock (practically: preserve evidence, fill Annexure I, email to `incident@cert-in.org.in`)?
- Did you avoid making public statements before legal review?
- Did you force password resets **only for confirmed-affected accounts**, not all users (unless risk warrants)?
- Did you implement passive controls first (rate-limiting, bot detection, CAPTCHA on suspicious patterns) before aggressive ones (lockouts)?
- Did your postmortem attribute to systemic causes (no bot protection, no MFA) rather than individual blame?

**Hints.** A senior will check if you separated "what we know" from "what we assume." The difference between these two sentences to customers is career-defining: "Your data was accessed" (fact) vs "We detected unusual activity on some accounts and are investigating" (honest until proven).

---

## Scenario 9 — "Vendor concentration risk realised" 🟢🟡🔴

**Prereqs:** Module 12.

**Setup.** Your email-delivery vendor (handles 100% of transactional emails) has a 48-hour outage. OTP emails aren't delivering; customers can't sign up, reset passwords, or receive invoices. Board asks: "How did we let this happen?"

**Deliverable.**

1. A **root-cause analysis** (technical + procurement + governance).
2. A **resiliency plan** (failover vendor, DNS-based switching, degraded-mode flows).
3. A **revised vendor policy** (single-vendor-risk thresholds, dual-run requirements).
4. A **board update** (1 page, no jargon).

**Rubric.**

- Did you identify the governance failure (no tier-1 critical vendor should be single-source)?
- Did you propose pragmatic mitigations rather than "switch vendors tomorrow"?
- Did you cover secondary impacts (OTP → account lockouts → helpdesk overload)?
- Did the board update focus on action, not blame?

---

## Scenario 10 — "Pen-test report lands with 4 Criticals" 🔴🟡

**Prereqs:** Modules 11, 14.

**Setup.** External pen-test report drops. 4 Critical, 11 High, 22 Medium. CTO wants all Criticals fixed in 30 days; product roadmap has zero slack; customer auditor is asking for the report in 45 days.

**Deliverable.**

1. A **triage summary** (2 pages, exec-ready).
2. A **remediation plan** with owners and dates.
3. A **compensating-controls memo** for any finding that won't be fixed in 30 days.
4. A **revised SDLC proposal** to prevent recurrence.
5. A **disclosure stance** for the customer-facing report.

**Rubric.**

- Did you validate findings before sprinting (false positives happen)?
- Did you separate "fix in code" from "fix in config" from "fix by control" (e.g., WAF rule as temporary)?
- Did you propose retest scope and schedule?
- Did you avoid leaking raw pen-test findings to customers — instead share a scoped summary or attestation letter?

---

## How to submit / use these as portfolio

For each scenario you complete:

1. Put the deliverables in a private GitHub repo (or public if sanitised).
2. Write a 2-page **README** framing the problem, your approach, and what you'd do differently with more time.
3. Link from LinkedIn under "Featured." Call it a *capstone*, not a "project."
4. Bring 2 scenarios ready to discuss in interviews — not recite, discuss.

Three completed capstones from this module will out-compete five certs on a resume.

---

## Mini-bank of 30-second drills (use on commute)

- Explain *exception* vs *deviation* vs *non-conformity* in 60 seconds.
- Map RBAC to ABAC to ReBAC with one use case each.
- Walk through a TLS 1.3 handshake in plain English.
- Explain DPDP to an American friend — 2 minutes.
- Defend why ISO 27001 is worth the cost to a sceptical founder.
- Contrast SOC 2 Type I vs Type II — and when each is enough.
- Describe what a BIA produces and why it matters.
- Name the 6 NIST CSF 2.0 functions in order.
- What's in a SoA and why do auditors love it?
- What's the difference between encryption at-rest and tokenisation?

→ Next: [Appendix A — Free Resources Master List](../reference/free-resources.md)

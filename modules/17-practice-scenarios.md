# Module 17: Practice Scenarios & Portfolio Capstones

> **Audience:** 🟢🟡🔴: all · **Time:** 8–20 hours of practice · **Prereqs:** Any modules relevant to each scenario (noted per scenario)

## How to use this module

Each scenario is a mini-capstone. A realistic, ambiguous situation you would face in a real job. For each, you get:

- The setup.
- Stakeholders and constraints.
- Your deliverables.
- A rubric, written the way I would actually judge the output if it landed on my desk.
- Hints. What strong answers cover. What weak answers miss.

Do not peek at the hints until you have attempted the scenario. Use the better ones as portfolio pieces.

A note on STAR and C4R: when you answer these in interviews, use STAR for behavioural ("tell me about a time") and C4R for technical ("how would you handle"). I have included framework hooks in each rubric so you can practise the structure as well as the content.

---

## Scenario 1: "Pre-audit evidence scramble" 🟢🟡

**Prereqs:** Modules 7, 9, 10.

**Setup.** You join a 90-person B2B SaaS as their first GRC hire. SOC 2 Type II fieldwork starts in 8 weeks. Your predecessor left. The shared drive is chaotic. The last internal audit was never done. The CTO says, "We are mostly ready."

**Stakeholders.** CEO (wants the report on time for deal X), CTO (believes it is fine), auditor (independent), 8 control owners across engineering, HR, and IT, a customer waiting on the report.

**Constraints.** Engineering bandwidth is one day a week for security work. Budget of 2L INR for tools.

**Deliverable.**

1. A gap-assessment report (4 to 6 pages) covering the 9 Common Criteria and the chosen TSCs.
2. A remediation roadmap, week by week, for 8 weeks.
3. An evidence binder index (control → evidence type → source → owner → frequency).
4. An email to the CTO when you find three controls that are not actually operating.

**Rubric (C4R-shaped).**

- Did you clarify the scope and the date the audit covers, or did you assume?
- Did you separate design gaps from operating gaps? These cost very different things to fix.
- Did you account for the auditor's sample period? Type II needs history. You cannot generate it retrospectively.
- Did your email to the CTO stay professional and evidence-led? No blame language.
- Did you propose a realistic option set (delay, split into Type I now plus Type II later, scope-reduce)?

**Hints.** Strong answers say plainly that 8 weeks is not enough to create Type II evidence that does not already exist. Recommend splitting into Type I now and Type II later, or moving fieldwork. Weak answers promise miracles. The CEO will be unhappy in the short term and grateful in six months.

---

## Scenario 2: "GDPR deletion request for an abandoned account" 🟢🟡

**Prereqs:** Modules 5, 6.

**Setup.** A user emails on 14 Jan 2026: "Under GDPR Article 17, delete everything you have on me." They signed up in 2019, never paid, account dormant since 2020. Your company is India-based SaaS with EU customers.

**Complications.** Data lives in the prod DB, backups (7-year retention for finance), a data warehouse, a support ticket system from a vendor you migrated away from, and marketing emails the user clicked in 2019.

**Deliverable.**

1. A DSR response workflow (diagram and narrative) specific to this request.
2. A timeline aligned to the GDPR clock (30 days plus the 60-day extension rules).
3. A response letter to the user explaining what will and will not be deleted, and why.
4. A ticket to engineering specifying what to delete and how to log proof.

**Rubric.**

- Did you identify every storage location, not just prod?
- Did you correctly apply the legal-obligation exemption for tax records?
- Did you handle backups using normal industry practice (flag and suppress on restore, do not delete from cold backups)?
- Did you verify the requester's identity before acting?
- Did you flag the ex-vendor data and document a contractual gap to fix going forward?

**Hints.** Reference Art. 17(3)(e) (establishment or defence of legal claims) and Art. 12(6) (identity verification). Note the "right to be informed of recipients" under Art. 19. Weak answers say "yes, deleted everything" without acknowledging the exemptions or the data warehouse.

---

## Scenario 3: "BYOK rollout" 🔴🟡

**Prereqs:** Modules 2, 13.

**Setup.** Three enterprise prospects demand BYOK before signing. Your app runs on AWS and uses S3, RDS, and DynamoDB. You have 3 months.

**Deliverable.**

1. A design doc (2 pages) for the BYOK architecture: per-tenant KMS keys, tenant key management, app integration, what happens if the customer rotates or deletes the key.
2. A threat model (STRIDE) of the BYOK design.
3. A customer-facing BYOK FAQ (1 page).
4. A rollback plan for the case where a customer's key becomes unavailable.

**Rubric.**

- Did you address crypto-shredding risk (customer deletes key, data unrecoverable)?
- Did you propose key grants and aliases rather than key sharing?
- Did you consider performance (KMS API quotas) and cost?
- Did you handle multi-region and DR?
- Did you make clear that BYOK is not HYOK? The key still lives inside AWS.

**Hints.** Discuss AWS KMS key policies and grants, envelope encryption (DEK / KEK), S3 SSE-KMS per tenant, CloudTrail visibility, and the tenant isolation model. Explicitly address what "deletion" means contractually. The customer-facing FAQ should not promise anything you cannot operationally do at 3 AM on a Sunday.

---

## Scenario 4: "Vendor DDQ nightmare" 🟢🟡

**Prereqs:** Modules 10, 12.

**Setup.** An enterprise prospect sends a 380-question DDQ (modified SIG). Sales wants it back in 48 hours. Previous answers live in four Google Docs in varying states of accuracy.

**Deliverable.**

1. A response strategy memo (1 page): what to answer, what to punt, what to refuse.
2. Sample answers to 15 representative questions across domains (identity, encryption, IR, privacy, backup, BCP, subcontractors).
3. A single-source-of-truth proposal (Trust Center plus question library plus RACI) for future DDQs.

**Rubric.**

- Did you mark questions you should not answer (overly intrusive, NDA territory)?
- Did you reference your SOC 2 or ISO 27001 to collapse 50 or more questions?
- Did you flag answers that need engineering sign-off before release?
- Did your Trust Center proposal show ROI (hours saved per deal)?

**Hints.** A real response library cuts per-DDQ time from two weeks to two days. Mention HECVAT, CAIQ, SIG, and platforms like Whistic, SafeBase, TrustCloud, or Vanta Trust Center. Weak answers try to answer all 380 questions in 48 hours. Strong answers triage and push for a Trust Center.

---

## Scenario 5: "Second-party customer audit next Tuesday" 🟢🟡

**Prereqs:** Modules 9, 10, 12.

**Setup.** A Fortune-500 customer invokes their right-to-audit clause. Two security auditors arrive on-site Tuesday for two days. Scope: your entire ISMS.

**Deliverable.**

1. A kickoff deck (8 to 10 slides) for Tuesday 9 am.
2. A document pack (15 to 20 documents to share in advance under NDA).
3. An internal prep checklist (what to brief each team member on).
4. A "do not say" list. Common mistakes people make in front of auditors.

**Rubric.**

- Did you establish scope in writing before they arrive?
- Did you have a single tour guide and liaison, not 12 people answering ad-hoc?
- Did you prepare the office? No sensitive whiteboards. No dev-mode credentials on screens.
- Did your "do not say" list include speculation, admitting things you are not sure about, or volunteering scope expansions?

**Hints.** Treat it like a planned fire drill. Rehearse walkthroughs. Each control owner should have a three-minute narrative ready. Keep a running findings log during the visit so nothing leaves the room unclarified. The thing nobody tells you: auditors notice the body language as much as the documents.

---

## Scenario 6: "DPDP SDF obligations kicking in" 🟢🟡

**Prereqs:** Module 6.

**Setup.** Your fintech app has 60M users in India. The government notifies you as a Significant Data Fiduciary under the DPDP Act 2023. The notification gives 6 months.

**Deliverable.**

1. A DPDP compliance gap assessment (5 pages) covering consent, notice, DPO appointment, DPIA, algorithmic audits, data-localisation implications, cross-border transfer stance, breach reporting.
2. A roadmap with month-by-month milestones.
3. A board memo requesting budget and headcount.

**Rubric.**

- Did you cover SDF-specific duties (DPO appointed in India, periodic DPIA, algorithmic audit)?
- Did you align with CERT-In 6-hour reporting as the operational reality, separate from DPDP timelines?
- Did you address children's data (verifiable parental consent)?
- Did you propose a Consent Manager integration pathway?

**Hints.** DPDP borrows concepts from GDPR but has its own duties: DPBI (Data Protection Board of India), no explicit legitimate-interest basis, cross-border transfers governed by a negative list, penalties up to ₹250 Cr. Weak board memos talk about features. Strong ones talk about risk reduced and money saved.

---

## Scenario 7: "AI feature launch gate" 🔴🟡

**Prereqs:** Modules 5, 6, 15.

**Setup.** Product wants to launch an AI feature that summarises customer support tickets using a third-party LLM API. Tickets contain PII and occasionally SPDI (health info in refund disputes).

**Deliverable.**

1. A review memo with go, no-go, or conditions.
2. A short-form DPIA.
3. A threat model using OWASP LLM Top 10 2025 and the relevant MITRE ATLAS techniques.
4. A contract checklist for the LLM vendor (DPA, training opt-out, retention, sub-processors).
5. A monitoring plan (prompt logging, hallucination rate, harmful output rate, drift).

**Rubric.**

- Did you address the lawful basis for sending existing-ticket personal data to a new purpose?
- Did you require a training opt-out and a zero-retention mode?
- Did you propose redaction or anonymisation before sending content to the LLM?
- Did you cover prompt injection via ticket content (indirect prompt injection)?
- Did you align with EU AI Act or NIST AI RMF categorisation?

**Hints.** Strong memos recommend redact, summarise, review with a human in the loop for SPDI. A conditional launch behind a feature flag with canary tenants is acceptable to a realistic senior. "Block forever" without conditions is rarely the right answer; nor is "launch tomorrow."

---

## Scenario 8: "3 AM breach: credential stuffing plus data exposure" 🔴🟡

**Prereqs:** Modules 3, 9, 11.

**Setup.** At 02:47 IST, an anomaly alert: 14,000 logins per minute from residential IPs worldwide with about a 3% success rate. The SIEM shows successful logins accessing invoice-download endpoints. Invoices contain customer PII and partial bank account numbers (SPDI under DPDP).

**Deliverable.**

1. A minute-by-minute IR log for the first 6 hours.
2. A regulatory notification plan (CERT-In 6-hour, DPDP DPBI, affected customers' bank regulators, GDPR for any EU users).
3. A containment plan that does not break legitimate users.
4. Customer communication (email and status page).
5. A postmortem with lessons learned, 5-whys, and action items.

**Rubric.**

- Did you hit the CERT-In 6-hour clock? Practically: preserve evidence, fill Annexure I, email `incident@cert-in.org.in`.
- Did you avoid making public statements before legal review?
- Did you force password resets for confirmed-affected accounts only, not all users, unless risk warrants the wider sweep?
- Did you implement passive controls first (rate limiting, bot detection, CAPTCHA on suspicious patterns) before aggressive ones (lockouts)?
- Did your postmortem attribute the failure to systemic causes (no bot protection, no MFA), not individual blame?

**Hints.** A senior will check whether you separated "what we know" from "what we assume." The difference between these two sentences is career defining: "your data was accessed" (a fact, often premature) versus "we detected unusual activity on some accounts and are investigating" (honest until proven). Choose your words carefully.

---

## Scenario 9: "Vendor concentration risk realised" 🟢🟡🔴

**Prereqs:** Module 12.

**Setup.** Your email-delivery vendor handles 100% of transactional emails. It has a 48-hour outage. OTP emails are not delivering. Customers cannot sign up, reset passwords, or receive invoices. The board asks: "How did we let this happen?"

**Deliverable.**

1. A root cause analysis (technical, procurement, governance).
2. A resiliency plan (failover vendor, DNS-based switching, degraded-mode flows).
3. A revised vendor policy (single-vendor-risk thresholds, dual-run requirements).
4. A board update (1 page, no jargon).

**Rubric.**

- Did you identify the governance failure? No Tier 1 critical vendor should be single source.
- Did you propose pragmatic mitigations rather than "switch vendors tomorrow"?
- Did you cover secondary impacts (OTP failures lead to account lockouts lead to helpdesk overload)?
- Did the board update focus on action, not blame?

---

## Scenario 10: "Pen-test report lands with 4 Criticals" 🔴🟡

**Prereqs:** Modules 11, 14.

**Setup.** External pen-test report drops. Four Critical, eleven High, twenty-two Medium. The CTO wants all Criticals fixed in 30 days. Product roadmap has zero slack. A customer auditor is asking for the report in 45 days.

**Deliverable.**

1. A triage summary (2 pages, exec-ready).
2. A remediation plan with owners and dates.
3. A compensating controls memo for any finding that will not be fixed in 30 days.
4. A revised SDLC proposal to prevent recurrence.
5. A disclosure stance for the customer-facing report.

**Rubric.**

- Did you validate findings before sprinting? False positives happen, even at the Critical level.
- Did you separate "fix in code" from "fix in config" from "fix by control" (e.g., a WAF rule as a temporary mitigation)?
- Did you propose a retest scope and schedule?
- Did you avoid leaking raw pen-test findings to the customer? Share a scoped summary or attestation letter instead.

---

## How to submit and use these as portfolio

For each scenario you complete:

1. Put the deliverables in a private GitHub repo, or public if sanitised.
2. Write a two-page README framing the problem, your approach, and what you would do differently with more time.
3. Link it from LinkedIn under "Featured." Call it a capstone, not a "project."
4. Bring two scenarios ready to discuss in interviews. Not to recite. To discuss.

Three completed capstones from this module will out-compete five certifications on a resume at the entry level. I have made that call as an interviewer more than once.

---

## Mini-bank of 30-second drills (use on commute)

- Explain exception versus deviation versus non-conformity in 60 seconds.
- Map RBAC to ABAC to ReBAC with one use case each.
- Walk through a TLS 1.3 handshake in plain English.
- Explain DPDP to an American friend in 2 minutes.
- Defend why ISO 27001 is worth the cost to a sceptical founder.
- Contrast SOC 2 Type I versus Type II, and when each is enough.
- Describe what a BIA produces and why it matters.
- Name the six NIST CSF 2.0 functions in order.
- What is in an SoA and why do auditors love it?
- Difference between encryption at rest and tokenisation.

→ Next: [Appendix A: Free Resources Master List](../reference/free-resources.md)

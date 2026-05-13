# Module 12: Third-Party / Vendor Risk

> **Audience:** 🟡 🔴 · **Time:** ~60 min · **Prereqs:** Modules 5–7

## Why this matters

Your customers trust you. You trust your vendors. Your vendors trust their own vendors. That chain breaks more often than people admit, and most of the named breaches you can list off the top of your head started in someone else's environment. Every audit, every enterprise procurement team, every regulator asks the same question in different words: how do you manage third-party risk? This module is what I wish someone had handed me before I owned the vendor inbox.

A small warning up front. TPRM is the function that gets the least respect and the most blame. When a vendor goes down at 3 AM, the call comes to you. When sales wants the deal closed by Friday, the questionnaire lands on you. Build the programme on the assumption that you will be understaffed.

---

## 12.1 Types of third parties

- Software vendors (SaaS, on-prem).
- Cloud and infrastructure providers.
- Professional services (consultants, auditors, lawyers).
- Outsourced operations (BPO, customer support, payroll).
- Data processors and sub-processors.
- Resellers and partners.
- Open-source dependencies (covered more in Module 14).

The fourth party is your vendor's vendor. The fifth party is beyond that. In my experience, fourth-party visibility is mostly theatre unless the vendor in question is Tier 1. Pick your battles. For a critical IdP or payments processor, yes, go two layers deep. For a Tier 3 tool, knowing the sub-processor list is enough.

---

## 12.2 The TPRM lifecycle

1. Onboarding (intake and tiering).
2. Due diligence.
3. Contracting.
4. Ongoing monitoring.
5. Offboarding.

The single biggest operational question is: who owns the inbox? If intake lives in a Slack channel, you have no programme. If it lives in a ticket queue with an SLA, you have one. Push hard for an intake form wired into procurement so a PO cannot be raised without a security ticket number.

### Tiering, because not every vendor deserves the same scrutiny

Assess by:
- Data access (none / internal only / personal / sensitive / regulated).
- System access (none / limited / privileged).
- Business criticality (non-critical / important / critical).
- Spend and volume.

Typical tiers:

| Tier | Example | Depth of DD |
|------|---------|-------------|
| Tier 1 (critical) | IdP, cloud provider, core DB, payroll processor | Full SIG, SOC 2/ISO, DPIA, deep review, executive approval, annual review |
| Tier 2 (important) | CRM, analytics, email provider | SIG-Lite, SOC 2, DPA, annual review |
| Tier 3 (low) | Stationery supplier, office coffee | Minimal questions, contract on file |

A common failure pattern: a Tier 3 tool is approved, then a year later product starts piping customer data into it without telling anyone. Re-tier on use, not just on procurement. Build a six-monthly check where finance pulls SaaS spend and you reconcile it against the inventory. Shadow IT is almost always discovered through invoices, not through scanning.

---

## 12.3 Due diligence: the DDQ universe

### Common questionnaires

- **SIG (Shared Assessments)**: full (1000+ questions) and **SIG-Lite** (~300). Industry standard.
- **CAIQ (Cloud Security Alliance)**: cloud-provider focused.
- **HECVAT**: education sector.
- **VSAQ (Google)**: simpler web-app focus.
- Proprietary: many large enterprises use their own, usually a Frankenstein of SIG plus pet questions from a CISO ten years ago.

Questionnaire fatigue is real. The SIG was never meant to be filled out from scratch for every deal. If you are the vendor, build a question bank once and reuse. If you are the assessor, do not send a 1,000-question SIG to a five-person startup; you will get nonsense back.

### What to collect

- Completed DDQ.
- SOC 2 Type II report (latest).
- ISO 27001 certificate and SoA.
- Pen-test summary. Most vendors will not share the full report and you should not expect them to.
- Evidence of relevant certifications (HIPAA BAA, PCI AoC, HDS, FedRAMP).
- DPA with processor's sub-processor list.
- Privacy notice, retention policy.
- BCP/DR summary, last test date.
- Financial health (for critical vendors).
- Insurance (cyber liability, E&O).
- OFAC / sanctions check in regulated industries.

### Reviewing a SOC 2: the bits that actually matter

- Read the auditor's opinion first. A qualified opinion is a red flag. So is an auditor nobody has heard of.
- Check the period covered. A report from 14 months ago is stale. Ask for a bridge letter.
- Check subservice handling. Inclusive means the sub-service org is in scope. Carve-out means it is not, and you have to assess it yourself. Most reports are carve-out. Plan accordingly.
- Check exceptions in Section 4. One or two is normal. Three exceptions all in the access management area is a problem.
- Check **CUECs (Complementary User Entity Controls)**. These are things you must do for the vendor's controls to work. Most teams ignore them. They are inherited obligations and auditors do ask.
- Scope mismatch. The product you are buying may not be in scope. I have seen vendors send a SOC 2 covering their corporate IT and not the product. Read the system description.

---

## 12.4 Contracting: make it binding

The security review is worth little if the contract does not back it up. Get involved before legal sends the redline back.

- **Data Processing Agreement (DPA)**: required under GDPR Art. 28.
- **Business Associate Agreement (BAA)**: required under HIPAA when PHI is involved.
- **Standard Contractual Clauses (SCCs)**: for cross-border transfers.
- Sub-processor list with notification rights and the right to object.
- Security addendum with specific controls (encryption, MFA, logging, response times).
- Breach notification SLA. Usually 24 to 72 hours to you. Push for the lower end.
- Right to audit, annually and after incidents.
- Termination and data return or destruction, with a certificate.
- Liability and indemnity. Cyber carve-outs are where the real fight is.
- Insurance minimums.
- Ethics, sanctions, and modern-slavery clauses in regulated industries.

A practical tip: get a security addendum template approved by legal once, and attach it to every contract. You will save weeks per deal.

---

## 12.5 Ongoing monitoring

Vendors do not stay safe forever. The signature on the DDQ a year ago is not a control.

- Continuous monitoring ratings: SecurityScorecard, Bitsight, UpGuard produce external risk scores. Treat them as a signal, not a verdict. The score can be gamed and often is. Use them to trigger conversations, not decisions.
- SOC 2 and ISO annual refresh. Chase reports before they expire. Build a calendar.
- Breach alerts. When a vendor is in the news, do not wait for the email; reach out.
- Quarterly or annual review depending on tier.
- Event triggers: M&A, leadership change, significant incident, new sub-processor.
- Sub-processor change notices. Evaluate before they go live. Most teams glance and approve. Read at least one a quarter properly.

### Concentration risk and the fourth-party question

This is where most programmes are weakest. You can have ten vendors and still have one cloud region underneath all of them. When that region has a bad day, you find out the hard way.

What I track:

- Percentage of critical workloads on a single cloud provider, and on a single region.
- How many of your critical SaaS vendors also run on that same region.
- Authentication chains. If your IdP, your SSO provider, and three downstream vendors all share an upstream, that is one breach away from a very bad Tuesday.

Put this in the risk register with a real owner. Saying "we have multi-cloud DR" without testing it is a comfort blanket, not a control.

---

## 12.6 Offboarding

Often forgotten, often audited. This is the cheapest finding to get hit with.

- Revoke access: SSO integrations, API keys, VPN, SaaS admin panels.
- Data return or destruction, with a certificate of destruction.
- Close the account in the vendor's platform. Do not just stop paying; dormant accounts get inherited by the next owner.
- Update the sub-processor list and tell customers if it is public.
- Keep evidence of offboarding per retention policy.

Zombie access is the single most common finding in vendor offboarding. Run a quarterly reconciliation of SaaS spend against the active vendor inventory; anything paid for but not on the list, investigate.

---

## 12.7 Building a Trust Center (the other side of the desk)

You are not just assessing vendors; customers assess you. Make it easy or pay for it in sales-engineering time.

- Public Trust Center with the summary, certifications, SOC 3, pen-test attestation letter, sub-processor list, status page, DPA template, security whitepaper. Not a marketing page.
- NDA-gated room for SOC 2 Type II, ISO SoA, detailed pen-test, full DDQ responses.
- Pre-answered library: SIG-Lite, CAIQ, HECVAT already filled in. Send in minutes, not weeks.
- Sub-processor notification: an email list or RSS feed and a published policy, for example 30 days' notice before a new sub-processor goes live.

Tools: SafeBase, Vanta Trust Center, Drata Trust Center, Whistic, Conveyor. They mostly do the same thing. Pick the one whose pricing your finance team can stomach.

A pre-answered library can take a deal from "two-week security review" to "we approved them last Thursday." It pays for itself in two enterprise deals.

---

## 12.8 Worked example: evaluating a new analytics vendor

Proposed: a product-analytics SaaS that ingests event data, including user IDs and IPs.

Steps I would actually take:

1. Intake ticket from product, with the use case and data fields.
2. Data assessment. User IDs and IPs are personal data. Tier 2.
3. Send SIG-Lite, request SOC 2 Type II and the DPA template.
4. Check hosting region. US-only means SCCs and a TIA for EU users.
5. Review sub-processor list. Anything in there you cannot accept (a competitor, a banned country)?
6. DPIA with product. What is collected, why, retention, user rights.
7. Security review: IAM, encryption, SSO support, export and deletion APIs.
8. Contract: 72-hour breach notice, audit rights, data destruction on termination.
9. Decision with documented residual risks.
10. Register in vendor inventory with a next-review date.
11. Train product on safe configuration. Data minimisation in event payloads is the single biggest lever; engineers tend to dump entire objects into events.

---

## 12.9 Common beginner mistakes

- Treating the DDQ as the assessment. The DDQ is an input. Read it, then verify against evidence.
- No tiering, so you over-audit the coffee supplier and under-audit the IdP.
- Ignoring sub-processor chains.
- No offboarding discipline. Zombie access persists for years.
- Procurement going around security. Fix it with a mandatory intake gate in the purchase-request flow, not by complaining in retros.
- Trusting the score from a rating platform without reading what is behind it.
- Approving a vendor based on a SOC 2 that does not cover the product you are buying.

---

## 12.10 Go deeper

- 🏛 [Shared Assessments (SIG)](https://sharedassessments.org/sig/)
- 🏛 [CSA CAIQ v4](https://cloudsecurityalliance.org/research/cloud-controls-matrix)
- 🏛 [AICPA SOC report user guidance (free)](https://www.aicpa-cima.com/resources/download/soc-for-service-organizations-trust-services-criteria)
- 📰 [Vanta / Drata / Secureframe TPRM guides (free)](https://www.vanta.com/resources)
- 📰 [Whistic / Conveyor content libraries](https://www.whistic.com/resources/)
- 🏛 [HECVAT (higher ed)](https://www.ren-isac.net/hecvat/)
- 🏛 [NIST SP 800-161: Supply Chain Risk Management](https://csrc.nist.gov/pubs/sp/800/161/r1/final)

## Module 12: Glossary recap

Third party, Fourth party, Sub-processor, TPRM lifecycle, Tiering, SIG, SIG-Lite, CAIQ, HECVAT, VSAQ, DPA, BAA, SCC, CUEC, Subservice org (inclusive vs carve-out), Qualified opinion, Sub-processor notice, Right to audit, Certificate of destruction, Continuous monitoring rating, Concentration risk, Trust Center, NDA-gated room, Pre-answered DDQ library.

→ Next: [Module 13: Cloud Security Basics](13-cloud-security.md)

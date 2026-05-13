# Module 9: Audit Lifecycle End-to-End

> **Audience:** 🟡 🔴 · **Time:** ~90 min · **Prereqs:** Modules 7–8

## Why this matters

Most GRC careers orbit audits. Customers audit you, regulators audit you, your internal audit team audits you, you audit your vendors. Knowing how audits actually work, what counts as evidence, how findings get written, and how non-conformities close, is what separates competent GRC analysts from everyone else.

Two strong opinions before we start.

First: **evidence freshness beats evidence volume**. A 400-screenshot folder where half is from last year fails audits faster than 40 clean, dated, system-generated artefacts. Build pipelines that produce evidence as a by-product of running the control, not a quarterly scramble.

Second: **auditor management is half the job**. Auditors are people, working under their own deadlines, with their own opinions. Treating them as adversaries makes audits worse. Treating them as collaborators (without becoming a pushover) makes them faster and produces better outcomes. More on this below.

---

## 9.1 Types of audits

| Type | Who runs it | Example |
|------|-------------|---------|
| **First-party (internal)** | Your own staff | Internal ISO 27001 audit |
| **Second-party** | A customer or partner | A bank customer audits you onsite |
| **Third-party (independent)** | Accredited firm or CPA | ISO certification body; SOC 2 CPA |
| **Regulator-driven** | A government body | RBI inspection, ICO investigation |
| **Attestation vs Certification vs Examination** | Varies | SOC 2 is an attestation; ISO 27001 is a certification |

### Attestation vs certification vs report

- **Attestation**: a licensed party expresses an opinion on management's assertions (SOC 1/2/3).
- **Certification**: an accredited body certifies conformance to a standard (ISO 27001, ISO 9001).
- **Examination / Audit report**: formal report with findings.

Customers and procurement teams routinely confuse these. Be ready to explain the difference in one sentence.

---

## 9.2 The audit lifecycle

1. **Planning**
   - Scope (systems, locations, time period).
   - Applicable criteria (SOC 2 TSC, ISO clauses plus Annex A, specific regulations).
   - Risk-based sampling strategy.
   - Timeline and logistics.
2. **Fieldwork**
   - Evidence collection.
   - Walkthroughs. SME explains a process, auditor observes.
   - Sample testing. Pick 25 access-grants from the year, verify approvals.
   - Tests of design plus tests of operating effectiveness.
3. **Reporting**
   - Draft findings (observations, NCs, CAPA).
   - Management response.
   - Final report.
4. **Follow-up**
   - Track CAPA through closure.
   - Verify fix with evidence.
   - Surveillance audits (ISO) years 1 and 2.
   - Recertification year 3.

### Readiness vs fieldwork vs observation period

- **Readiness assessment**: pre-audit dry run. Gaps identified before the auditor arrives. Worth it for first audits.
- **Observation period (SOC 2 Type II)**: 3 to 12 months during which controls must be operating. Evidence collected continuously, not at the end.
- **Fieldwork**: 2 to 6 weeks of intensive testing after the observation period.

The number-one mistake on first Type II audits: teams treat the observation period as the prep period. By the time fieldwork starts, half the months have no evidence and the auditor reports exceptions. Start the evidence pipeline before day one of the observation period.

---

## 9.3 Evidence: the currency of audits

Auditors accept four types of evidence (**OIIR**: Observe, Inquire, Inspect, Re-perform):

1. **Observation**: auditor watches a process (e.g., onboarding a new employee).
2. **Inquiry**: auditor interviews SMEs. Weakest alone. Always pair with another type.
3. **Inspection**: auditor examines artefacts (docs, tickets, logs, screenshots, reports).
4. **Re-performance**: auditor re-runs the control (e.g., re-runs a vuln scan, re-pulls a user list).

### What good evidence looks like

- **Dated and tied to the audit period.**
- **Showing who did what, when.** Screenshots of tickets and logs include usernames and timestamps.
- **Complete populations or statistically valid samples.** Not cherry-picked.
- **Repeatable.** The auditor could re-collect and get the same thing.
- **Independent.** System-generated beats hand-typed.
- **Accessible later.** Stored where future audits can find it.

The "dated" bit is what kills most evidence. A screenshot of an access list with no timestamp tells the auditor nothing useful. Either the system stamps it, or you stamp it (and the auditor will quietly downgrade that artefact).

### Evidence per common control

| Control | Typical evidence |
|---------|-----------------|
| MFA enforced on admin SSO | Screenshot of IdP policy plus user list with MFA status |
| Quarterly access review | Signed review report plus tickets for revocations |
| Encryption in transit | TLS scan report plus server config |
| Change management | Sample of 25 PRs showing review, test, approval, ticket linkage |
| Vulnerability management | Scan reports plus remediation tickets closed within SLA |
| Vendor due diligence | Completed DDQs, signed DPAs, vendor SOC 2 reports |
| Incident response | Tickets, tabletop records, post-mortems |
| Backup restoration | Test-restore log plus attestation memo |

---

## 9.4 Writing findings: NCs, OFIs, exceptions

Different frameworks use different words. Logic is the same.

### ISO 27001 findings

- **Major NC**: systemic failure, or a requirement not met. Certificate at risk. Must close before certificate issues.
- **Minor NC**: isolated lapse. CAPA required.
- **Observation / OFI**: not a failure. A recommendation.

### SOC 2 findings

Auditor reports **exceptions** in Section 4's Results of Tests. A single exception may or may not lead to a qualified opinion depending on materiality. Multiple exceptions in the same area usually imply a **deficiency** (potentially significant or material).

### What a well-written finding looks like

- **Condition**: what was found.
- **Criteria**: which requirement it violates.
- **Cause**: why it happened.
- **Consequence**: what harm could result.
- **Recommendation**: suggested remediation.

If the finding you receive is missing any of these, push back politely. Vague findings produce vague CAPAs that don't actually fix anything.

### CAPA: Corrective Action Plan

Every finding needs a CAPA with:
- Root cause analysis (5-whys is the cheap default).
- Correction (fix this instance).
- Corrective action (prevent recurrence: policy, training, tooling).
- Owner and due date.
- Evidence of closure.
- Auditor verification.

In my experience the corrective action gets skipped because the correction is faster. Don't. A correction with no preventive action means the same finding next year.

---

## 9.5 Preparing your organisation for audit

This is the actual GRC analyst job description.

### 90 days before

- Confirm scope with auditor.
- Update SoA (ISO) or System Description (SOC 2).
- Re-run a gap assessment against current controls.
- Freeze policy updates, or version them carefully.
- Communicate timeline to engineering and ops leaders.

### 60 days before

- Evidence collection automations in place (GRC tool plus direct pulls).
- Mock interviews with 5 to 10 SMEs who will face auditors.
- Remediate low-hanging gaps.

### 30 days before

- Clean-up sweep. Open tickets older than SLA, orphan accounts, expired certs, missing policy acknowledgements.
- Finalise evidence request list from auditor (the PBC, "Prepared by Client").

### During fieldwork

- Daily stand-up between audit manager and team.
- Single channel for auditor requests (Slack channel or email alias). Multiple channels guarantee dropped requests.
- SMEs coached: answer what's asked, show the evidence, admit unknowns, don't volunteer off-topic information. The "let me show you this other thing we built" is how scope expands mid-audit.
- Document every conversation. If the auditor verbally accepts something, write it down and circulate it.

### After fieldwork

- Draft findings: negotiate ambiguities, provide missing evidence, push back where a finding is mis-stated. Auditors get findings wrong sometimes. Polite, evidence-backed pushback works.
- Management response: concise, own the gap, present the CAPA.
- Communicate outcome to leadership and customers.

---

## 9.6 Sampling: how auditors actually test

Auditors sample. They cannot look at everything.

- **Population**: e.g., all change-tickets in the observation period.
- **Sample size**: varies by framework and control frequency. Typical SOC 2 rules of thumb:

| Control frequency | Sample size (typical) |
|-------------------|----------------------|
| Annual | 1 |
| Quarterly | 2 |
| Monthly | 2–5 |
| Weekly | 5–15 |
| Daily | 20–40 |
| Many-times-daily | 25+ statistical |

If the population is small and even one exception occurs, expect it to be reported. Control discipline matters more for low-frequency controls precisely because there's less room to hide.

---

## 9.7 Internal audit: the ISO 27001 Clause 9.2 requirement

Before your external audit, you must audit yourself. Internal audit should be:

- **Independent.** Auditors don't audit their own work.
- **Competent.** Trained internal auditors or external hire.
- **Risk-based.** Frequency tied to risk.
- **Documented.** Reports, findings, closure evidence.
- **Reported.** To top management in the management review meeting.

A real internal audit programme halves external-audit surprises. A box-ticking one doesn't. Auditors can tell the difference within an hour.

---

## 9.8 Management review (Clause 9.3)

A formal, documented meeting at least annually. Standard agenda:

- Status of previous actions.
- Changes in context (business, regulatory, tech).
- Performance of ISMS (incidents, NCs, KRIs, audit results).
- Feedback from interested parties.
- Risk treatment plan status.
- Opportunities for improvement.
- Decisions and actions (documented minutes).

Auditors read the minutes. Thin minutes equal thin assurance. "Discussed and noted" as the entire content of an item is a Minor NC waiting to happen.

---

## 9.9 Customer / second-party audits: the grind

Large customers (banks, insurers, telcos, governments) run their own audits on vendors. Typical ask:

- SIG or proprietary 300-question DDQ.
- On-site or virtual 2 to 5 day review.
- Specific evidence: pen-test reports, policy pack, ISMS manual, recent incident log, sub-processor DDQs, DR-test results.
- Right-to-audit clause invoked.

**Playbook:** build a trust centre (even a Notion page) that preempts 80% of questions. Tools: SafeBase, Vanta Trust, Drata Trust Center, Whistic. The first time you answer the same SIG question for the fifth time, you'll wish you'd built one sooner.

---

## 9.10 Common beginner mistakes

- Producing evidence only for the auditor. Living controls produce always-on evidence. Audits sample the stream.
- Taking findings personally. Findings are how organisations improve. Own them.
- Over-promising on CAPA ("we'll fix in 30 days"). Be realistic. Missed CAPA dates escalate to Major NCs in the next surveillance.
- Re-writing the system description post-hoc when reality changes. Change the description and the system together.
- Treating internal audit as a formality. Weak internal audit exposes you later.
- Treating the auditor as the enemy. The relationship is professional, not adversarial. Build credibility once and it pays off for years.

---

## 9.11 Interview traps

- **Q:** "Difference between a test of design and test of operating effectiveness?" Design = control is set up correctly. Effectiveness = it actually runs consistently over the period.
- **Q:** "What would you do if you discover an undocumented control exception mid-audit?" Disclose, document, propose remediation. Hiding it becomes a larger finding when discovered.
- **Q:** "Difference between an observation and a Minor NC in ISO 27001?" Observation is a recommendation, no failure. Minor NC = requirement not met, CAPA required.

## 9.12 Mini-exercise (30 min)

For the control *"Terminated employees lose access within 24 hours"*:
1. List 5 evidence artefacts an auditor would accept.
2. Describe how you'd sample a year's terminations.
3. Draft a finding (Condition/Criteria/Cause/Consequence/Recommendation) for a case where one ex-employee kept GitHub access for 10 days.

## 9.13 Go deeper

- 🏛 [ISO 19011: guidelines for auditing management systems](https://www.iso.org/standard/70017.html)
- 🏛 [IAF Mandatory Documents (free)](https://iaf.nu/en/iaf-documents/)
- 🏛 [AICPA Trust Services Criteria](https://www.aicpa-cima.com/resources/download/soc-for-service-organizations-trust-services-criteria)
- 📰 [AuditBoard resources](https://www.auditboard.com/resources/) · [Secureframe audit guides](https://secureframe.com/hub)
- 📘 [ISACA CISA exam topics: free outlines](https://www.isaca.org/credentialing/cisa)
- 📰 [SANS Audit whitepapers](https://www.sans.org/white-papers/?focus-area=audit-compliance)

## Module 9: Glossary recap

Internal / second-party / third-party audit, Attestation, Certification, Accredited body, Scope, Criteria, Observation period, Fieldwork, Readiness assessment, PBC list, Evidence types (Observation/Inquiry/Inspection/Re-performance), Walkthrough, Sampling, Test of design, Test of operating effectiveness, Major NC, Minor NC, OFI, Exception, Qualified opinion, Deficiency, CAPA, 5-Whys, Clause 9.2 Internal audit, Clause 9.3 Management review, Surveillance audit, Trust center.

→ Next: [Module 10: Policies, Standards & Procedures](10-policies.md)

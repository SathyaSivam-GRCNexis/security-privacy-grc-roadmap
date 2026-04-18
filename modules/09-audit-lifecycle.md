# Module 9 — Audit Lifecycle End-to-End

> **Audience:** 🟡 🔴 · **Time:** ~90 min · **Prereqs:** Modules 7–8

## Why this matters

Most GRC/compliance careers orbit audits. Customers audit you; regulators audit you; your internal audit team audits you; you audit your vendors. Knowing how audits *actually work* — what auditors ask, what counts as evidence, how findings get written, how non-conformities are closed — separates competent GRC analysts from everyone else. This is a big gap in courses; it's the #1 skill companies value.

---

## 9.1 Types of audits (know these cold)

| Type | Who runs it | Example |
|------|-------------|---------|
| **First-party (internal)** | Your own staff | Internal ISO 27001 audit |
| **Second-party** | A customer / partner | Big-bank customer audits you onsite |
| **Third-party (independent)** | An accredited firm or CPA | ISO certification body; SOC 2 CPA |
| **Regulator-driven** | A government body | RBI inspection; ICO investigation |
| **Attestation vs Certification vs Examination** | Varies | SOC 2 is an attestation; ISO 27001 is a certification |

### Attestation vs certification vs report

- **Attestation** — licensed party expresses an opinion on management's assertions (SOC 1/2/3).
- **Certification** — accredited body certifies conformance to a standard (ISO 27001, ISO 9001).
- **Examination/Audit report** — formal report with findings.

---

## 9.2 The audit lifecycle

1. **Planning**
   - Define scope (systems, locations, time period).
   - Identify applicable criteria (SOC 2 TSC, ISO clauses + Annex A, specific regulations).
   - Risk-based sampling strategy.
   - Agree timeline and logistics.
2. **Fieldwork**
   - Evidence collection.
   - Walkthroughs — SME explains a process, you observe.
   - Sample testing — e.g., pick 25 access-grants from the year, verify approvals.
   - Tests of design + tests of operating effectiveness.
3. **Reporting**
   - Draft findings (observations, NCs, CAPA).
   - Management response.
   - Final report.
4. **Follow-up**
   - Track CAPA through closure.
   - Verify fix (evidence-based).
   - Surveillance audits (ISO) — years 1 and 2.
   - Re-certification — year 3.

### Readiness vs fieldwork vs observation period

- **Readiness assessment** — pre-audit dry run; gaps identified before the auditor arrives.
- **Observation period (SOC 2 Type II)** — 3–12 months during which controls must be operating. Evidence collected continuously.
- **Fieldwork** — 2–6 weeks of intensive testing after the observation period.

---

## 9.3 Evidence — the currency of audits

Auditors accept four types of evidence (mnemonic: **OIIR** — Observe, Inquire, Inspect, Re-perform):

1. **Observation** — auditor watches a process (e.g., watching a new employee onboarding).
2. **Inquiry** — auditor interviews SMEs. Weakest alone; always pair with another type.
3. **Inspection** — auditor examines artefacts (docs, tickets, logs, screenshots, reports).
4. **Re-performance** — auditor re-runs the control (e.g., re-runs a vuln scan; re-pulls a user list).

### What "good" evidence looks like

- **Dated and tied to the audit period.**
- **Showing who did what, when** — screenshots of tickets/logs include usernames and timestamps.
- **Complete populations or statistically valid samples** — not cherry-picked.
- **Repeatable** — auditor could re-collect and get the same thing.
- **Independent** — system-generated beats hand-typed.
- **Accessible later** — stored where future audits can find it.

### Evidence per common control (examples)

| Control | Typical evidence |
|---------|-----------------|
| MFA enforced on admin SSO | Screenshot of IdP policy + user list with MFA status |
| Quarterly access review | Signed review report + tickets for revocations |
| Encryption in transit | TLS scan report + server config |
| Change management | Sample of 25 PRs showing review, test, approval, ticket linkage |
| Vulnerability management | Scan reports + remediation tickets closed within SLA |
| Vendor due diligence | Completed DDQs + signed DPAs + vendor SOC 2 reports |
| Incident response | Tickets, tabletop-exercise records, post-mortems |
| Backup restoration | Test-restore log + attestation memo |

---

## 9.4 Writing findings — NCs, OFIs, exceptions

Different frameworks use different words; the logic is the same.

### ISO 27001 findings

- **Major NC** — systemic failure, or a requirement not met at all. Certificate at risk. Must be closed before certificate issues.
- **Minor NC** — isolated lapse. Must have a CAPA.
- **Observation / OFI (Opportunity for Improvement)** — not a failure; a recommendation.

### SOC 2 findings

- Auditor reports **exceptions** in Section 4's "Results of Tests." A single exception may or may not lead to a **qualified opinion** depending on materiality. Multiple exceptions in the same area usually imply a **deficiency** (potentially significant or material).

### What a well-written finding looks like

- **Condition:** what was found.
- **Criteria:** which requirement it violates.
- **Cause:** why it happened.
- **Consequence:** what harm could result.
- **Recommendation:** suggested remediation.

### CAPA — Corrective Action Plan

Every finding needs a CAPA with:
- Root cause analysis (5-whys is the cheap default).
- Correction (fix this instance).
- Corrective action (prevent recurrence — policy/training/tooling).
- Owner + due date.
- Evidence of closure.
- Verification by auditor.

---

## 9.5 Preparing your organisation for audit

This is the actual GRC analyst job description.

### 90 days before

- Confirm scope with auditor.
- Update SoA (ISO) / System Description (SOC 2).
- Re-run a gap assessment against current controls.
- Freeze policy updates (or version carefully).
- Communicate timeline to engineering and ops leaders.

### 60 days before

- Evidence collection automations in place (GRC tool + direct pulls).
- Mock interviews with 5–10 SMEs who will face auditors.
- Remediate low-hanging gaps.

### 30 days before

- Clean-up sweep: open tickets older than SLA, orphan accounts, expired certs, missing policy acknowledgements.
- Finalise evidence request list from auditor (PBC — "Prepared by Client").

### During fieldwork

- Triaged daily stand-up between audit manager and team.
- Single channel for auditor requests (Slack / email alias).
- SMEs coached: answer what's asked; show evidence; admit unknowns; don't volunteer off-topic info.
- Document every conversation in case of follow-up.

### After fieldwork

- Draft findings: negotiate ambiguities, provide missing evidence, push back where the finding is mis-stated.
- Management response: concise, own the gap, present CAPA.
- Communicate outcome to leadership and customers.

---

## 9.6 Sampling — how auditors actually test

Auditors cannot look at everything. They **sample**.

- **Population** — e.g., all change-tickets in the observation period.
- **Sample size** — varies by framework and control frequency. Typical SOC 2 rules of thumb:

| Control frequency | Sample size (typical) |
|-------------------|----------------------|
| Annual | 1 |
| Quarterly | 2 |
| Monthly | 2–5 |
| Weekly | 5–15 |
| Daily | 20–40 |
| Many-times-daily | 25+ statistical |

If the population is very small and even one exception occurs, it's often reported. Control discipline matters.

---

## 9.7 Internal audit — the ISO 27001 Clause 9.2 requirement

Before your external audit, **you must audit yourself**. Internal audit should be:

- **Independent** — auditors don't audit their own work.
- **Competent** — trained internal auditors (or external hire).
- **Risk-based** — frequency tied to risk.
- **Documented** — reports, findings, closure evidence.
- **Reported** — to top management in the management review meeting.

A robust internal audit programme halves external-audit surprises.

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

Auditors read the minutes. Thin minutes = thin assurance.

---

## 9.9 Customer/second-party audits — the grind

Large customers (banks, insurers, telcos, governments) run their own audits on vendors. Typical ask:

- SIG or proprietary 300-question DDQ.
- On-site (or virtual) 2–5 day review.
- Specific evidence: pen-test reports, policy pack, ISMS manual, recent incident log, vendor DDQs you ran on your sub-processors, DR-test results.
- Right-to-audit clause invoked.

**Playbook:** build an evidence room / trust center (even a Notion page) that preempts 80% of questions. Tools: SafeBase, Vanta Trust, Drata Trust Center, Whistic.

---

## 9.10 Common beginner mistakes

- Producing evidence *only for the auditor*. You need living controls with always-on evidence — audits just sample the stream.
- Taking findings personally. Findings are how organisations improve. Own them.
- Over-promising in CAPA ("we'll fix in 30 days"). Be realistic; missed CAPA dates become Major NCs.
- Re-writing the system description when reality changes, post-hoc. Change the description *and* the system together.
- Treating internal audit as a formality. A weak internal audit exposes you later.

---

## 9.11 Interview traps

- **Q:** "What's the difference between a test of design and test of operating effectiveness?" Design = control is set up right; effectiveness = it actually runs consistently over the period.
- **Q:** "What would you do if you discover an undocumented control exception mid-audit?" Disclose, document, propose remediation. Hiding it becomes a larger finding when discovered.
- **Q:** "What's the difference between an observation and a minor NC in ISO 27001?" Observation is a recommendation, no failure. Minor NC = requirement not met, requires CAPA.

## 9.12 Mini-exercise (30 min)

For the control *"Terminated employees lose access within 24 hours"*:
1. List 5 evidence artefacts an auditor would accept.
2. Describe how you'd sample a year's terminations.
3. Draft a finding (Condition/Criteria/Cause/Consequence/Recommendation) for a case where one ex-employee kept GitHub access for 10 days.

## 9.13 Go deeper

- 🏛 [ISO 19011 — guidelines for auditing management systems](https://www.iso.org/standard/70017.html)
- 🏛 [IAF Mandatory Documents (free)](https://iaf.nu/en/iaf-documents/)
- 🏛 [AICPA Trust Services Criteria](https://www.aicpa-cima.com/resources/download/soc-for-service-organizations-trust-services-criteria)
- 📰 [AuditBoard resources](https://www.auditboard.com/resources/) · [Secureframe audit guides](https://secureframe.com/hub)
- 📘 [ISACA CISA exam topics — free outlines](https://www.isaca.org/credentialing/cisa)
- 📰 [SANS Audit whitepapers](https://www.sans.org/white-papers/?focus-area=audit-compliance)

## Module 9 — Glossary recap

Internal / second-party / third-party audit, Attestation, Certification, Accredited body, Scope, Criteria, Observation period, Fieldwork, Readiness assessment, PBC list, Evidence types (Observation/Inquiry/Inspection/Re-performance), Walkthrough, Sampling, Test of design, Test of operating effectiveness, Major NC, Minor NC, OFI, Exception, Qualified opinion, Deficiency, CAPA, 5-Whys, Clause 9.2 Internal audit, Clause 9.3 Management review, Surveillance audit, Trust center.

→ Next: [Module 10 — Policies, Standards & Procedures](10-policies.md)

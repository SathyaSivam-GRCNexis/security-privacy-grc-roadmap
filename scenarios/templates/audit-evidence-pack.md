# Audit Evidence Pack

> **Template note.** An evidence pack is what you hand to an auditor, internal, external or customer, to show that a control is **designed properly** and **operating effectively** across the period under review. The single biggest reason audits drag on is bad evidence. Missing dates. Screenshots with no context. Samples that do not cover the whole period. Policies handed over as proof of practice when they are not.
>
> One folder per control. Audit firms use different conventions. This template works for SOC 2, ISO 27001, and most customer audits. If your auditor is prescriptive about labels, adapt to theirs and keep the structure.
>
> **Audit / Engagement:** _________ · **Period under review:** _________ to _________ · **Pack prepared by:** _________ · **Date:** _________

---

## How to use this pack

For each control in scope:

1. Create one folder named `<control-id>_<short-name>` (e.g. `A.8.5_secure-authentication`, `CC6.1_logical-access`).
2. Inside the folder, place a copy of this template renamed to `00-evidence-summary.md`.
3. Drop the supporting files into the same folder, named per the convention in section 5.
4. Cross-reference everything from the summary. Auditors should not have to hunt.

The pack is a self-contained narrative. An auditor should be able to open the folder cold and work out what the control is, why it matters, who owns it, and how you know it works.

---

## 1. Control identification

| Field | Value |
|---|---|
| Framework | ISO 27001:2022 / SOC 2 / NIST CSF 2.0 / [other] |
| Control reference | e.g., A.8.5 / CC6.1 / PR.AA-01 |
| Control name | _________ |
| Control objective (one sentence) | _________ |
| Control owner (role and named person) | _________ |
| Control operator (who performs the activity, if different) | _________ |
| Frequency | Continuous / daily / weekly / monthly / quarterly / annually / event-driven |
| First implemented (date) | _________ |
| Period covered by this evidence pack | _________ to _________ |

---

## 2. Control narrative

Three short paragraphs.

**What the control does, in plain English.** A non-specialist should be able to read this and follow it with no prior context.

> Example: *Multi-factor authentication is enforced for all employees accessing production systems. When a user signs in to the corporate IdP, they must present a password and a second factor (TOTP via authenticator app, or hardware key). Logins without the second factor are rejected at the IdP, before any application is reached.*

**How it is implemented.** Name the systems, the configurations, the policies, the procedures. Reference policy ID and version. Reference system documentation.

**How it is monitored.** What tells you, on a recurring basis, that the control is operating? Logs, alerts, reports, reviews. Be specific.

---

## 3. Linked policies and standards

| Document ID | Title | Version | Section relevant to this control |
|---|---|---|---|
| | | | |
| | | | |

---

## 4. Linked risks

The control is intended to mitigate the following risks from the risk register:

| Risk ID | Risk title | Inherent score | Residual score |
|---|---|---|---|
| | | | |

---

## 5. Evidence inventory

For SOC 2 Type II and ISO 27001 surveillance audits, evidence has to cover the **entire** period under review, not just a snapshot on the day. Auditors sample. Give them a population to sample from, and the means to sample randomly.

**File-naming convention:** `<seq>_<evidence-type>_<date-or-range>.<ext>` (e.g. `01_screenshot_idp-mfa-enforcement_2026-04-15.png`, `02_log-export_login-events_2026-01-01_to_2026-03-31.csv`).

| # | Evidence file | What it demonstrates | Date or range covered | Source system | Captured by | Notes |
|---|---|---|---|---|---|---|
| 01 | 01_screenshot_idp-mfa-policy_2026-04-15.png | IdP configuration showing the MFA policy enabled and assigned to all members of the production-access group. | Configuration as at 15/04/2026; policy in place since 14/02/2025. | Okta | | Annotated to highlight the relevant setting. |
| 02 | 02_log-export_login-events_2026-01-01_to_2026-03-31.csv | Full population of production-system logins in Q1 2026, with the MFA factor recorded for each event. | 01/01/2026 to 31/03/2026 | Okta SystemLog API | | Total events: _________. Auditor may sample. |
| 03 | 03_report_quarterly-access-review_2026-Q1.pdf | Quarterly access review showing MFA enrolment for 100% of production-access group members. | Q1 2026 review completed 10/04/2026 | Internal report | | Review log signed off by Head of IT. |
| 04 | 04_screenshot_user-mfa-enrolment_2026-04-15.png | List from Okta showing all production-access users enrolled with at least one second factor. | As at 15/04/2026 | Okta | | |
| 05 | 05_policy_information-security_v3-1.pdf | Information Security Policy v3.1 §6.4 mandating MFA for production access. | Effective 14/02/2025 | Policy library | | |

Add rows as needed. The inventory is the audit-ready surface. The files in the folder are the proof.

---

## 6. Tests of operating effectiveness

For Type II and surveillance audits, design evidence alone is not enough. Describe the tests you have performed, or are inviting the auditor to perform, to confirm the control is **operating** as designed across the period.

| Test # | Test description | Population | Sample size | Result | Tester | Date |
|---|---|---|---|---|---|---|
| T1 | For a sample of production-access logins in the period, confirm a second factor was used. | All production logins Q1 2026 (n = _________) | 25 | 25 / 25 passed | Internal Audit | _________ |
| T2 | For all employees who joined the production-access group in the period, confirm MFA was enrolled within 3 working days of access being granted. | All joiners Q1 2026 (n = _________) | 100% | _________ | Internal Audit | _________ |
| T3 | For all employees who left the production-access group in the period, confirm access was removed within 1 working day. | All leavers Q1 2026 (n = _________) | 100% | _________ | Internal Audit | _________ |

**Findings from internal testing:** _________ (state any exceptions, including remediation. Do not hide exceptions. Auditors find them, and trust drops.)

---

## 7. Known exceptions and remediations

Record honestly any case where the control did not operate as designed during the period. Include root cause and remediation.

| Date | Exception | Root cause | Remediation | Status |
|---|---|---|---|---|
| | | | | |

If there are no exceptions, state: *No exceptions identified during the period under review.*

---

## 8. Sign-off

| Role | Name | Confirmation | Date |
|---|---|---|---|
| Control owner | | I confirm the evidence in this pack is complete, accurate, and covers the period stated. | |
| Internal audit / GRC reviewer | | I confirm I have reviewed the pack and tested the evidence per section 6. | |

---

## 9. Auditor notes (leave blank)

*This section is for the external auditor's working notes. Leave it blank when handing over.*

---

*Filed at: `audits/<engagement>/<control-folder>/00-evidence-summary.md`. Linked from the master evidence index `audits/<engagement>/00-evidence-index.md`.*

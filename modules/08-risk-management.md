# Module 8: Risk Management in Practice

> **Audience:** 🟡 🔴 · **Time:** ~75 min · **Prereqs:** Modules 0, 7

## Why this matters

"Risk" is the word every executive understands. A credible, prioritised risk register justifies budget, drives decisions, and passes audits. This module takes you from Module 0 vocabulary to the actual artefacts of a working programme.

Strong opinion up front: **don't reach for quantitative scoring early**. In my experience, FAIR and Monte Carlo simulations become spreadsheet theatre at small companies because the input ranges are guessed. Disciplined qualitative scoring with named owners and honest residual ratings beats a precision-looking dollar number that nobody trusts. Move to quantitative when (a) the data exists, and (b) the CFO is actually asking for it.

---

## 8.1 The risk management lifecycle (NIST SP 800-37 RMF)

Risk management is a process, not a one-off:

1. **Prepare**: scope, roles, appetite.
2. **Categorise**: classify systems and data by impact.
3. **Select controls**: from a catalog (800-53, ISO Annex A).
4. **Implement**: deploy.
5. **Assess**: do they work?
6. **Authorise**: formal sign-off (ATO in gov).
7. **Monitor**: continuously, re-assess on change.

Commercial orgs rarely formalise ATOs. The rhythm (identify → assess → treat → monitor) is universal.

## 8.2 Building a risk assessment

### Step 1: Scope

Define what you're assessing (an app, a process, the whole company) and which risk universe applies (security, privacy, operational, regulatory, financial, reputational, strategic, third-party, AI).

### Step 2: Identify risks

Sources:
- Threat catalogs (MITRE ATT&CK, ENISA).
- Past incidents, including near-misses. The near-miss list is more useful than people think.
- Vulnerability scan output.
- Audit findings.
- Workshop with business owners.
- Peer breach analysis.

**Phrasing rule:** *"\<Threat actor\> exploits \<vulnerability\> leading to \<impact\>."* One-word risks like "ransomware" or "compliance" are useless and auditors will push back. Specific risks lead to specific controls.

### Step 3: Analyse (score likelihood and impact)

**Likelihood (qualitative, 1–5):**
1. Rare (< 5% per year)
2. Unlikely
3. Possible
4. Likely
5. Almost certain

**Impact (qualitative, 1–5):** define *per domain*. Financial loss ranges, customer count, regulatory fine bands, downtime hours, reputation tier. Without defined bands, every risk drifts to "3" and the register becomes useless.

Multiply for a 5×5 heatmap.

### Step 4: Evaluate against appetite

Compare residual risk to appetite. Above appetite, treat. Within appetite, monitor and document the decision.

### Step 5: Treat

Four options:
- **Mitigate**: add controls.
- **Avoid**: stop doing the risky thing.
- **Transfer**: insurance, contractual indemnity. Transfer doesn't actually remove the risk, it just shifts the financial impact. Auditors and regulators know.
- **Accept**: document acceptance at the right management level.

Acceptance is the option most abused. "We accept this" written by a security analyst means nothing. It needs to be signed by the person who'd carry the consequence, with a date and a review trigger.

### Step 6: Monitor

KRIs, quarterly reviews, trigger-based re-assessment on major change. Quarterly is the minimum cadence I'd defend in an audit.

---

## 8.3 Quantitative risk: FAIR in 5 minutes

**FAIR** (Factor Analysis of Information Risk) decomposes risk into:

- **Loss Event Frequency** = Threat Event Frequency × Vulnerability.
- **Loss Magnitude** = Primary loss + Secondary loss.

Estimate ranges, run a Monte Carlo, get loss distributions in dollars. Genuinely useful when you have data and a CFO who reads the model. Tools: FAIR-U (free), RiskLens (paid).

Where it goes wrong in practice: people guess the input ranges from gut feel, then present the output as if the simulation made it scientific. It didn't. Use FAIR for your top 5 to 10 risks where you can defend the inputs. Don't try to FAIR a whole register.

---

## 8.4 The risk register: the artefact

Minimum fields:

| Field | Example |
|-------|---------|
| ID | R-045 |
| Title | "Customer PII exposed via unauthorised API access" |
| Description | Detailed sentence as above |
| Category | Security / Privacy / etc. |
| Inherent L × I | 4 × 5 = 20 |
| Existing controls | "API gateway with OAuth; WAF; logging" |
| Control effectiveness | H/M/L |
| Residual L × I | 2 × 4 = 8 |
| Treatment | Mitigate |
| Action plan | Add fine-grained authz; threat-model API |
| Owner | Eng Lead: Platform |
| Due date | 2026-08-15 |
| Status | In progress |
| Last reviewed | 2026-04-01 |
| Risk appetite comparison | Above appetite |

Spreadsheet, Notion DB, or a GRC tool (Vanta, Drata, Secureframe, AuditBoard, ServiceNow). The tool doesn't matter early. Discipline matters. I have seen Notion-based registers run circles around six-figure GRC platforms.

---

## 8.5 Risk governance: who owns risk?

- **Risk owner**: a business leader accountable. Usually not security. This trips up new programmes: if the CISO owns every risk, no business decision-maker is on the hook.
- **Control owner**: operational, runs the control.
- **Risk steward / analyst**: facilitates, maintains the register.
- **CRO / CISO**: aggregates, reports upstream.
- **Risk committee / board risk committee**: reviews top risks, sets appetite.

**Three Lines of Defence (IIA model):**
- 1st line: business operations own and manage risks.
- 2nd line: risk and compliance oversee.
- 3rd line: internal audit provides independent assurance.

Auditors and regulators expect to see these roles and escalation paths on paper.

---

## 8.6 Risk appetite and tolerance

**Risk appetite** = qualitative statement of how much risk the org will take. Example: "we will not accept any risk that could lead to unrecoverable loss of customer data."

**Risk tolerance** = quantitative limits. Example: "we tolerate up to 2 critical vulnerabilities unpatched for no more than 7 days."

Write a one-page appetite statement and get it signed by the board or exec team. Without it, every risk decision becomes ad-hoc.

---

## 8.7 Emerging risk categories you must track in 2026

- **AI / LLM risks**: prompt injection, training-data leakage, hallucination-driven operational risk, bias, IP leakage, third-party AI vendors. Most teams underestimate the third-party piece.
- **Supply-chain / software-supply-chain**: SBOM, SLSA, dependency compromise.
- **Geopolitical / sanctions**: export controls on cryptography, AI, and cloud services.
- **Climate / ESG**: data-centre sustainability, regulatory disclosures.
- **Deepfake / BEC**: voice and video fraud of executives. This is no longer theoretical.
- **Quantum**: "harvest now, decrypt later" for long-lived secrets.
- **Insider threat**: including the trade-offs of remote-work monitoring.

Add each to your risk universe. Don't wait for the incident.

---

## 8.8 KPIs vs KRIs (and how to report them)

- **KPI**: *have we achieved a goal?* ("95% of servers patched within SLA this quarter.")
- **KRI**: *are we approaching a dangerous level?* ("Critical vulns open > 30 days. Threshold: > 5 triggers escalation.")

Report to execs with:
- Top 10 risks, one slide.
- Trend (heatmap this quarter vs last).
- Key mitigations in progress.
- KRIs against thresholds.
- Incidents since last report.

Keep it short. A 40-slide risk pack to a board gets ignored. A two-page memo gets read.

---

## 8.9 Worked example: risk assessment for an EdTech platform

Scope: customer-facing web and mobile platform, including a resume-review AI feature.

### Enumerated risks (illustrative)

| ID | Risk | Inherent | Controls | Residual | Treatment |
|----|------|---------|----------|---------|-----------|
| R01 | Credential stuffing → account takeover | 4×4 | Rate limit, breached-password check, MFA | 2×3 | Monitor |
| R02 | S3 misconfig → resume leak | 3×5 | SCP, CSPM, bucket policy guardrails | 1×4 | Monitor |
| R03 | Vendor compromise (email vendor) → phishing of users | 3×4 | Vendor SOC 2 review, DMARC, SEG | 2×3 | Monitor |
| R04 | DPDP / CCPA non-compliance → fine + reputational | 3×4 | DSR workflow, consent mgmt, DPIA process | 2×3 | Monitor |
| R05 | AI resume feature leaks PII across users (prompt injection / cross-tenant) | 4×5 | Per-tenant isolation, prompt filtering, PII redaction, audit logs | 3×4 | Mitigate: open action |
| R06 | Ransomware on dev endpoints → source leak | 3×4 | EDR, MDM, offline backups, BCP | 2×3 | Monitor |

That gives you a defensible, business-readable risk picture in one table.

---

## 8.10 Common beginner mistakes

- Treating the register as a static spreadsheet. It must live. Review quarterly, update on incidents and major changes.
- Scoring inherent only. Executives want residual. So do auditors.
- Controls without owners. Unowned equals untested equals absent.
- Acceptance without authority. The person who'd carry the loss signs.
- Mixing issues (operational defects) with risks (future uncertain events). Different registers. Different conversations.
- Letting every risk drift to "3 × 3 = 9". Define your bands and enforce them.

## 8.11 Go deeper

- 🏛 [NIST SP 800-30 Rev. 1](https://csrc.nist.gov/pubs/sp/800/30/r1/final) · [SP 800-37 Rev. 2](https://csrc.nist.gov/pubs/sp/800/37/r2/final) · [SP 800-39](https://csrc.nist.gov/pubs/sp/800/39/final)
- 🏛 [ISO 31000](https://www.iso.org/iso-31000-risk-management.html) · [ISO 27005](https://www.iso.org/standard/80585.html)
- 🧪 [FAIR-U (free tool)](https://www.fairinstitute.org/fair-u) · 📘 [FAIR Institute resources](https://www.fairinstitute.org/resources)
- 📰 [Vanta: free risk register template](https://www.vanta.com/resources)
- 📰 [ISACA Journal: risk articles](https://www.isaca.org/resources/isaca-journal)

## Module 8: Glossary recap

RMF, Risk universe, Likelihood, Impact, Inherent vs residual, Heatmap, KRI, KPI, Risk appetite, Risk tolerance, FAIR, Loss Event Frequency, Primary/secondary loss, Three Lines of Defence, Risk owner, Control owner, Risk register, Risk acceptance, Treatment (Mitigate/Avoid/Transfer/Accept), ATO.

→ Next: [Module 9: Audit Lifecycle End-to-End](09-audit-lifecycle.md)

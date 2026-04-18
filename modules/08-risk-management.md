# Module 8 — Risk Management in Practice

> **Audience:** 🟡 🔴 · **Time:** ~75 min · **Prereqs:** Modules 0, 7

## Why this matters

"Risk" is the word every executive understands. If you can produce a credible, prioritised risk register, you can justify budget, drive decisions, and pass audits. This module takes you from the vocabulary (Module 0) to the actual artefacts — a real working risk program.

---

## 8.1 The risk management lifecycle (NIST SP 800-37 RMF)

Risk Management is a **process**, not a one-off.

1. **Prepare** — define scope, roles, risk appetite.
2. **Categorise** — classify systems/data by impact.
3. **Select controls** — from a catalog (800-53, ISO Annex A).
4. **Implement** — deploy.
5. **Assess** — do they work?
6. **Authorise** — formal sign-off (Authority to Operate, ATO, in gov).
7. **Monitor** — continuously; re-assess when things change.

Commercial orgs rarely formalise ATOs but the rhythm (identify → assess → treat → monitor) is universal.

## 8.2 Building a risk assessment

### Step 1 — Scope

Define *what* you're assessing (an app, a process, the whole company) and against *which* risk universe (security, privacy, operational, regulatory, financial, reputational, strategic, third-party, AI).

### Step 2 — Identify risks

Sources:
- Threat catalogs (MITRE ATT&CK, ENISA).
- Past incidents.
- Vulnerability scan output.
- Audit findings.
- Workshop with business owners.
- Peer breach analysis.

**Rule of craft:** phrase each risk as a sentence: *"\<Threat actor\> exploits \<vulnerability\> leading to \<impact on CIA / privacy / business\>."* Vague one-word risks ("ransomware") are a red flag.

### Step 3 — Analyse (score likelihood and impact)

**Likelihood scale (qualitative, 1–5):**
1. Rare (< 5 % per year)
2. Unlikely
3. Possible
4. Likely
5. Almost certain

**Impact scale (qualitative, 1–5):** define *per domain* — financial loss ranges, customer count, regulatory fine bands, downtime hours, reputation tier.

Multiply to get a 5×5 heatmap (1–25).

### Step 4 — Evaluate against appetite

Compare residual risk to your **risk appetite**. Above appetite → must treat. Within appetite → monitor.

### Step 5 — Treat

Four options:
- **Mitigate** — add controls.
- **Avoid** — stop doing the risky thing.
- **Transfer** — insurance, contractual indemnity.
- **Accept** — document acceptance at an appropriate management level.

### Step 6 — Monitor

Key Risk Indicators (KRIs), quarterly reviews, trigger-based re-assessment on major change.

---

## 8.3 Quantitative risk — FAIR in 5 minutes

**FAIR** (Factor Analysis of Information Risk) decomposes risk into:

- **Loss Event Frequency** = Threat Event Frequency × Vulnerability.
- **Loss Magnitude** = Primary loss + Secondary loss.

You estimate ranges (e.g., *"between 2 and 20 attempts per year"*), run a Monte Carlo, get loss distributions in dollars. Much more useful for CFO conversations than "High/Medium/Low."

Tools: FAIR-U (free), RiskLens (paid).

---

## 8.4 The risk register — the artefact

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
| Owner | Eng Lead — Platform |
| Due date | 2026-08-15 |
| Status | In progress |
| Last reviewed | 2026-04-01 |
| Risk appetite comparison | Above appetite |

Use a spreadsheet, Notion DB, or a GRC tool (Vanta/Drata/Secureframe/Auditboard/ServiceNow).

---

## 8.5 Risk governance — who owns risk?

- **Risk owner** — a business leader accountable (usually not security).
- **Control owner** — operational, implements & runs the control.
- **Risk steward / analyst** — facilitates, maintains register.
- **CRO / CISO** — aggregates, reports upstream.
- **Risk committee / board risk committee** — reviews top risks, sets appetite.

**Three Lines of Defence (IIA model):**
- 1st line — business operations own and manage risks.
- 2nd line — risk & compliance functions oversee.
- 3rd line — internal audit provides independent assurance.

Auditors and regulators expect to see these roles and escalation paths.

---

## 8.6 Risk appetite and tolerance

**Risk appetite** = qualitative statement of how much risk the org is willing to take ("we will not accept any risk that could lead to unrecoverable loss of customer data").

**Risk tolerance** = quantitative limits per risk ("we tolerate up to 2 critical vulnerabilities unpatched for no more than 7 days").

Boards love a one-page appetite statement. Write one.

---

## 8.7 Emerging risk categories you must track in 2026

- **AI / LLM risks** — prompt injection, training-data leakage, hallucination-driven operational risk, bias, IP leakage, third-party AI vendors.
- **Supply-chain / software-supply-chain** — SBOM, SLSA, dependency compromise.
- **Geopolitical / sanctions** — export controls on cryptography, AI, and cloud services.
- **Climate / ESG** — data-centre sustainability; regulatory disclosures.
- **Deepfake / BEC** — voice/video fraud of executives.
- **Quantum** — "harvest now, decrypt later" for long-lived secrets.
- **Insider threat** — including remote-work monitoring trade-offs.

Add each to your risk universe; don't wait for the incident.

---

## 8.8 KPIs vs KRIs (and how to report them)

- **KPI** — *have we achieved a goal?* ("95% of servers patched within SLA this quarter.")
- **KRI** — *are we approaching a dangerous level?* ("Critical vulns open > 30 days." Threshold: > 5 triggers escalation.)

Report to execs with:
- Top 10 risks (one slide).
- Trend (heatmap this quarter vs last).
- Key mitigations in progress.
- KRIs against thresholds.
- Incidents since last report.

---

## 8.9 Worked example — risk assessment for an EdTech platform

Scope: customer-facing web + mobile platform, including resume-review AI feature.

### A few enumerated risks

| ID | Risk | Inherent | Controls | Residual | Treatment |
|----|------|---------|----------|---------|-----------|
| R01 | Credential stuffing → account takeover | 4×4 | Rate limit, breached-password check, MFA | 2×3 | Monitor |
| R02 | S3 misconfig → resume leak | 3×5 | SCP, CSPM, bucket policy guardrails | 1×4 | Monitor |
| R03 | Vendor compromise (email vendor) → phishing of users | 3×4 | Vendor SOC 2 review, DMARC, SEG | 2×3 | Monitor |
| R04 | DPDP/CCPA non-compliance → fine + reputational | 3×4 | DSR workflow, consent mgmt, DPIA process | 2×3 | Monitor |
| R05 | AI resume feature leaks PII across users (prompt injection / cross-tenant) | 4×5 | Per-tenant isolation, prompt filtering, PII redaction, audit logs | 3×4 | Mitigate — open action |
| R06 | Ransomware on dev endpoints → source leak | 3×4 | EDR, MDM, offline backups, BCP | 2×3 | Monitor |

Now you have a defensible, business-readable risk picture.

---

## 8.10 Common beginner mistakes

- Treating risk register as a static spreadsheet. It must live: reviewed quarterly, updated on incidents/changes.
- Inherent-only scoring. Executives want **residual**.
- Controls without owners. Unowned = untested = absent.
- Accepting without authority. Acceptance needs the person who owns the consequence.
- Mixing issues (operational defects) with risks (future uncertain events). Keep separate registers.

## 8.11 Go deeper

- 🏛 [NIST SP 800-30 Rev. 1](https://csrc.nist.gov/pubs/sp/800/30/r1/final) · [SP 800-37 Rev. 2](https://csrc.nist.gov/pubs/sp/800/37/r2/final) · [SP 800-39](https://csrc.nist.gov/pubs/sp/800/39/final)
- 🏛 [ISO 31000](https://www.iso.org/iso-31000-risk-management.html) (overview) · [ISO 27005](https://www.iso.org/standard/80585.html)
- 🧪 [FAIR-U (free tool)](https://www.fairinstitute.org/fair-u) · 📘 [FAIR Institute resources](https://www.fairinstitute.org/resources)
- 📰 [Vanta — free risk register template](https://www.vanta.com/resources) (search "risk")
- 📰 [ISACA Journal — risk articles](https://www.isaca.org/resources/isaca-journal)

## Module 8 — Glossary recap

RMF, Risk universe, Likelihood, Impact, Inherent vs residual, Heatmap, KRI, KPI, Risk appetite, Risk tolerance, FAIR, Loss Event Frequency, Primary/secondary loss, Three Lines of Defence, Risk owner, Control owner, Risk register, Risk acceptance, Treatment (Mitigate/Avoid/Transfer/Accept), ATO.

→ Next: [Module 9 — Audit Lifecycle End-to-End](09-audit-lifecycle.md)

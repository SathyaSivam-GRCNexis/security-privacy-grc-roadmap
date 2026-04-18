# Incident Response Runbook

> **Template note.** A runbook is not a policy. A policy says "we will respond to incidents." A runbook tells the on-call engineer at 02:47 IST what to do in the next ten minutes. Keep it short, scannable, and rehearsed. If your runbook is twelve pages long, nobody reads it during a real incident.
>
> Maintain one runbook per scenario type (credential stuffing, ransomware, data exposure, account takeover, vendor outage, insider misuse). This template is the credential-stuffing variant. Copy and adapt.
>
> **Runbook ID:** RB-_________ · **Scenario:** _________ · **Owner:** _________ · **Last reviewed:** _________ · **Last tested (tabletop):** _________

---

## When to use this runbook

Trigger this runbook when **any** of the following are true:

- Authentication monitoring shows login attempts > 1,000 per minute against a single endpoint.
- Failed-login rate exceeds 10x the rolling 7-day baseline for more than 5 minutes.
- A successful login originates from an IP, ASN, or geography not previously associated with the user, accompanied by access to sensitive data endpoints.
- A user reports their account was accessed without authorisation.
- A threat-intel feed reports your domain or user credentials in a fresh dump.

If unsure, escalate. False alarms are cheaper than late responses.

---

## Roles for this incident

| Role | Who | How to reach |
|---|---|---|
| Incident Commander (IC) | On-call SecOps lead | PagerDuty schedule "secops-primary" |
| Communications lead | Comms duty manager | PagerDuty schedule "comms-primary" |
| Engineering responder | On-call SRE | PagerDuty schedule "sre-primary" |
| Legal counsel | Legal duty | Phone tree, see contact sheet |
| DPO | DPO on-call | Phone tree |
| Executive sponsor | CISO (deputy: CTO) | Phone tree |

The IC is the single decision-maker. The IC does not type into systems — the IC directs others. If the IC is also the responder, you are understaffed and that is itself a finding.

---

## Severity definitions

- **SEV-1:** Confirmed unauthorised access to customer personal data, or active exfiltration in progress.
- **SEV-2:** Highly likely unauthorised access; investigation in progress; no confirmed exfiltration.
- **SEV-3:** Anomalous activity detected; impact unclear; investigation in early stages.

Default this scenario to **SEV-2** until evidence reduces or escalates it.

---

## Phase 1 — Detect & Triage (T+0 to T+30 min)

| # | Action | Owner | Done? |
|---|---|---|---|
| 1 | Acknowledge the alert in SIEM. Stop the alert from re-paging. | On-call | |
| 2 | Open an incident ticket. Assign IC. Open the war-room channel `#inc-YYYYMMDD-shortname`. | On-call | |
| 3 | Capture the alert payload, source IPs, ASNs, target endpoints, time window. Snapshot dashboards. **Do not clear logs.** | On-call | |
| 4 | Confirm the indicator: is the failed-login rate genuinely anomalous, or is it a marketing campaign / load test? | On-call | |
| 5 | Set initial severity. Page IC if SEV-1 or SEV-2. | On-call | |
| 6 | IC takes command. State explicitly: "I am IC for this incident." | IC | |

---

## Phase 2 — Contain (T+30 min to T+2 hr)

The goal of containment is to stop the bleeding **without breaking legitimate users**. Aggressive responses (mass password reset, full IP block) cause customer-facing harm and should be a last resort, not a first.

| # | Action | Owner | Done? |
|---|---|---|---|
| 7 | Apply rate-limit at the WAF for the affected endpoint. Start with 10 requests / IP / minute. | SRE | |
| 8 | Enable bot-detection challenge (CAPTCHA / JS challenge) on the login endpoint. | SRE | |
| 9 | Block the top offending ASNs if traffic is geographically concentrated and not customer-relevant. | SRE | |
| 10 | Pull a list of accounts with **successful** logins from suspicious IPs in the affected window. This is the confirmed-affected list. | SecOps | |
| 11 | Force password reset and session invalidation **only for confirmed-affected accounts**. Do not mass-reset. | SecOps | |
| 12 | Enable enhanced logging on the affected endpoint and on accounts that successfully logged in. | SecOps | |
| 13 | Preserve evidence: snapshot SIEM data, WAF logs, application logs for the affected window. Tag for legal hold. | SecOps | |

**Stop and reassess.** Has the rate dropped? Are legitimate users complaining? Are confirmed-affected accounts active in unusual ways (e.g., bulk download)?

---

## Phase 3 — Investigate & Assess (T+2 hr to T+6 hr)

This phase runs in parallel with regulatory clock decisions in Phase 4.

| # | Action | Owner | Done? |
|---|---|---|---|
| 14 | For each confirmed-affected account, identify what was accessed. Specifically: did they hit endpoints that return personal data, financial data, or SPDI? | SecOps | |
| 15 | Determine whether **data was exfiltrated** (downloaded, exported, emailed) versus only viewed. The distinction matters for regulator notifications. | SecOps | |
| 16 | Identify the source of the credentials. Reused passwords from a third-party breach? Phishing? Insider? A combination? | SecOps + AppSec | |
| 17 | Document the timeline minute-by-minute. Use the language "what we know" vs "what we assume." | IC | |
| 18 | Brief the executive sponsor. Hold the decision on customer notification until Phase 4. | IC | |

---

## Phase 4 — Notify (regulatory clocks start at detection)

**Read the [`reference/regulator-clocks.md`](../../reference/regulator-clocks.md) document before this incident if you have not already.** During an incident is the wrong time to learn the clocks.

| # | Action | Owner | Trigger | Deadline | Done? |
|---|---|---|---|---|---|
| 19 | Decide if CERT-In Annexure I applies (any "cyber incident" under the 2022 directions). | IC + Legal | Always assess | **6 hours from awareness** | |
| 20 | If personal data of EU residents is affected: notify the lead supervisory authority. | DPO | Risk to data subject rights | **72 hours from awareness** (GDPR Art. 33) | |
| 21 | If personal data of Indian residents is affected: notify the Data Protection Board of India. | DPO | DPDP Sec. 8(6) | "Without delay" — operate to **72 hours** | |
| 22 | Notify affected customers individually if high risk to their rights and freedoms. | DPO + Comms | GDPR Art. 34 / DPDP | "Without undue delay" | |
| 23 | Notify business partners, payment processors, or others under contractual obligations. | Legal + Comms | Contracts | Per contract | |
| 24 | Prepare a public statement only if media interest is likely or confirmed. **Legal must review before publication.** | Comms + Legal | Reputational | As needed | |

**Critical principle.** Do not make public statements before you can defend them. The difference between "Your data was accessed" (a fact, often unverified at this stage) and "We detected unusual activity on a subset of accounts and are investigating" (honest until proven) is career-defining.

---

## Phase 5 — Eradicate & Recover (T+6 hr onwards)

| # | Action | Owner | Done? |
|---|---|---|---|
| 25 | Confirm the attack vector is closed. Verify rate-limits, bot detection, and any deployed code changes are operating. | SRE | |
| 26 | Re-enable any flows that were degraded for safety. Do this incrementally. | SRE | |
| 27 | For affected users, document the support flow for re-onboarding. Brief the support team on the agreed messaging. | Comms + Support | |
| 28 | Hold a joint debrief at T+24 hr. Confirm IC handover or stand-down. | IC | |

---

## Phase 6 — Postmortem (within 5 working days)

A postmortem that names individuals as the cause is a failed postmortem. The output of a postmortem is **systemic improvement**, not blame.

A good postmortem includes:

1. **Timeline.** Minute-by-minute, factual.
2. **What we know vs what we assume.** Be explicit.
3. **Five whys** for each contributing factor. Stop only when the answer becomes "this is how the system was designed."
4. **Customer impact.** Numbers, not adjectives.
5. **Action items.** Each with an owner, a date, and a way to verify completion. No "investigate further" — that is not an action item.
6. **What went well.** Strengthen what worked, do not only fix what broke.

The postmortem is shared with the executive sponsor, the engineering team, and (with appropriate sanitisation) the affected customers if they ask.

---

## Pre-incident readiness checklist

Run this checklist quarterly. If any item is "no," it is a finding for your next risk review.

- [ ] WAF rate-limit rules deployed and tested.
- [ ] Bot detection / CAPTCHA available and rehearsed.
- [ ] MFA enforced for all users (or a documented exception register).
- [ ] SIEM alerts for credential-stuffing patterns tuned and tested.
- [ ] PagerDuty schedules current; deputies named.
- [ ] CERT-In Annexure I template in `incident-templates/` folder, owner identified.
- [ ] Customer notification template drafted and legal-reviewed.
- [ ] Tabletop exercise for this scenario run within last 6 months.
- [ ] War-room channel naming convention documented.
- [ ] Legal-hold process documented and tested.

---

## Document control

| Version | Date | Change | Author |
|---|---|---|---|
| 1.0 | 17/04/2026 | Initial version | |

---

*Linked from: Incident Response Policy POL-008 · Tested in tabletop exercise log TBL-_________ · Cross-referenced from [Module 11](../../modules/11-incident-response-and-bcp.md).*

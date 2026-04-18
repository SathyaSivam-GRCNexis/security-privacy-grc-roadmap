# Module 11 — Incident Response, BCP & DR

> **Audience:** 🟡 🔴 · **Time:** ~90 min · **Prereqs:** Modules 0–4

## Why this matters

Every organisation *will* have incidents. You're measured not on whether one happens, but on **how fast you detect, contain, communicate, and recover**. This module covers the three overlapping disciplines: **IR** (what to do when attacked), **BCP** (how to keep the business running), **DR** (how to recover IT systems).

---

## 11.1 The NIST IR lifecycle (SP 800-61 Rev. 2)

Four phases in a continuous loop:

1. **Preparation** — before anything happens.
2. **Detection & Analysis** — noticing something is wrong; triaging.
3. **Containment, Eradication, Recovery** — stopping the bleeding; removing the attacker; restoring service.
4. **Post-Incident Activity** — lessons learned, improvements, reporting.

Most orgs skimp on 1 and 4. Those are exactly the phases that make 2 and 3 succeed.

### Preparation — the boring phase that wins

- **IR team** with defined roles (commander, comms lead, investigator, legal, HR if insider).
- **Playbooks** for top-10 scenarios.
- **Contact tree** (on-call rotation, execs, vendors, regulators, counsel, insurer, PR).
- **Tools ready** — forensic workstation, clean laptops, secure comms channel (Signal/Keybase/out-of-band), SIEM query templates.
- **Legal pre-retainer** for incident counsel (attorney-client privilege helps).
- **Cyber insurance** — know the claim process and required steps.
- **Tabletop exercises** — quarterly; senior-leadership annually.

### Detection sources

- SIEM alerts, EDR alerts, WAF logs, IDS, anomaly detection.
- User reports ("I clicked this phishing link"), help-desk tickets.
- External notifications — law enforcement, CERT, customer, researcher, dark-web monitor.
- Bug-bounty / responsible disclosure.

### Triage & severity

Define a severity scale (SEV1–SEV5 typical). Each severity maps to response SLAs, who wakes up, who decides to declare an incident.

| Sev | Criteria | Response |
|-----|----------|---------|
| 1 | Confirmed breach / major outage / PII exfil | All hands, 24×7, exec + board notified |
| 2 | Major security event, likely breach | IR commander + full team |
| 3 | Confirmed attack, limited scope | IR team during business hours |
| 4 | Minor event | Single on-call |
| 5 | Noise / false positive | Close with notes |

### Containment

- **Short-term** — isolate affected hosts, revoke credentials, block IPs.
- **Long-term** — maintain evidence, preserve forensics, plan clean rebuild.
- **Evidence preservation** — chain of custody, disk images, memory dumps, network captures.

### Eradication

Remove malware, close exploited vuln, rotate credentials and secrets, rebuild from clean images, invalidate tokens/sessions.

### Recovery

Return to normal operations — phased, with monitoring for re-infection. Validation tests before declaring "recovered."

### Post-incident

- **Timeline reconstruction** (minute-by-minute).
- **Root cause** (technical + process + human).
- **What went well, what didn't.**
- **Actions** with owners and dates.
- **Communicate** to execs, board, regulators, affected parties.
- **Update playbooks and detection rules.**

---

## 11.2 Communication — the part that makes or breaks careers

### Internal

- Pre-agreed channels. If your primary chat is compromised, switch to out-of-band (Signal/phone).
- **Minimum information, wide distribution** — "Confirmed incident; SEV1; IR commander = X; updates hourly." Avoid speculation.
- Never discuss attacker identity or attribution externally without legal sign-off.

### External — customer communication

- Be honest, timely, factual.
- Acknowledge what happened, what data was affected, what you're doing.
- Provide a status page; update regularly.
- Offer remediation (password reset, credit monitoring as appropriate).

### Regulator communication

- Know your clocks (Module 6):
  - **GDPR:** 72 hours to supervisory authority.
  - **HIPAA:** 60 days (individuals + HHS); media if > 500 residents of a state.
  - **CERT-In:** **6 hours.**
  - **DPDP:** per DPBI rules.
  - **PCI-DSS:** immediately to acquirer and card brands.
  - **State AGs (US):** varies; many by 30–60 days.
  - **SEC (US public companies):** 4 business days after materiality determination (Form 8-K Item 1.05, since Dec 2023).

Build a **regulator-notification matrix** keyed to data categories and jurisdictions. Pre-draft notification templates.

### Law enforcement

- In India: CERT-In, local police cyber cell.
- In US: FBI IC3, Secret Service.
- In EU: national CERTs, local police.
- Involvement is your call (and legal's). Cooperate carefully; preserve evidence.

---

## 11.3 Playbooks — the top scenarios to pre-write

1. Phishing with credential theft.
2. Ransomware.
3. Malware infection on endpoint.
4. Suspicious admin activity / insider.
5. Cloud key compromise (AWS, Azure, GCP).
6. Customer data exfiltration.
7. DDoS.
8. Website defacement.
9. Third-party vendor breach.
10. Lost / stolen laptop.
11. Social-engineering of support staff.
12. Source-code leak.
13. AI model / prompt-injection incident.
14. Deepfake BEC of executive.

Each playbook: trigger criteria, immediate actions, investigation steps, containment actions, communications, evidence to capture, recovery criteria.

---

## 11.4 SOC, SIEM, SOAR — the operational backbone

- **SOC (Security Operations Centre)** — the team and function that monitors 24×7. Tiered: **T1** (triage), **T2** (investigation), **T3** (advanced hunt), plus detection engineers, IR leads.
- **SIEM** — centralised log + alerting platform. Splunk, Sentinel, Elastic, Chronicle, Sumo Logic, QRadar.
- **SOAR** — automation of response playbooks. XSOAR, Tines, Splunk SOAR.
- **XDR / MDR** — newer managed detection-and-response offerings.
- **Threat intel** — feeds (commercial + OSINT) and internal tracking (ISACs, CERT-In advisories).

Key metrics: **MTTD** (Mean Time to Detect), **MTTR** (Mean Time to Respond/Recover), **dwell time** (attacker inside to discovery).

---

## 11.5 Business Continuity vs Disaster Recovery vs Crisis Management

Commonly confused; distinct:

- **Business Continuity (BCP)** — keep **business processes** running through a disruption. Non-IT as well (manual fallback, alternate offices, remote work activation).
- **Disaster Recovery (DR)** — recover **IT systems** after a technology disruption.
- **Crisis Management** — handle **the organisation's response** (execs, PR, legal) to a major event (cyber, natural, reputational).

ISO 22301 is the BCMS standard.

### Core artefacts

- **Business Impact Analysis (BIA)** — for each business process, determine MTD, RTO, RPO, dependencies.
  - **MTD** (Maximum Tolerable Downtime) — beyond this, the business suffers unacceptably.
  - **RTO** (Recovery Time Objective) — how fast you must recover.
  - **RPO** (Recovery Point Objective) — how much data loss is acceptable.
- **BCP playbooks** — per critical process.
- **DR runbooks** — per critical system.
- **Alternate sites** — hot/warm/cold (immediate / some setup / bare-bones).
- **Backup strategy** — 3-2-1 rule: 3 copies, 2 media types, 1 offsite (modern: 3-2-1-1-0 — one offline/immutable, zero errors on restore tests).

### Testing

- **Walk-through** — discuss.
- **Tabletop** — scenario-driven discussion.
- **Simulation** — partial systems exercised.
- **Parallel** — run DR site alongside prod.
- **Full cut-over** — fail over completely (risky, high-assurance).

Regulated orgs must test **annually**. Document: scope, participants, scenario, outcome, gaps, corrective actions.

---

## 11.6 Ransomware — the big one

Modern ransomware is **double/triple extortion**: encrypt + exfiltrate + threaten disclosure + DDoS.

Defences stack:
- **Backups** — immutable, offline, tested.
- **EDR** with behavioural detection; disable PowerShell where possible.
- **Email & web filtering** to block initial access.
- **MFA everywhere**, especially on email and VPN.
- **Network segmentation** — limits blast radius.
- **Patching** — critical within days, not weeks.
- **Playbook** — includes legal and insurer involvement, communication strategy, "do we pay?" decision framework (usually no; follow counsel).
- **Anti-exfiltration** — monitor large outbound transfers; DLP.

Regulators in some jurisdictions (US OFAC) can penalise paying sanctioned groups. Know before you wire.

---

## 11.7 Worked example — a credential-stuffing incident (IR + BCP in miniature)

**0:00:** SIEM alerts spike of failed logins (1200/min) across many accounts.
**0:05:** T1 analyst triages; escalates to T2.
**0:10:** IR on-call paged; declared SEV2.
**0:15:** WAF rule to rate-limit that geography; cloud-provider bot protection engaged.
**0:30:** Identify ~50 accounts with successful logins from unusual locations; force-logout + password reset.
**1:00:** Customer support briefed; status page updated.
**2:00:** IR commander confirms no data exfil; SEV2 held.
**Day 1:** customer communications for affected users; breach assessed under DPDP / GDPR / applicable laws; legal consulted.
**Day 2:** post-mortem; actions: enforce MFA, add bot protection to IdP, investigate source of stuffed credentials.

Clean, fast, documented. That's the goal.

---

## 11.8 Go deeper

- 🏛 [NIST SP 800-61 Rev. 2](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) · [SP 800-61 Rev. 3 (draft)](https://csrc.nist.gov/pubs/sp/800/61/r3/ipd)
- 🏛 [NIST SP 800-34 Rev. 1 — Contingency Planning](https://csrc.nist.gov/pubs/sp/800/34/r1/upd1/final)
- 🏛 [CISA IR Playbooks](https://www.cisa.gov/resources-tools/resources/federal-government-cybersecurity-incident-and-vulnerability-response-playbooks)
- 🏛 [CERT-In directions (India)](https://www.cert-in.org.in/)
- 🏛 [ISO 22301 overview](https://www.iso.org/standard/75106.html)
- 📰 [SANS — IR whitepapers](https://www.sans.org/white-papers/?focus-area=incident-response)
- 🎥 [Simply Cyber — SOC paths](https://www.youtube.com/@SimplyCyber)
- 🧪 [LetsDefend free tier](https://letsdefend.io/) · [Blue Team Labs Online](https://blueteamlabs.online/) · [TryHackMe SOC Level 1 path](https://tryhackme.com/)
- 🏛 [MITRE ATT&CK](https://attack.mitre.org/) — map incidents to techniques.

## Module 11 — Glossary recap

NIST IR lifecycle, Preparation, Detection & Analysis, Containment / Eradication / Recovery, Post-incident, SEV1–SEV5, IR commander, Chain of custody, Disk image, Memory dump, Playbook, Tabletop exercise, SOC, SIEM, SOAR, XDR, MDR, MTTD, MTTR, Dwell time, BCP, DR, Crisis Management, BIA, MTD, RTO, RPO, Hot/Warm/Cold site, 3-2-1 backup, Immutable backup, Double/triple extortion ransomware, OFAC, Regulator notification matrix, 72-hour clock, 6-hour CERT-In, SEC 8-K 1.05.

→ Next: [Module 12 — Third-Party / Vendor Risk](12-vendor-risk.md)

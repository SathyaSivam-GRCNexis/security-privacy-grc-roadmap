# Module 11: Incident Response, BCP & DR

> **Audience:** 🟡 🔴 · **Time:** ~90 min · **Prereqs:** Modules 0–4

## Why this matters

Every organisation will have incidents. You're measured on how fast you detect, contain, communicate, and recover. This module covers the three overlapping disciplines: **IR** (what to do when attacked), **BCP** (how to keep the business running), **DR** (how to recover IT systems).

Three opinions worth stating up front.

First: **tabletops reveal what runbooks hide**. A clean PDF runbook looks great in audits and breaks within five minutes of a real exercise. Run the tabletop. Watch where people freeze, who can't be reached, which decision has no owner. Then fix the runbook.

Second: **the comms tree breaks first, every time**. The on-call rotation is current. The legal contact left two months ago. The PR firm's after-hours line is wrong. The insurer's claim email goes to a shared inbox nobody reads at 2am. Test the tree like you'd test backups.

Third: **most real incidents are vendor outages, not breaches**. Cyber incidents are sexier. SaaS-to-SaaS dependency outages happen weekly. Your IR programme should be honest about that distribution, and your status-page and customer-comms muscle is what gets used most.

---

## 11.1 The NIST IR lifecycle (SP 800-61 Rev. 2)

Four phases in a continuous loop:

1. **Preparation**: before anything happens.
2. **Detection & Analysis**: noticing something is wrong; triaging.
3. **Containment, Eradication, Recovery**: stop the bleeding, remove the attacker, restore service.
4. **Post-Incident Activity**: lessons learned, improvements, reporting.

Most orgs skimp on 1 and 4. Those are exactly the phases that make 2 and 3 succeed.

### Preparation: the boring phase that wins

- **IR team** with defined roles: commander, comms lead, investigator, legal, HR if insider.
- **Playbooks** for the top 10 scenarios.
- **Contact tree** with on-call rotation, execs, vendors, regulators, counsel, insurer, PR. Tested.
- **Tools ready**: forensic workstation, clean laptops, out-of-band comms channel (Signal, phone), SIEM query templates.
- **Legal pre-retainer** for incident counsel. Attorney-client privilege helps.
- **Cyber insurance**: know the claim process and required steps. Some insurers void cover if you call your own forensics first.
- **Tabletop exercises** quarterly. Senior-leadership annually.

### Detection sources

- SIEM alerts, EDR alerts, WAF logs, IDS, anomaly detection.
- User reports ("I clicked this phishing link"), help-desk tickets.
- External notifications. Law enforcement, CERT, customer, researcher, dark-web monitor.
- Bug-bounty and responsible disclosure.

In my experience, external notifications are how most serious incidents get found. Build the intake path for them. The "we have something you should know" email needs to reach a human within minutes, not days.

### Triage & severity

Define a severity scale (SEV1–SEV5 is typical). Each severity maps to response SLAs, who wakes up, who declares.

| Sev | Criteria | Response |
|-----|----------|---------|
| 1 | Confirmed breach / major outage / PII exfil | All hands, 24×7, exec + board notified |
| 2 | Major security event, likely breach | IR commander + full team |
| 3 | Confirmed attack, limited scope | IR team, business hours |
| 4 | Minor event | Single on-call |
| 5 | Noise / false positive | Close with notes |

The severity definitions are the part everyone copies and nobody tunes. Spend 30 minutes making the criteria match your business reality. "Major outage" needs a number.

### Containment

- **Short-term**: isolate hosts, revoke credentials, block IPs.
- **Long-term**: maintain evidence, preserve forensics, plan clean rebuild.
- **Evidence preservation**: chain of custody, disk images, memory dumps, network captures.

The classic mistake: rebuild the box immediately because you want service back. You just destroyed your forensics. Decide upfront which hosts get preserved and which get rebuilt.

### Eradication

Remove malware, close the exploited vuln, rotate credentials and secrets, rebuild from clean images, invalidate tokens and sessions. The "rotate secrets" step is the one teams under-do. Rotate everything the attacker could have touched, not just what you can prove they touched.

### Recovery

Return to normal operations. Phased, with monitoring for re-infection. Validation tests before declaring "recovered."

### Post-incident

- **Timeline reconstruction** (minute by minute).
- **Root cause** (technical, process, human).
- **What went well, what didn't.**
- **Actions** with owners and dates.
- **Communicate** to execs, board, regulators, affected parties.
- **Update playbooks and detection rules.**

Blameless post-mortems land better and produce more honest information than the alternative. Engineering culture knows this. GRC tends to relearn it.

---

## 11.2 Communication: the part that makes or breaks careers

### Internal

- Pre-agreed channels. If your primary chat is compromised, switch to out-of-band (Signal, phone).
- Minimum information, wide distribution. "Confirmed incident. SEV1. IR commander = X. Updates hourly." Avoid speculation.
- Never discuss attacker identity or attribution externally without legal sign-off.

### External: customer communication

- Honest, timely, factual.
- Acknowledge what happened, what data was affected, what you're doing.
- Status page, updated regularly. A silent status page during an outage is worse than a partial one.
- Offer remediation (password reset, credit monitoring as appropriate).

### Regulator communication

Know your clocks (Module 6):
- **GDPR:** 72 hours to supervisory authority.
- **HIPAA:** 60 days (individuals and HHS); media if > 500 residents of a state.
- **CERT-In:** **6 hours.** Tightest clock in the world.
- **DPDP:** per DPBI Rules.
- **PCI-DSS:** immediately to acquirer and card brands.
- **State AGs (US):** varies. Many 30 to 60 days.
- **SEC (US public companies):** 4 business days after materiality determination (Form 8-K Item 1.05, since Dec 2023).

Build a **regulator-notification matrix** keyed to data categories and jurisdictions. Pre-draft notification templates. Drafting the GDPR notice at hour 60 while running the incident is how clocks get missed.

### Law enforcement

- India: CERT-In, local police cyber cell.
- US: FBI IC3, Secret Service.
- EU: national CERTs, local police.
- Involvement is your call (and legal's). Cooperate carefully. Preserve evidence.

---

## 11.3 Playbooks: the top scenarios to pre-write

1. Phishing with credential theft.
2. Ransomware.
3. Malware infection on endpoint.
4. Suspicious admin activity / insider.
5. Cloud key compromise (AWS, Azure, GCP).
6. Customer data exfiltration.
7. DDoS.
8. Website defacement.
9. Third-party vendor breach.
10. Lost or stolen laptop.
11. Social-engineering of support staff.
12. Source-code leak.
13. AI model / prompt-injection incident.
14. Deepfake BEC of executive.

Each playbook: trigger criteria, immediate actions, investigation steps, containment, communications, evidence to capture, recovery criteria.

Three of these are quietly more important than the rest right now: vendor breach (because half your stack is third-party), cloud key compromise (because key sprawl is real), and social-engineering of support staff (because that's how most consumer-facing breaches actually start).

---

## 11.4 SOC, SIEM, SOAR: the operational backbone

- **SOC**: the team monitoring 24×7. Tiered: T1 triage, T2 investigation, T3 advanced hunt, plus detection engineers and IR leads.
- **SIEM**: centralised log and alerting platform. Splunk, Sentinel, Elastic, Chronicle, Sumo Logic, QRadar.
- **SOAR**: automation of response playbooks. XSOAR, Tines, Splunk SOAR.
- **XDR / MDR**: managed detection-and-response offerings.
- **Threat intel**: feeds (commercial and OSINT) plus internal tracking (ISACs, CERT-In advisories).

Key metrics: **MTTD** (Mean Time to Detect), **MTTR** (Mean Time to Respond/Recover), **dwell time** (attacker inside to discovery).

A reality check: smaller orgs don't need a full SOC. An MDR contract plus a clear on-call rotation and well-tuned alerts usually outperforms a homegrown SOC of two analysts.

---

## 11.5 Business Continuity vs Disaster Recovery vs Crisis Management

Commonly confused. Distinct:

- **Business Continuity (BCP)**: keep **business processes** running through a disruption. Non-IT as well (manual fallback, alternate offices, remote work activation).
- **Disaster Recovery (DR)**: recover **IT systems** after a technology disruption.
- **Crisis Management**: handle **the organisation's response** (execs, PR, legal) to a major event (cyber, natural, reputational).

ISO 22301 is the BCMS standard.

### Core artefacts

- **Business Impact Analysis (BIA)**: for each business process, determine MTD, RTO, RPO, dependencies.
  - **MTD** (Maximum Tolerable Downtime).
  - **RTO** (Recovery Time Objective).
  - **RPO** (Recovery Point Objective).
- **BCP playbooks**: per critical process.
- **DR runbooks**: per critical system.
- **Alternate sites**: hot, warm, cold.
- **Backup strategy**: 3-2-1 rule. 3 copies, 2 media types, 1 offsite. Modern: 3-2-1-1-0 (one offline or immutable, zero errors on restore tests).

The BIA is the part most teams skip or do badly. Ask the business: "if this process is down for 24 hours, what happens?" If they can't answer, your RTO is a guess. RTOs derived from gut feel get exposed during the first real outage.

### Testing

- **Walk-through**: discuss.
- **Tabletop**: scenario-driven discussion.
- **Simulation**: partial systems exercised.
- **Parallel**: run DR site alongside prod.
- **Full cut-over**: fail over completely. Risky. High-assurance.

Regulated orgs must test annually. Document: scope, participants, scenario, outcome, gaps, corrective actions. The post-test gaps are the actual value of the exercise.

---

## 11.6 Ransomware: the big one

Modern ransomware is **double or triple extortion**: encrypt, exfiltrate, threaten disclosure, sometimes DDoS.

Defences stack:
- **Backups**: immutable, offline, tested. Untested backups are not backups.
- **EDR** with behavioural detection. Disable PowerShell where possible.
- **Email and web filtering** to block initial access.
- **MFA everywhere**, especially on email and VPN.
- **Network segmentation** to limit blast radius.
- **Patching**: critical within days, not weeks.
- **Playbook** with legal and insurer involvement, comms strategy, "do we pay?" decision framework (usually no, follow counsel).
- **Anti-exfiltration**: monitor large outbound transfers, DLP.

In some jurisdictions (US OFAC) paying sanctioned groups carries its own penalty. Know before you wire. Insurers will also have opinions; that's another reason the insurer is in the call early.

---

## 11.7 Worked example: a credential-stuffing incident

Illustrative, not real.

**0:00** SIEM alerts spike of failed logins (1200/min) across many accounts.
**0:05** T1 analyst triages and escalates to T2.
**0:10** IR on-call paged. Declared SEV2.
**0:15** WAF rule rate-limits that geography. Cloud-provider bot protection engaged.
**0:30** Identify around 50 accounts with successful logins from unusual locations. Force-logout plus password reset.
**1:00** Customer support briefed. Status page updated.
**2:00** IR commander confirms no data exfil. SEV2 held.
**Day 1** Customer communications for affected users. Breach assessed under DPDP, GDPR, applicable laws. Legal consulted.
**Day 2** Post-mortem. Actions: enforce MFA, add bot protection to IdP, investigate source of stuffed credentials.

Clean, fast, documented. The kind of trail that survives an audit and customer questions.

---

## 11.8 Go deeper

- 🏛 [NIST SP 800-61 Rev. 2](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) · [SP 800-61 Rev. 3 (draft)](https://csrc.nist.gov/pubs/sp/800/61/r3/ipd)
- 🏛 [NIST SP 800-34 Rev. 1: Contingency Planning](https://csrc.nist.gov/pubs/sp/800/34/r1/upd1/final)
- 🏛 [CISA IR Playbooks](https://www.cisa.gov/resources-tools/resources/federal-government-cybersecurity-incident-and-vulnerability-response-playbooks)
- 🏛 [CERT-In directions (India)](https://www.cert-in.org.in/)
- 🏛 [ISO 22301 overview](https://www.iso.org/standard/75106.html)
- 📰 [SANS: IR whitepapers](https://www.sans.org/white-papers/?focus-area=incident-response)
- 🎥 [Simply Cyber: SOC paths](https://www.youtube.com/@SimplyCyber)
- 🧪 [LetsDefend free tier](https://letsdefend.io/) · [Blue Team Labs Online](https://blueteamlabs.online/) · [TryHackMe SOC Level 1 path](https://tryhackme.com/)
- 🏛 [MITRE ATT&CK](https://attack.mitre.org/): map incidents to techniques.

## Module 11: Glossary recap

NIST IR lifecycle, Preparation, Detection & Analysis, Containment / Eradication / Recovery, Post-incident, SEV1–SEV5, IR commander, Chain of custody, Disk image, Memory dump, Playbook, Tabletop exercise, SOC, SIEM, SOAR, XDR, MDR, MTTD, MTTR, Dwell time, BCP, DR, Crisis Management, BIA, MTD, RTO, RPO, Hot/Warm/Cold site, 3-2-1 backup, Immutable backup, Double/triple extortion ransomware, OFAC, Regulator notification matrix, 72-hour clock, 6-hour CERT-In, SEC 8-K 1.05.

→ Next: [Module 12: Third-Party / Vendor Risk](12-vendor-risk.md)

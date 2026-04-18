# Frameworks Cross-Map — SOC 2 ↔ ISO 27001:2022 ↔ NIST CSF 2.0 ↔ NIST 800-53 Rev 5 ↔ PCI-DSS v4 ↔ HIPAA Security

> *The single most useful skill in GRC. Build controls once, map to many frameworks, present the same evidence to many auditors.*

**Last verified:** 17 April 2026. Source: [Module 7.7](../modules/07-grc-frameworks.md).

---

## How to read this map

Each row is a **control topic** — something you actually do. The columns are how each framework refers to that topic. When a customer questionnaire asks "do you have MFA?" you can answer once with confidence that you're covered for SOC 2 CC6.1, ISO 27001:2022 A.8.5, NIST CSF 2.0 PR.AA-03, NIST 800-53 IA-2(1), PCI-DSS Req 8.4, and HIPAA §164.312(a)(2)(i).

In your day-to-day work, keep this in a spreadsheet. When a new framework lands, add a column. When a control changes, update one cell, and it propagates to evidence across audits.

---

## The cross-map

| Topic | SOC 2 (TSC CC) | ISO 27001:2022 | NIST CSF 2.0 | NIST 800-53 Rev 5 | PCI-DSS v4 | HIPAA Security |
|---|---|---|---|---|---|---|
| Access control policy | CC6.1 | A.5.15 | PR.AA-01 | AC-1 | Req 7 | §164.308(a)(4) |
| Multi-factor authentication | CC6.1 | A.8.5 | PR.AA-03 | IA-2(1) | Req 8.4 | §164.312(a)(2)(i) |
| User access provisioning | CC6.2 | A.5.16, A.5.18 | PR.AA-01 | AC-2 | Req 8.2 | §164.308(a)(4)(ii)(B) |
| Privileged access management | CC6.1 | A.8.2 | PR.AA-05 | AC-6 | Req 7.2 | §164.308(a)(4) |
| Periodic access review | CC6.3 | A.5.18 | PR.AA-05 | AC-6(7) | Req 7.2.4 | §164.308(a)(4)(ii)(C) |
| Encryption at rest | CC6.1, CC6.7 | A.8.24 | PR.DS-01 | SC-28 | Req 3 | §164.312(a)(2)(iv) |
| Encryption in transit | CC6.7 | A.8.24 | PR.DS-02 | SC-8 | Req 4 | §164.312(e)(1) |
| Key management | CC6.1, CC6.7 | A.8.24 | PR.DS-01 | SC-12, SC-13 | Req 3.6, 3.7 | §164.312(a)(2)(iv) |
| Asset inventory | CC6.1 | A.5.9, A.5.10 | ID.AM-01, ID.AM-02 | CM-8 | Req 9.5, 12.5 | §164.310(d)(1) |
| Data classification | CC6.1 | A.5.12, A.5.13 | ID.AM-05 | RA-2 | Req 9.4 | §164.308(a)(7)(ii)(E) |
| Vulnerability management | CC7.1 | A.8.8 | ID.RA-01 | RA-5 | Req 11.3 | §164.308(a)(1)(ii)(B) |
| Patch management | CC7.1 | A.8.8 | PR.PS-02 | SI-2 | Req 6.3 | §164.308(a)(5)(ii)(B) |
| Logging | CC7.2 | A.8.15 | DE.CM-01 | AU-2 | Req 10.2 | §164.312(b) |
| Log monitoring & alerting | CC7.2 | A.8.16 | DE.CM-01, DE.AE-02 | AU-6 | Req 10.4 | §164.308(a)(1)(ii)(D) |
| Incident response policy | CC7.3 | A.5.24 | RS.MA-01 | IR-1 | Req 12.10.1 | §164.308(a)(6)(i) |
| Incident response plan / playbooks | CC7.4 | A.5.26 | RS.MA, RS.AN | IR-4 | Req 12.10 | §164.308(a)(6)(ii) |
| Incident reporting / notification | CC7.5 | A.5.25, A.5.26 | RS.CO-02 | IR-6 | Req 12.10.5 | §164.308(a)(6) |
| Business continuity | CC9.1 | A.5.29 | RC.RP-01 | CP-2 | Req 12.10.1 | §164.308(a)(7)(i) |
| Disaster recovery | CC9.1 | A.5.30 | RC.RP-01 | CP-9, CP-10 | Req 12.10 | §164.308(a)(7)(ii)(B) |
| Backup | CC9.1 | A.8.13 | PR.DS-11 | CP-9 | Req 9.5.1, 12.10 | §164.308(a)(7)(ii)(A) |
| Vendor / third-party risk | CC9.2 | A.5.19, A.5.20, A.5.21, A.5.22 | GV.SC | SR-3 | Req 12.8 | §164.308(b) |
| Risk assessment | CC3.1–3.4 | Clause 6.1, A.5.4 | ID.RA, GV.RM | RA-3 | Req 12.3 | §164.308(a)(1)(ii)(A) |
| Secure software development | CC8.1 | A.8.25, A.8.28 | PR.PS-06 | SA-11, SA-15 | Req 6.2 | n/a |
| Change management | CC8.1 | A.8.32 | PR.PS-01 | CM-3 | Req 6.5 | §164.308(a)(5)(ii)(C) |
| Network security | CC6.6 | A.8.20, A.8.21, A.8.22 | PR.IR-01 | SC-7 | Req 1 | §164.312(e)(1) |
| Physical security | CC6.4 | A.7.1–A.7.14 | PR.AA-06 | PE-3 | Req 9.1, 9.2, 9.3 | §164.310 |
| Awareness & training | CC1.4, CC2.2 | A.6.3 | PR.AT-01 | AT-2 | Req 12.6 | §164.308(a)(5)(i) |
| HR security (joiner/mover/leaver) | CC1.4 | A.6.1, A.6.2, A.6.5 | PR.AA-01, GV.RR-04 | PS-3, PS-4, PS-5 | Req 12.7 | §164.308(a)(3) |
| Cryptography policy | CC6.1 | A.8.24 | PR.DS-01 | SC-13 | Req 3.6 | §164.312(e)(2)(ii) |
| Data leakage prevention | CC6.7 | A.8.12 | PR.DS-05 | SC-7(10) | Req 3 | n/a |
| Threat intelligence | CC7.2 | A.5.7 | ID.RA-02 | PM-16 | Req 6.3.1 | n/a |
| Cloud services use | CC6.1 | A.5.23 | GV.SC | SA-9 | n/a | n/a |
| Capacity / availability | A1.1 | A.8.6 | PR.IR-04 | CP-2(2) | n/a | §164.308(a)(7)(i) |
| Privacy notice / data subject rights | P series | A.5.34 | GV.OV | PT-1, IP-1 | n/a | §164.520 |

---

## How to use this in real life

### When a customer sends you a SIG / CAIQ / DDQ

Per [Module 17 Scenario 4](../modules/17-practice-scenarios.md): the average enterprise DDQ has 200–400 questions. Most map to the topics above. If you can confidently say "we satisfy this via control X, evidenced by Y, mapped to SOC 2 CC6.1 and ISO A.8.5," you collapse 50+ questions down to a handful of unique answers. That is the difference between a 2-week response and a 2-day response.

### When you're designing a new control

Don't design for a single framework. Design for the topic, then map. A well-designed access-review process passes SOC 2 (CC6.3), ISO 27001 (A.5.18), NIST CSF (PR.AA-05), 800-53 (AC-6(7)), PCI-DSS (Req 7.2.4), and HIPAA (§164.308(a)(4)(ii)(C)) at the same time, with the same evidence.

### When a new framework version lands

Add a column. Walk every row. Update cells that change. Most won't. The 2022 update of ISO 27001 (114 → 93 controls) was painful for organisations that hadn't built a cross-map; trivial for the ones that had.

---

## Authoritative cross-mapping sources

Don't rebuild what NIST already publishes:

- 🏛 [NIST CSF Informative References (cross-walks to 800-53, ISO 27001, COBIT)](https://www.nist.gov/cyberframework/informative-references)
- 🏛 [Secure Controls Framework — free common control set with cross-mappings](https://securecontrolsframework.com/)
- 🏛 [CIS Controls v8 → CSF / ISO / PCI mapping (Center for Internet Security)](https://www.cisecurity.org/controls)
- 🏛 [HITRUST CSF — unified mapping for healthcare](https://hitrustalliance.net/)

---

## A warning

This map covers **what control topic** maps to **what reference**. It does not cover **how good your implementation is**. A poorly implemented MFA rollout (SMS-only, no admin coverage) still maps to CC6.1, A.8.5, PR.AA-03 — and still fails the audit. The framework reference is your starting point, not your evidence.

> *Build controls once. Map to many. Defend the implementation, not the citation.*

# Security, Privacy and GRC, A Practical Roadmap

> *A free, opinionated, India-aware curriculum for getting into security, privacy, or GRC. Built around the questions hiring managers actually ask, and the artefacts auditors actually open.*

**Last full reference check: 17 April 2026**

---

## Who this is for

Three kinds of people pick up this material. The route is different for each. Pick the wrong one and you will waste weeks on modules that are not your problem.

| You are | You will spend most of your time on | Read first |
|---|---|---|
| 🟢 **A non-tech beginner** working out whether security, privacy or GRC is for you | The foundation pack and the GRC core (Modules 0, 1, 3, 5–10) | [paths/00-non-tech-beginner.md](paths/00-non-tech-beginner.md) |
| 🟡 **A career-shifter** from law, finance, audit, ops, or project management | The full curriculum, with extra weight on Modules 5 to 12 | [paths/01-career-shifter.md](paths/01-career-shifter.md) |
| 🔴 **A tech professional** moving from dev / SRE / cloud into security or GRC | A fast pass through the basics, then deep on 7–15 | [paths/02-tech-professional.md](paths/02-tech-professional.md) |

If you are not sure which one fits, read [START-HERE.md](START-HERE.md) first. Five minutes, and you will know.

---

## What you will produce

By the time you finish, your public GitHub will hold real artefacts. Not certificates of attendance:

- A populated **risk register** (owners, scores, treatment plans, review dates)
- A filled **DPIA** (Data Protection Impact Assessment) for a realistic scenario
- A **Statement of Applicability** showing which controls you implemented and why
- An **incident response runbook** for at least one playbook (ransomware, data exposure, vendor outage, your pick)
- A **vendor risk assessment** with tiering and a real questionnaire response
- A **policy set** mapped to ISO 27001:2022 and SOC 2
- An **audit evidence pack** that an auditor could actually open

These are the things hiring managers ask to see. Not your cert count. In my experience, one candidate who can talk through their own risk register beats three who only have CC and Security+ on the wall.

---

## Coverage scope (with versions, because versions matter)

Current to April 2026. Every framework and regulation here is cited with the version number, because that is what gets you through the interview. If a YouTube playlist or blog post does not mention versions, it is probably out of date.

**Privacy and data protection laws:** GDPR (EU), UK GDPR, **DPDP Act 2023 + DPDP Rules 2025 (India)**, CCPA/CPRA (California), HIPAA (US healthcare), COPPA, FERPA, PCI-DSS 4.0.1.

**GRC frameworks and standards:** SOC 2 (TSC 2017, revised 2022), **ISO/IEC 27001:2022**, ISO/IEC 27701:2019, ISO/IEC 27017, ISO/IEC 27018, ISO/IEC 22301:2019, ISO/IEC 42001:2023 (AI management), **NIST CSF 2.0**, NIST SP 800-53 Rev 5, NIST SP 800-171 Rev 3, NIST AI RMF 1.0 + Generative AI Profile.

**Technical and operational:** OWASP Top 10 (Web 2021, API 2023, **LLM 2025**), MITRE ATT&CK, **MITRE ATLAS** (AI/ML threats), CIS Controls v8.1, **CISA Zero Trust Maturity Model 2.0**, FIPS 140-3, **FIPS 203/204/205** (post-quantum cryptography).

**Regulatory landmarks:** **EU AI Act** (in force August 2024, GPAI obligations August 2025), **SEC Cybersecurity Disclosure Rule** (Item 1.05, 4-business-day clock), **CERT-In Directions 2022** (6-hour incident reporting in India), **RBI Cybersecurity Framework**, **SEBI CSCRF**.

---

## What this is *not*

To save your time:

- Not a cert dump. Certifications open doors. The job is still about chasing evidence.
- Not a tool tutorial. Tools change every 18 months. Concepts do not.
- Not a CTF. Capture-the-flag is fun, and unrelated to most security and GRC work.
- Not US-only. India, EU, and UK regulatory contexts are first-class throughout.
- Not affiliate-link bait. There is nothing to sell you.
- Not chatbot output. Written by someone who has sat through audits and argued with engineering on a Friday evening.

---

## Free-first commitment

Every required resource here is free. Where a paid resource is genuinely worth the money (a specific certification, a specific book with no online equivalent), it is flagged 💰 and clearly marked optional. Most of the time you will not need it.

The full curated free-resource library is in [reference/free-resources.md](reference/free-resources.md).

---

## How the repo is organised

```
.
├── README.md                          (you are here)
├── START-HERE.md                      ← 5-minute orientation if you are new
│
├── paths/                             ← persona-specific reading guides
│   ├── 00-non-tech-beginner.md
│   ├── 01-career-shifter.md
│   └── 02-tech-professional.md
│
├── modules/                           ← the 18 teaching modules, in order
│   ├── 00-foundation-pack.md
│   ├── 01-security-first-principles.md
│   ├── … (02 through 16)
│   └── 17-practice-scenarios.md
│
├── reference/                         ← standalone references, used across modules
│   ├── glossary.md                    ← every term defined, alphabetised
│   ├── acronyms.md                    ← the acronym zoo, decoded
│   ├── frameworks-cross-map.md        ← SOC 2 ↔ ISO 27001 ↔ NIST CSF ↔ PCI ↔ HIPAA
│   ├── regulator-clocks.md            ← every notification deadline in one table
│   └── free-resources.md              ← curated free-resource library
│
├── scenarios/                         ← 10 capstone scenarios with rubrics
│   ├── README.md
│   └── templates/                     ← blank starters for portfolio artefacts
│
├── interview-prep/
│   ├── question-bank.md               ← every interview trap from every module
│   ├── 30-second-drills.md            ← rapid-fire concept drills
│   ├── star-and-c4r-frameworks.md     ← how to structure interview answers
│   └── india-market-snapshot.md       ← INR salary bands, key employers
│
├── faq.md                             ← the 20 questions everyone asks
├── CONTRIBUTING.md                    ← style guide and PR checklist
├── CODE_OF_CONDUCT.md
├── CHANGELOG.md
└── LICENSE                            ← CC BY-SA 4.0 (content), MIT (templates)
```

---

## Maintenance promise

References are re-verified every quarter. The last full check was **17 April 2026** and the date is on every reference list. If you find a stale link, an outdated regulation, or a framework that has shipped a new version, open an issue. It gets fixed.

---

## A note on ethics

You will learn things here that can harm people if misused. Threat models, attack patterns, data extraction techniques, social engineering vectors. Do not.

The work of security and GRC is fundamentally about reducing harm to other humans. Learn it for that. Practise it for that. If you find yourself drifting away from that, walk away.

---

## Licensing

- **Content** (modules, references, scenarios): Creative Commons Attribution-ShareAlike 4.0 (CC BY-SA 4.0). Use it, adapt it, build on it. Credit the source. Share-alike means your derivative works carry the same licence.
- **Templates and any code** (in `scenarios/templates/` and `.github/`): MIT.

See [LICENSE](LICENSE) for the full text.

---

## Get in touch

- Open an [issue](../../issues) for content errors, broken links, or topic requests
- Pull requests welcome (read [CONTRIBUTING.md](CONTRIBUTING.md) first for the voice / tone rules)
- Author: **Sathya Sivam** ([LinkedIn](https://www.linkedin.com/in/sathya-sivam))

---

> *Compliance is the floor, not the ceiling. Passing an audit means you met the minimum someone else wrote down. It does not mean you are safe.*

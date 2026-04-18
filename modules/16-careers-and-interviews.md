# Module 16 — Careers, Certifications & Interview Prep

> **Audience:** 🟢🟡🔴 — tailored paths for each · **Time:** ~60 min · **Prereqs:** Modules 0–12 at minimum

## Why this matters

Security/privacy/GRC has the most confusing role taxonomy of any tech field. "Security Analyst" can mean three completely different jobs. This module maps roles, certs, salaries, and how to actually land the first offer.

---

## 16.1 The role map

### By function

| Family | Roles | Typical day |
|--------|-------|------------|
| **GRC / Compliance** | GRC Analyst, Compliance Manager, Audit Lead, ISMS Manager, Privacy Analyst, DPO, Risk Analyst | Policies, audits, evidence, vendor reviews, risk register |
| **Security Operations** | SOC Analyst (L1/L2/L3), Threat Hunter, DFIR, Incident Responder, Detection Engineer | Alerts, investigations, hunts, IR |
| **Offensive** | Pen-tester, Red-teamer, Bug bounty hunter, Application security researcher | Find & exploit |
| **Defensive Engineering** | Security Engineer, Cloud Security Eng, AppSec Eng, DevSecOps, Detection Engineer, IAM Engineer | Build controls, pipelines, detections |
| **Architecture / Strategy** | Security Architect, Principal Engineer, Field CISO, CISO | Design, standards, board |
| **Privacy** | Privacy Analyst, Privacy Engineer, Privacy Counsel, DPO | DSRs, DPIAs, data mapping, privacy-by-design |
| **Specialist** | Forensics, Cryptography Eng, OT/ICS, Cloud Compliance, AI Security, Trust & Safety | Depends |

### Entry points by background

- 🟢 **No tech background** — GRC Analyst, Privacy Analyst, IT Audit (Big 4), Risk Analyst, Compliance Associate, TPRM Analyst, Security Awareness/Training, Trust & Safety Analyst.
- 🟡 **Career-switcher (support/ops/PM/BA/HR/legal/finance/IT Ops)** — GRC, TPRM, IAM Ops, SOC L1, Security Awareness, IT Auditor, Compliance, Product Security PM.
- 🔴 **Tech (SDE/SRE/DBA/cloud/QA)** — AppSec, Cloud Security, DevSecOps, Detection Engineering, SOC L2/L3, Security Engineer, Privacy Engineer.

**Counterintuitive truth:** GRC pays well and scales to CISO. It isn't "lesser" than hacking roles. Many CISOs came through audit/risk, not offensive.

---

## 16.2 Certifications — the honest decision tree

Order of cost-effectiveness (free/cheap first, expensive later):

### Starter / free

- **ISC2 Certified in Cybersecurity (CC)** — free via ISC2's "One Million Certified in Cybersecurity" initiative. Good first credential.
- **Google Cybersecurity / Google IT Support Certificate (Coursera)** — audit free, cheap if you want cert.
- **AWS Cloud Practitioner / Azure AZ-900 / GCP Cloud Digital Leader** — useful even for GRC folks.

### Foundational

- **CompTIA Security+** — broad baseline; widely recognised entry bar.
- **CompTIA Network+** (if weak on networking).

### GRC / audit / privacy path

- **ISO/IEC 27001 Lead Implementer** (PECB/BSI) — hands-on ISMS building.
- **ISO/IEC 27001 Lead Auditor** — to do audits; chargeable mostly by consulting firms.
- **CISA** (ISACA) — the IT audit gold standard. Needed at Big 4.
- **CRISC** (ISACA) — risk-specific; good after 2–3 years.
- **CISM** (ISACA) — management-side; pairs with CISA.
- **CISSP** (ISC2) — broad management cert; needs 5 years' experience (endorsement waiver with degree).
- **Privacy:** **CIPP/E** (GDPR), **CIPP/US**, **CIPM** (operational), **CDPSE** (ISACA, technical-privacy) — IAPP is the privacy body.

### Technical path

- **SSCP** (ISC2) — hands-on security admin.
- **AWS Security Specialty / Azure SC-100/200/300/400 / Google Professional Cloud Security Engineer.**
- **GCIH, GSEC, GCIA, GPEN, GREM** (SANS/GIAC) — expensive but heavy-weight.
- **OSCP** (Offensive Security) — pen-test rite of passage.
- **CCSP** (ISC2) — cloud, management-leaning.

### Decision rules

1. **Don't stack certs before a job.** 2 relevant certs + projects > 5 certs + nothing to show.
2. Match cert to role: GRC → Sec+ + ISO LI/LA + CISA. AppSec → Sec+ + cloud specialty + OSCP. Privacy → CIPP/E + CDPSE.
3. In India, **ISO 27001 Lead Implementer + CISA** is the most employable GRC combo at Big 4, GCCs, and SaaS companies.
4. CISSP is a ceiling-raiser after 5+ years, not a starter cert.
5. Vendor certs age fast; fundamentals (Sec+, CISA, CIPP/E) age slowly.

---

## 16.3 Portfolio — what actually gets you hired

Recruiters skim for ~10 seconds. Give them evidence.

### For 🟢🟡 (GRC / Privacy)

- **Sample ISMS artefacts** on GitHub or Notion: an information security policy (your own writing, not templates), a risk register with 10 rows, a SoA covering 20 controls, a DPIA for a hypothetical product.
- **Mock vendor assessment** — you, playing security reviewer of a sample vendor, producing a 2-page memo with findings.
- **Policy-writing samples** — access control, IR, data retention — 1 page each.
- **A "mapped controls" sheet** — SOC 2 ↔ ISO 27001 for one domain.
- **A DSR response workflow diagram.**
- **LinkedIn write-ups** (5–10 posts) on real regulatory events (CERT-In directive, new DPDP rules, a public breach teardown).

### For 🔴 (Technical)

- **GitHub with real commits** — IaC modules, scanning pipelines, custom Semgrep rules, a Terraform module for secure S3.
- **Write-ups** — TryHackMe / HackTheBox / flaws.cloud walk-throughs.
- **A small open-source contribution** to a security tool.
- **CTF score** (picoCTF, CSAW, HTB rank).
- **Blog posts** explaining one concept deeply.

### For everyone

A **LinkedIn headline** that states your specialisation, not "aspiring cybersecurity enthusiast." A **portfolio link** in the banner. A **short pinned post** summarising your best artefact.

---

## 16.4 Resume template

```
NAME · City, Country · email · linkedin · portfolio-link
────────────────────────────────────────────────────────────
SUMMARY (3 lines)
  One-line identity + core tech stack / frameworks + what you want next.

EXPERIENCE (most recent first; metric-driven bullets)
  Role · Company · Dates
    • Action verb + scope + outcome with a number.
    • e.g., "Led SOC 2 Type 1 readiness across 42 controls; passed with zero
             exceptions in 4 months, saving est. ₹18L in consulting fees."

PROJECTS (portfolio)
  Project name — 1-line outcome · tools · link
    • 2 bullets: technical depth + impact.

EDUCATION · CERTIFICATIONS · SKILLS
```

Rules:

- Page 1 for 0–8 years. Page 2 only with heavy research/publication record.
- Quantify everything possible (controls, days saved, risk reduction, # users).
- No "responsible for." Use "Led / built / shipped / reduced / prevented."
- No photo, no age, no marital status (unless a local convention requires; generally avoid).
- Tailor per role: put keywords the JD uses so ATS sees them.

---

## 16.5 Interview preparation — the scenario-first method

Memorising definitions fails. Top companies ask **scenario questions**. Prepare by practising *frameworks of answer*, not facts.

### Framework 1 — STAR for behavioural

**S**ituation · **T**ask · **A**ction · **R**esult. 2 minutes max.

### Framework 2 — C4R for technical scenarios

**C**larify (ask 2 smart questions) · **C**onstrain (what's in/out of scope) · **C**onsider (2–3 options with trade-offs) · **C**hoose (your recommendation, why) · **R**isks/next-steps.

### Framework 3 — DREAD/CIA for risk/threat questions

Name the model you're using; walk through it.

### High-frequency questions (role-bucket)

**GRC / Audit:**

- Walk me through a SOC 2 audit from kickoff to report.
- Control fails during audit — what do you do?
- Evidence collection for a control tested quarterly — sample size and why.
- Difference between exception, observation, NC, OFI.
- Explain your approach to building a risk register from zero.
- How do you handle a vendor who refuses to share SOC 2?
- Give an example of a control you designed that failed — and what you learned.

**Privacy:**

- DSR arrives for account deleted 2 years ago — walk through.
- Marketing team wants to launch a new analytics tool that tracks users across sites — your stance.
- Explain lawful basis choice for an EdTech signup flow.
- A vendor in the US processes EU data — what do you need?
- DPIA for a new AI feature — what sections do you cover?

**AppSec / Tech:**

- You find a vulnerable library in 200 repos — plan to remediate.
- Explain SAML vs OIDC to a PM.
- Design a secure CI/CD pipeline.
- A dev says "we'll fix it next sprint" about a critical CVE — how do you respond.
- Review this code snippet — what's wrong? (Expect an IDOR, injection, or crypto misuse.)

**SOC / IR:**

- Walk me through a suspected credential-stuffing incident from alert to closure.
- Explain the NIST IR lifecycle.
- Priority-triage these 5 alerts (interviewer gives list).
- Difference between containment strategies.
- Lessons-learned example from a real incident you handled (or a public one).

### Curveballs to prepare

- *"What would you do in your first 90 days?"* — have a specific plan.
- *"Tell me about a time you disagreed with engineering / a customer / a senior."*
- *"What's a security opinion you hold that most people disagree with?"*
- *"Teach me [concept] as if I'm a 12-year-old."*

---

## 16.6 The India market — a frank snapshot (as of 2025-2026)

- **Big 4 (Deloitte, PwC, EY, KPMG)** — largest GRC/audit recruiter. Start as Analyst/Associate; bias for CA/MBA/CISA. Learn broad exposure, churn is real.
- **Product SaaS (Zoho, Freshworks, Postman, Razorpay, Atlassian India, etc.)** — lean GRC teams; hire generalists who code-adjacent.
- **BFSI** — banks, NBFCs, insurers. RBI/IRDAI/SEBI regulated, so demand heavy on audits, vendor risk, ITGCs. Good pay, slower tech pace.
- **Global Capability Centres (GCCs / in-house centres for MNCs)** — often best comp for tech security roles (AppSec, cloud).
- **Consulting boutiques (DSCI-empanelled, ISO firms)** — fast exposure to multiple clients; good for certs (LI/LA) experience.
- **Startups** — 1st security hire wears every hat: opportunity and risk.

Salary bands (2025, broad, INR LPA, be skeptical of any single number — use Levels.fyi, AmbitionBox, glassdoor triangulation):

- GRC Analyst (0–2y): 6–12
- GRC Manager (4–8y): 18–35
- SOC L2 (2–4y): 10–20
- AppSec / Cloud Sec Engineer (3–6y): 25–55
- Security Architect (8–12y): 45–90
- CISO (BFSI / tech unicorn): 80 – 3Cr+

Remote-first for global roles frequently offers 1.8–3× India-local bands.

---

## 16.7 The first-90-days-on-the-job plan

**Days 1–30 — Listen & inventory.** Shadow the team. Read every policy, SoA, risk register, last audit report, IR runbooks. Map stakeholders. Don't propose changes.

**Days 31–60 — Find the 3 gaps.** Where is evidence inconsistent? Which control owners are overloaded? Which regulator/customer is next pressing? Pick **three** gaps to fix.

**Days 61–90 — Ship one visible win.** A cleaned-up access-review process; a vendor inventory reconciled; an IR runbook tested. Present to your manager with data.

Promotions come from measurable outcomes, not hours worked.

---

## 16.8 Go deeper

- 🏛 [NICE Framework (NIST) — roles & competencies](https://niccs.cisa.gov/workforce-development/nice-framework)
- 🏛 [(ISC)² Certified in Cybersecurity — free](https://www.isc2.org/certifications/cc)
- 🏛 [ISACA certs (CISA, CRISC, CISM, CDPSE)](https://www.isaca.org/credentialing)
- 🏛 [IAPP (CIPP/E, CIPM, CIPT)](https://iapp.org/certify/)
- 📘 [Google Cybersecurity Certificate (Coursera — audit free)](https://www.coursera.org/professional-certificates/google-cybersecurity)
- 📘 [SANS Cyber Aces (free)](https://www.cyberaces.org/)
- 📰 [Daniel Miessler's "How to Become a Cybersecurity Expert"](https://danielmiessler.com/)
- 📰 [TLDRSec newsletter — weekly AppSec/cloud digest](https://tldrsec.com/)
- 🧪 [TryHackMe free paths (Pre-Security, Introduction to Cyber Security)](https://tryhackme.com/) · [picoCTF](https://picoctf.org/)
- 💰 Paid but worth it — SANS (rare, expensive; try work sponsorship) · Offensive Security OSCP.

## Module 16 — Glossary recap

GRC Analyst, SOC Analyst, DFIR, Red team, AppSec, DevSecOps, Security Architect, CISO, DPO, TPRM Analyst, CC (ISC2), Security+, CISA, CRISC, CISM, CISSP, CIPP/E, CIPM, CDPSE, CCSP, OSCP, SSCP, NICE Framework, STAR, C4R, SoA (artefact), 90-day plan, GCC, DSCI, Levels.fyi, ATS.

→ Next: [Module 17 — Practice Scenarios & Portfolio Capstones](17-practice-scenarios.md)

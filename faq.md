# FAQ — The 20 Questions Everyone Asks

> *Honest answers, not marketing answers.*

---

### 1. Do I need a CS degree to break into security, privacy, or GRC?

No. GRC, privacy, audit, TPRM, and trust & safety routinely hire people with no CS degree. Law, finance, audit, customer success, journalism, social sciences, ops — all valid origin stories. For deeply technical roles (AppSec, cloud security, detection engineering), you don't need a CS degree but you do need real systems fluency.

---

### 2. Do I need to learn to code?

For 🟢 GRC/Privacy/Audit/TPRM: not really. You should be comfortable reading a JSON file and running basic terminal commands. That's it.

For 🟡 GRC engineering, privacy engineering, security automation: enough Python to read scripts, modify them, and write small tools. A weekend course is sufficient.

For 🔴 AppSec, cloud security, DevSecOps, detection engineering: yes — at the level of a working software engineer. You should be able to read code in 2–3 languages and write infrastructure-as-code.

---

### 3. Which certification should I get first?

Free **ISC2 Certified in Cybersecurity (CC)** for everyone. After that:

- **GRC / Privacy / Audit:** Security+ → ISO 27001 Lead Implementer → CISA → CIPP/E.
- **Technical:** Security+ → cloud specialty (AWS Security or Azure SC-200) → optionally OSCP or CCSP.

The honest rule from [Module 16.2](modules/16-careers-and-interviews.md): **2 relevant certs + portfolio > 5 certs + nothing to show.**

---

### 4. Is GRC "lesser" than offensive security or hacking?

No. GRC pays well, scales to CISO, and many CISOs came up through audit and risk rather than offensive paths. The "GRC is boring" narrative is wrong; it's the function that decides what gets funded, what risks get accepted, and what the board hears. That is power, not paperwork.

---

### 5. How long until I can get my first job?

If you start with no background and put in 8–10 hours a week:

- 🟢 Non-tech beginner: **3–5 months** to first interview-ready, **6–9 months** to first offer.
- 🟡 Career-shifter: **4–6 months** to first interview-ready, **6–10 months** to first offer.
- 🔴 Tech professional moving sideways: **2–4 months** to interview-ready in a security-engineering role.

People who do the capstone scenarios get offers faster than people who only watch videos. This is the single strongest correlation in this field.

---

### 6. Do I need to do CTFs / TryHackMe / HackTheBox?

For technical roles: useful, not required. For GRC/privacy: not really. Don't let CTF rank gatekeep you.

---

### 7. Should I get a Master's in cybersecurity?

Usually no. Most are expensive and behind the curve on what hiring managers want. Exceptions: if your country/employer subsidises it heavily, or if you want to shift into research/academia/government. Otherwise, certs + portfolio + work experience win.

---

### 8. SOC 2 vs ISO 27001 — which should my company do first?

If your buyers are mostly US-based: **SOC 2 Type II first**, ISO 27001 second.
If your buyers are mostly EU / India / APAC / ME: **ISO 27001 first**, SOC 2 second.
If you sell globally and have the budget: do both, since 70%+ of controls overlap. See [reference/frameworks-cross-map.md](reference/frameworks-cross-map.md).

---

### 9. Is GDPR really enforced or just talked about?

Really enforced. Notable fines: Meta €1.2B (2023, transfers), Amazon €746M (2021), WhatsApp €225M (2021, transparency), LinkedIn €310M (2024, behavioural ads). Treat the 72-hour breach clock as real, because it is.

---

### 10. Does the DPDP Act 2023 actually matter yet?

Yes. The Act is notified; the DPDP Rules 2025 are operationalising it; the Data Protection Board of India (DPBI) is being stood up. Penalties go up to **₹250 crore** per instance for failure to protect data. Add the **CERT-In 6-hour breach reporting clock** on top, and the operational reality in India is already strict. See [Module 6](modules/06-privacy-laws.md) §6.3.

---

### 11. Can AI tools replace security/privacy/GRC analysts?

They are absolutely changing the work. Vendor questionnaire response, evidence collection, and policy drafting are all being automated or semi-automated. What is not being replaced any time soon: judgement on ambiguous regulatory questions, stakeholder negotiation, novel-incident leadership, defending design decisions in front of auditors and regulators. The analysts who learn to use the tools well will be more valuable, not less.

---

### 12. I'm in India. Do US-focused resources still apply?

Most concepts: yes. Specific laws and regulators: no. This curriculum treats India (DPDP, CERT-In, RBI, SEBI, IRDAI) as first-class throughout, alongside EU and US. You do not have to translate.

---

### 13. What salary should I expect in India in 2026?

From [Module 16.6](modules/16-careers-and-interviews.md), broad bands in INR LPA:

- GRC Analyst (0–2y): 6–12
- GRC Manager (4–8y): 18–35
- SOC L2 (2–4y): 10–20
- AppSec / Cloud Security Engineer (3–6y): 25–55
- Security Architect (8–12y): 45–90
- CISO (BFSI / unicorn): 80 to 3 Cr+

Remote-first global roles often offer 1.8–3× India-local. See [interview-prep/india-market-snapshot.md](interview-prep/india-market-snapshot.md) for employer types and what they actually pay.

---

### 14. Which cloud should I learn — AWS, Azure, or GCP?

Pick the one your target employer uses. If you don't know: AWS has the largest market share globally; Azure dominates enterprise + Microsoft shops; GCP is strong in data and AI. Learn one deeply before adding a second. A cloud-agnostic candidate often beats a multi-cloud-shallow one.

---

### 15. Is "compliance" the same as "security"?

No. From [Module 0.1](modules/00-foundation-pack.md): security is about *protecting*; compliance is about *proving* you meet a rulebook. You can be compliant and unsafe (passing an audit with paper controls), or safe and non-compliant (having strong controls but no documentation). The goal is both.

> *Compliance is the floor, not the ceiling.*

---

### 16. Do I need to read the full text of GDPR / ISO 27001 / NIST 800-53?

No. Read summaries first. Read the actual text only for the articles or controls you're working on. ICO guidance, EDPB guidelines, and AICPA TSC documents are more readable than the standards themselves. See [reference/free-resources.md](reference/free-resources.md).

---

### 17. How do I build a portfolio without a job?

Use [Module 17 — Practice Scenarios](modules/17-practice-scenarios.md). Pick 3 scenarios. Do the deliverables properly. Put them in a public (sanitised) GitHub repo. Write a short README per scenario explaining your approach. Link from the "Featured" section of LinkedIn.

Three completed capstones beat five certifications.

---

### 18. Should I pay for a bootcamp?

Almost never. The best resources in this field — Cloudflare Learning, NIST publications, ICO guidance, Google Cybersecurity Certificate (audit free), Professor Messer, SANS Cyber Aces, TryHackMe free paths, Daniel Miessler's writing — are free. See [reference/free-resources.md](reference/free-resources.md). Spend money only on certifications you actually need for a specific role.

---

### 19. How do I stand out in interviews?

Be the candidate who can map a control to specific framework criteria ("this maps to SOC 2 CC6.1, ISO 27001:2022 A.8.5, and NIST CSF 2.0 PR.AA-03"), and who has 2 capstone scenarios ready to *discuss* — not recite. Use the **C4R framework** (Clarify, Constrain, Consider, Choose, Risks/next-steps) for technical scenarios. See [interview-prep/star-and-c4r-frameworks.md](interview-prep/star-and-c4r-frameworks.md).

---

### 20. What's the single most useful skill I can build?

**Framework cross-mapping.** The ability to take one control and articulate how it satisfies SOC 2, ISO 27001, NIST CSF, PCI-DSS, and HIPAA simultaneously. This is the skill that lets mature organisations build controls once and pass many audits, and it's the skill that makes you immediately credible in any GRC interview. See [reference/frameworks-cross-map.md](reference/frameworks-cross-map.md) and [Module 7.7](modules/07-grc-frameworks.md).

---

> *If you have a question that isn't here, open an issue. The list grows from real questions.*

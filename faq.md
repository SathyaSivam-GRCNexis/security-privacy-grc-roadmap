# FAQ, The 20 Questions Everyone Asks

> *Honest answers, not marketing answers.*

---

### 1. Do I need a CS degree to break into security, privacy, or GRC?

No. GRC, privacy, audit, TPRM, and trust & safety routinely hire people with no CS degree. Law, finance, audit, customer success, journalism, social sciences, ops, all valid origin stories. For the deeply technical roles (AppSec, cloud security, detection engineering), you still do not need a CS degree, but you do need real systems fluency. A degree helps you get past HR filters at large enterprises. It does not help you do the work.

---

### 2. Do I need to learn to code?

For 🟢 GRC, Privacy, Audit, TPRM: not really. You should be comfortable reading a JSON file and running basic terminal commands. That is it. If you can write a clean Excel formula and understand what a webhook is, you are fine.

For 🟡 GRC engineering, privacy engineering, security automation: enough Python to read scripts, modify them, and write small tools. A weekend course is usually sufficient.

For 🔴 AppSec, cloud security, DevSecOps, detection engineering: yes, at the level of a working software engineer. You should be able to read code in two or three languages and write infrastructure-as-code.

---

### 3. Which certification should I get first?

Free **ISC2 Certified in Cybersecurity (CC)** for everyone. After that:

- **GRC / Privacy / Audit:** Security+ → ISO 27001 Lead Implementer → CISA → CIPP/E.
- **Technical:** Security+ → cloud specialty (AWS Security or Azure SC-200) → optionally OSCP or CCSP.

The honest rule from [Module 16.2](modules/16-careers-and-interviews.md): **two relevant certs plus a portfolio beat five certs and nothing to show.** I have rejected candidates with CISSP because they could not walk me through a risk register. Certifications open doors. The job is still about chasing evidence.

---

### 4. Is GRC "lesser" than offensive security or hacking?

No. GRC pays well, scales to CISO, and many CISOs came up through audit and risk rather than red-team paths. The "GRC is boring" narrative is wrong. GRC is the function that decides what gets funded, what risks get accepted, and what the board hears. That is power, not paperwork.

That said, GRC has its own grind. Most of the time you are chasing engineering for evidence that should have existed last quarter, sitting in vendor questionnaire hell, and translating audit findings into language a product manager will read. Go in with eyes open.

---

### 5. How long until I can get my first job?

If you start with no background and put in 8 to 10 hours a week:

- 🟢 Non-tech beginner: **3 to 5 months** to interview-ready, **6 to 9 months** to first offer.
- 🟡 Career-shifter: **4 to 6 months** to interview-ready, **6 to 10 months** to first offer.
- 🔴 Tech professional moving sideways: **2 to 4 months** to interview-ready in a security-engineering role.

The single strongest correlation I have seen: people who do the capstone scenarios get offers faster than people who only watch videos. By a lot.

---

### 6. Do I need to do CTFs / TryHackMe / HackTheBox?

For technical roles: useful, not required. For GRC and privacy: not really. Do not let CTF rank gatekeep you. I have hired excellent GRC analysts who could not solve a single TryHackMe room.

---

### 7. Should I get a Master's in cybersecurity?

Usually no. Most are expensive and behind the curve on what hiring managers want. Exceptions: if your country or employer subsidises it heavily, or if you want to shift into research, academia, or government. Otherwise certs plus portfolio plus work experience win every time.

---

### 8. SOC 2 vs ISO 27001, which should my company do first?

If your buyers are mostly US-based: **SOC 2 Type II first**, ISO 27001 second.
If your buyers are mostly EU, India, APAC, or ME: **ISO 27001 first**, SOC 2 second.
If you sell globally and have the budget: do both. Around 70 per cent of controls overlap, so the second one is much cheaper than the first. See [reference/frameworks-cross-map.md](reference/frameworks-cross-map.md).

Opinion: most SMBs do not need a GRC platform in year one. A shared drive, a spreadsheet risk register, and a disciplined evidence folder gets you through your first SOC 2. Buy the platform when you have outgrown the spreadsheet, not before.

---

### 9. Is GDPR really enforced or just talked about?

Really enforced. Notable fines: Meta €1.2B (2023, transfers), Amazon €746M (2021), WhatsApp €225M (2021, transparency), LinkedIn €310M (2024, behavioural ads). Treat the 72-hour breach clock as real, because it is.

---

### 10. Does the DPDP Act 2023 actually matter yet?

Yes. The Act is notified, the DPDP Rules 2025 are operationalising it, the Data Protection Board of India (DPBI) is being stood up. Penalties go up to **₹250 crore** per instance for failure to protect data. Add the **CERT-In 6-hour breach reporting clock** on top, and the operational reality in India is already stricter than most people realise. If you sit on a breach in India for 12 hours debating wording, you have already missed the clock. See [Module 6](modules/06-privacy-laws.md) §6.3.

---

### 11. Can AI tools replace security, privacy, or GRC analysts?

They are absolutely changing the work. Vendor questionnaire response, evidence collection, and policy drafting are all being automated or part-automated. What is not being replaced any time soon: judgement on ambiguous regulatory questions, stakeholder negotiation, novel-incident leadership, and defending design decisions in front of auditors and regulators. The analysts who learn to use the tools well will be more valuable, not less. The ones who paste raw model output into a SoA will be found out at the first audit.

---

### 12. I am in India. Do US-focused resources still apply?

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

Remote-first global roles often offer 1.8 to 3 times India-local. See [interview-prep/india-market-snapshot.md](interview-prep/india-market-snapshot.md) for employer types and what they actually pay. These bands move quickly. Do your own salary research at the time you negotiate.

---

### 14. Which cloud should I learn, AWS, Azure, or GCP?

Pick the one your target employer uses. If you do not know: AWS has the largest market share globally, Azure dominates enterprise and Microsoft shops, GCP is strong in data and AI. Learn one deeply before adding a second. A candidate deep in one cloud beats a shallow multi-cloud one in almost every interview I have run.

---

### 15. Is "compliance" the same as "security"?

No. From [Module 0.1](modules/00-foundation-pack.md): security is about *protecting*, compliance is about *proving* you meet a rulebook. You can be compliant and unsafe (passing an audit with paper controls), or safe and non-compliant (strong controls, no documentation). The goal is both. In my experience, SOC 2 fails on evidence management more often than control design. The control exists. Nobody screenshotted it.

> *Compliance is the floor, not the ceiling.*

---

### 16. Do I need to read the full text of GDPR / ISO 27001 / NIST 800-53?

No. Read summaries first. Read the actual text only for the articles or controls you are working on. ICO guidance, EDPB guidelines, and AICPA TSC documents are more readable than the standards themselves. See [reference/free-resources.md](reference/free-resources.md). Anyone who tells you they have memorised all 93 ISO 27001:2022 Annex A controls is either lying or has nothing better to do.

---

### 17. How do I build a portfolio without a job?

Use [Module 17, Practice Scenarios](modules/17-practice-scenarios.md). Pick three scenarios. Do the deliverables properly. Put them in a public (sanitised) GitHub repo. Write a short README per scenario explaining your approach and the trade-offs you made. Link from the "Featured" section of LinkedIn.

Three completed capstones beat five certifications. Hiring managers can spot a "did the work" candidate within three minutes of opening the repo.

---

### 18. Should I pay for a bootcamp?

Almost never. The best resources in this field are free. Cloudflare Learning, NIST publications, ICO guidance, the Google Cybersecurity Certificate (audit free), Professor Messer, SANS Cyber Aces, TryHackMe free paths, Daniel Miessler's writing. See [reference/free-resources.md](reference/free-resources.md). Spend money only on certifications you actually need for a specific role.

---

### 19. How do I stand out in interviews?

Be the candidate who can map a control to specific framework criteria ("this maps to SOC 2 CC6.1, ISO 27001:2022 A.8.5, and NIST CSF 2.0 PR.AA-03"), and who has two capstone scenarios ready to *discuss*, not recite. Use the **C4R framework** (Clarify, Constrain, Consider, Choose, Risks/next-steps) for technical scenarios. See [interview-prep/star-and-c4r-frameworks.md](interview-prep/star-and-c4r-frameworks.md).

The other underrated move: have a strong opinion on something and be willing to defend it. "I think most SMBs do not need a GRC platform in year one" lands far better than reciting NIST CSF functions.

---

### 20. What is the single most useful skill I can build?

**Framework cross-mapping.** The ability to take one control and articulate how it satisfies SOC 2, ISO 27001, NIST CSF, PCI-DSS, and HIPAA at the same time. This is the skill that lets mature organisations build a control once and pass many audits. It is also the skill that makes you immediately credible in a GRC interview. See [reference/frameworks-cross-map.md](reference/frameworks-cross-map.md) and [Module 7.7](modules/07-grc-frameworks.md).

---

> *If you have a question that is not here, open an issue. The list grows from real questions.*

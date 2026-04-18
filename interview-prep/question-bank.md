# Interview Question Bank — Every Trap from Every Module

> *Curated from the "Interview traps" sections of all 18 modules, plus the high-frequency role questions in [Module 16.5](../modules/16-careers-and-interviews.md). Practise with these. Do not memorise — internalise.*

**Last verified:** 17 April 2026.

---

## How to use this bank

1. **Pick your role bucket** below.
2. **Cover the answer.** Speak the answer out loud. If you can't talk for 60–90 seconds, you don't know it well enough.
3. **Use a framework.** Behavioural questions → STAR. Technical scenarios → C4R. Risk/threat questions → CIA / STRIDE / DREAD. See [star-and-c4r-frameworks.md](star-and-c4r-frameworks.md).
4. **Pair-practise.** Reading silently is half the value of speaking.

---

## Foundational concepts (every role)

1. Define security, privacy, compliance, and GRC. Don't blur them.
2. "We're encrypting everything — are we privacy-compliant?" Defend your answer.
3. "Is HTTPS enough to protect login?"
4. "We have a cookie banner, so we're GDPR compliant." React.
5. Difference between encoding (Base64) and encryption.
6. What does the padlock icon in the browser actually mean?
7. "You find a vulnerability scanner reports 3,000 findings. What's the risk?"
8. Explain inherent vs residual risk; which one do executives care about and why?
9. Why is "ransomware" a bad way to phrase a risk?
10. Walk me through the difference between SLE, ARO, and ALE.
11. What's a Statement of Applicability, and why do auditors love it?
12. Explain *exception* vs *deviation* vs *non-conformity* vs *observation* vs *OFI*.

---

## GRC / Audit / Compliance

### Framework knowledge

13. Difference between SOC 2 Type I and Type II. When is each enough?
14. Name the five SOC 2 Trust Services Criteria. Which is mandatory?
15. How many Annex A controls in ISO 27001:2022? How are they organised?
16. Is ISO 27001 a certification of security?
17. What's the difference between NIST CSF and NIST 800-53?
18. Name the six NIST CSF 2.0 functions in order.
19. What changed in NIST CSF 2.0 vs 1.1?
20. Walk me through ISO 27001 Clauses 4–10. Why do major non-conformities come from these rather than Annex A?
21. SOC 2 vs ISO 27001 — which would you do first, and why does the answer depend on the buyer?

### Audit lifecycle

22. Walk me through a SOC 2 audit from kickoff to report.
23. What's in Section 4 of a SOC 2 report and why does it matter most?
24. How do you handle a control that fails during audit fieldwork?
25. Sample size for a control tested quarterly — what would you propose, and why?
26. What evidence would convince an auditor that your access-review control is operating?
27. A vendor refuses to share their SOC 2 Type II under NDA. What do you accept instead?
28. What's a Type 2 sample period, and what would you do if your evidence only goes back 2 months?
29. Explain Stage 1 vs Stage 2 audit (ISO 27001).
30. What happens during an ISO surveillance audit?

### Risk

31. Build me a risk register from zero — walk through the steps.
32. Difference between risk owner and control owner.
33. Three Lines of Defence — what are they and what does each do?
34. Quantitative vs qualitative risk — when would you use each?
35. Walk me through FAIR at a high level.
36. What's a KRI and how is it different from a KPI?
37. Define risk appetite vs risk tolerance.
38. Give me an example of a risk you'd recommend the business accept rather than mitigate.

### Vendor / TPRM

39. How do you tier vendors by risk? What goes into each tier?
40. A vendor sends back a SIG with 50 questions blank. What do you do?
41. How do you handle vendor concentration risk (e.g., single email-delivery vendor)?
42. What's the difference between a SOC 2 Type II report, an ISO 27001 certificate, and a CAIQ response in terms of assurance?

### Behavioural (STAR)

43. Tell me about a time you disagreed with engineering or a senior leader on a security decision.
44. What's a security opinion you hold that most people in this field disagree with?
45. Walk me through a control you designed that failed and what you learned.
46. What would you do in your first 90 days?

---

## Privacy

47. A DSR arrives for an account that was deleted 2 years ago. Walk me through your response.
48. Marketing wants to launch an analytics tool that tracks users across sites. What's your stance?
49. Explain lawful basis choice for an EdTech signup flow.
50. A vendor in the US processes EU personal data — what do you need contractually and operationally?
51. Walk through a DPIA for a new AI feature.
52. What's the difference between a controller and a processor under GDPR?
53. Explain Schrems II in two minutes. What did it change for US transfers?
54. DPDP Act 2023 — what's a Significant Data Fiduciary, and what extra obligations do they have?
55. Compare GDPR's lawful basis structure with DPDP's consent-by-default approach.
56. CERT-In 6-hour clock — what triggers it and what do you actually file?
57. A user emails: "Under GDPR Article 17, delete everything." Tax records require 7-year retention. How do you respond?
58. What goes in a Records of Processing Activities (ROPA)?
59. Verifiable parental consent under DPDP (children under 18) — how would you operationalise it?
60. Explain the difference between de-identification, pseudonymisation, and anonymisation.

---

## AppSec / DevSecOps / Tech

61. You find a vulnerable library in 200 repos. Walk me through your remediation plan.
62. Explain SAML vs OIDC to a product manager.
63. Design a secure CI/CD pipeline. What controls do you put where?
64. A dev says "we'll fix it next sprint" about a critical CVE. How do you respond?
65. Review this code snippet. What's wrong? *(Expect IDOR, injection, or crypto misuse.)*
66. STRIDE-model a multi-tenant SaaS API.
67. Walk me through OWASP Top 10 (Web 2021). Pick one and explain a real-world example.
68. What's in OWASP API Top 10 (2023) that's *not* in Web Top 10?
69. Explain OWASP LLM Top 10 (2025). Which one would you guard against most aggressively in a customer-facing AI feature?
70. Difference between SAST, DAST, IAST, SCA, and IaC scanning. When do you use each?
71. Explain envelope encryption (DEK / KEK).
72. Walk through a TLS 1.3 handshake in plain English.
73. What is a session fixation attack, and how do you prevent it?
74. Explain Content Security Policy (CSP) and what it protects against.
75. What's a SBOM, and why does it matter for supply-chain security?

---

## Cloud security

76. Explain the shared responsibility model for IaaS, PaaS, and SaaS.
77. Walk me through a BYOK design on AWS for a multi-tenant SaaS. What happens if a tenant deletes their key?
78. What's CSPM and how does it differ from CWPP?
79. Design a secure logging pipeline that satisfies SOC 2 CC7.2 *and* supports a DFIR investigation.
80. How do you isolate tenants in a multi-tenant Kubernetes cluster?
81. Explain CISA Zero Trust Maturity Model 2.0 — name the five pillars.
82. Where do most cloud breaches actually originate? (Hint: not zero-days.)

---

## SOC / Incident Response

83. Walk me through a suspected credential-stuffing incident from alert to closure.
84. Explain the NIST IR lifecycle (SP 800-61 Rev. 2).
85. Triage these 5 alerts in priority order. *(Expect a list from the interviewer.)*
86. Difference between containment, eradication, and recovery.
87. Lessons-learned example from an incident you handled (or a public one you've studied).
88. What's in a chain of custody?
89. MTTD vs MTTR vs dwell time — define each.
90. Walk me through the regulator-notification clocks for an India-based SaaS with EU customers (CERT-In 6h, GDPR 72h, DPDP per Rules).
91. The CTO wants to make a public statement an hour after detection. How do you handle that?
92. When would you force a global password reset, and when would you not?

---

## Business Continuity / DR

93. Define BCP, DR, and Crisis Management. Why do they get conflated?
94. What does a BIA produce and why does it matter?
95. Difference between RTO and RPO. Give an example where they're very different numbers.
96. Explain the 3-2-1 backup rule. What does the modern 3-2-1-1-0 add?
97. Hot vs warm vs cold site — pick one for a fintech and defend it.
98. How do you test a DR plan without breaking production?

---

## Emerging topics (2026 currency)

99. Explain "harvest now, decrypt later" and what FIPS 203/204/205 are for.
100. Walk through OWASP LLM Top 10 (2025). What's "prompt injection" and how do you mitigate it?
101. EU AI Act — name the four risk categories. Which obligations kicked in August 2025?
102. ISO 42001 — what is it and how does it relate to NIST AI RMF?
103. NIST AI RMF Generative AI Profile — what does it add?
104. What is MITRE ATLAS and how is it different from MITRE ATT&CK?
105. SEC Cybersecurity Disclosure Rule (Item 1.05) — what triggers the 4-business-day clock?

---

## The "explain it simply" curveballs

106. Teach me TLS as if I'm a 12-year-old.
107. Explain DPDP to an American friend in two minutes.
108. Defend why ISO 27001 is worth the cost to a sceptical founder.
109. Convince a CFO to fund an ISMS programme.
110. Explain Zero Trust without using the words "trust" or "perimeter."

---

## Behavioural (STAR-format)

111. Tell me about a time you found a serious issue and the team didn't believe you. What did you do?
112. Tell me about a time you had to deliver bad news to leadership.
113. Tell me about a project that failed. What did you learn?
114. Tell me about a time you had to influence without authority.
115. Tell me about a time you said "no" to a customer or sales team.
116. Tell me about a time you were wrong. How did you find out, and what did you change?

---

## How a senior interviewer scores you

The interviewer is mostly listening for three things:

1. **Do you know the words correctly?** "MFA, TOTP, FIDO2, passkey" should land in the right contexts.
2. **Can you explain trade-offs?** A senior never gives a one-word answer. They give a recommendation and one alternative.
3. **Do you reach for the right framework on a scenario?** STRIDE for threats. CIA for design discussion. NIST IR lifecycle for incidents. The C4R structure for ambiguous scenarios.

A confident "I don't know, but here's how I'd find out" beats a confident wrong answer. Honesty + structure is the highest-scoring combination.

---

> *Three completed [Module 17](../modules/17-practice-scenarios.md) capstone scenarios you can discuss in detail will outperform answering 100 of these from memory.*

# Glossary: Every Term, Defined

> *Alphabetised. Plain English. Cross-referenced to the module that teaches it properly. Where it helps, I have added a short note on how the term actually shows up in audits, vendor calls or board decks.*

**Last verified:** 17 April 2026. Compiled from the glossary recaps of all 18 modules. For acronym expansions, see [acronyms.md](acronyms.md).

---

## A

**Access control**: the discipline of deciding who gets to see and do what. Implemented as RBAC, ABAC, or ReBAC. See [Module 3](../modules/03-identity-and-access.md).

**Annex A (ISO 27001:2022)**: the 93-control reference list, organised into 4 themes: Organisational (37), People (8), Physical (14), Technological (34). See [Module 7.2](../modules/07-grc-frameworks.md).

**Asset**: something of value: data, a server, a reputation. The thing you're protecting. See [Module 0.4](../modules/00-foundation-pack.md).

**Audit**: independent examination producing an opinion on whether something meets stated criteria. See [Module 9](../modules/09-audit-lifecycle.md).

**Authentication**: proving you are who you say you are. Three factors: something you know, have, are. See [Module 0.3](../modules/00-foundation-pack.md).

**Authorisation**: deciding what an authenticated user is allowed to do. Often confused with authentication, even by senior engineers. In practice, half of access incidents are authorisation bugs, not authentication ones. See [Module 3](../modules/03-identity-and-access.md).

---

## B

**BAA (Business Associate Agreement)**: required HIPAA contract between a covered entity and any vendor handling PHI. See [Module 6.5](../modules/06-privacy-laws.md).

**BCP (Business Continuity Plan)**: keeping **business processes** running through a disruption. Distinct from DR. Usually owned outside IT, which is why it goes stale unless someone chases it. See [Module 11.5](../modules/11-incident-response-bcp.md).

**BIA (Business Impact Analysis)**: for each business process, determines MTD, RTO, RPO, dependencies. See [Module 11.5](../modules/11-incident-response-bcp.md).

**Breach**: confirmed unauthorised access, modification, destruction or disclosure of data. Starts regulator clocks. Be careful with the word internally: legal will want a precise definition before anyone uses it in writing. See [Module 11.2](../modules/11-incident-response-bcp.md) and [reference/regulator-clocks.md](regulator-clocks.md).

**BYOK (Bring Your Own Key)**: customer manages their own encryption key in the cloud provider's KMS. Distinct from HYOK (key never enters provider). See [Module 17 Scenario 3](../modules/17-practice-scenarios.md).

---

## C

**CAIQ (Consensus Assessments Initiative Questionnaire)**: CSA's standardised vendor security questionnaire. See [Module 12](../modules/12-vendor-risk.md).

**Certificate**: a signed cryptographic document proving a server is who it claims to be, issued by a CA. See [Module 0.2](../modules/00-foundation-pack.md).

**CIA Triad**: Confidentiality, Integrity, Availability. The original security objectives. See [Module 1](../modules/01-security-first-principles.md).

**Common Criteria (CC, in SOC 2)**: the mandatory Trust Services Criteria series (CC1–CC9), applied to all SOC 2 reports. See [Module 7.1](../modules/07-grc-frameworks.md).

**Compensating control**: a control implemented to substitute for a required control that cannot be implemented. See [Module 7](../modules/07-grc-frameworks.md), [Module 17 Scenario 10](../modules/17-practice-scenarios.md).

**Compliance**: proving you meet a specific external rulebook (law, standard, contract). Distinct from security. See [Module 0.1](../modules/00-foundation-pack.md).

**Consent Manager (DPDP)**: registered intermediaries that help individuals manage consents across data fiduciaries. Novel concept in DPDP. See [Module 6.3](../modules/06-privacy-laws.md).

**Containment**: IR phase: stopping the attacker spread without destroying evidence. See [Module 11.1](../modules/11-incident-response-bcp.md).

**Control**: a measure put in place to reduce risk. Synonyms: safeguard, countermeasure. See [Module 0.4](../modules/00-foundation-pack.md).

**Control owner**: the operational individual responsible for implementing and running a control. Distinct from the risk owner. In practice, audits go badly when a control has no clear owner or a vague one like "IT team". See [Module 8.5](../modules/08-risk-management.md).

**Controller (GDPR)**: the organisation that decides why and how personal data is processed. See [Module 6.1](../modules/06-privacy-laws.md).

**Cookie**: a small piece of data the server asks your browser to remember and send back. Used for sessions, personalisation, tracking. See [Module 0.2](../modules/00-foundation-pack.md).

**Cross-border transfer**: sending personal data to another jurisdiction. Mechanisms vary: SCCs/BCRs (GDPR), negative list (DPDP), security assessment (PIPL). See [Module 6](../modules/06-privacy-laws.md).

---

## D

**Data Fiduciary (DPDP)**: controller-equivalent in India's DPDP Act. See [Module 6.3](../modules/06-privacy-laws.md).

**Data Principal (DPDP)**: the individual whose data is processed (India's term for data subject). See [Module 6.3](../modules/06-privacy-laws.md).

**Data Subject (GDPR)**: the individual whose personal data is processed. See [Module 6.1](../modules/06-privacy-laws.md).

**DPA (Data Processing Agreement)**: required GDPR Art. 28 contract between controller and processor. See [Module 6.1](../modules/06-privacy-laws.md).

**DPIA (Data Protection Impact Assessment)**: required GDPR Art. 35 assessment for high-risk processing. Required for SDFs under DPDP. Auditors actually ask for the DPIA register, not just one DPIA; build it as a list from day one. See [Module 5](../modules/05-privacy-fundamentals.md), [Module 6](../modules/06-privacy-laws.md).

**DPO (Data Protection Officer)**: independent privacy officer; mandatory under GDPR Art. 37 in certain cases and for SDFs under DPDP. See [Module 6.9](../modules/06-privacy-laws.md).

**DR (Disaster Recovery)**: recovering **IT systems** after a disruption. Distinct from BCP. See [Module 11.5](../modules/11-incident-response-bcp.md).

**DSR / DSAR (Data Subject Request / Access Request)**: request from an individual to exercise rights (access, deletion, correction). SLA varies by law: 30 days (GDPR), 45 (CCPA), per Rules (DPDP). See [Module 5](../modules/05-privacy-fundamentals.md), [reference/regulator-clocks.md](regulator-clocks.md).

**Dwell time**: duration an attacker is inside a network before being detected. The metric you want minimised. See [Module 11.4](../modules/11-incident-response-bcp.md).

---

## E

**Encryption at rest**: protecting data stored on disk. SOC 2 CC6.1, ISO A.8.24, PCI Req 3, HIPAA §164.312. See [Module 2](../modules/02-cryptography-for-humans.md).

**Encryption in transit**: protecting data moving across a network. TLS is the dominant mechanism. See [Module 2](../modules/02-cryptography-for-humans.md).

**Eradication**: IR phase: removing malware, closing the exploited vulnerability, rotating credentials. See [Module 11.1](../modules/11-incident-response-bcp.md).

**Evidence**: artefacts an auditor can examine to confirm a control is operating. The daily output of a GRC analyst. The unsexy truth: most of the job is collecting, naming and filing this stuff. See [Module 9](../modules/09-audit-lifecycle.md).

**Exception**: a documented, approved deviation from a policy or control. Distinct from non-conformity. In practice, an exception register that nobody reviews is worse than no exceptions at all. See [Module 9](../modules/09-audit-lifecycle.md).

**Exploit**: the technique used to take advantage of a vulnerability. See [Module 0.4](../modules/00-foundation-pack.md).

---

## F

**FAIR (Factor Analysis of Information Risk)**: a quantitative risk methodology decomposing risk into Loss Event Frequency × Loss Magnitude. See [Module 8.3](../modules/08-risk-management.md).

**FIDO2 / WebAuthn**: phishing-resistant cryptographic authentication standard. The basis for passkeys. See [Module 0.3](../modules/00-foundation-pack.md).

**FIPS 140-3**: US standard for cryptographic module validation. See [Module 2](../modules/02-cryptography-for-humans.md).

**FIPS 203/204/205**: NIST post-quantum cryptography standards (ML-KEM, ML-DSA, SLH-DSA), finalised 2024. See [Module 15](../modules/15-emerging-topics.md).

---

## G

**GDPR (General Data Protection Regulation)**: EU privacy law, in effect since May 2018. Applies extraterritorially. See [Module 6.1](../modules/06-privacy-laws.md).

**Governance**: the management discipline that sets direction, accountability, and oversight. The "G" in GRC. See [Module 0.1](../modules/00-foundation-pack.md).

**GRC (Governance, Risk, Compliance)**: the management discipline that sits above security, privacy, and audit. See [Module 0.1](../modules/00-foundation-pack.md).

---

## H

**Heatmap (risk)**: 5×5 grid of likelihood × impact. The standard board visualisation. See [Module 8.2](../modules/08-risk-management.md).

**HIPAA**: US health-data law. Three rules: Privacy, Security, Breach Notification. 60-day breach clock. See [Module 6.5](../modules/06-privacy-laws.md).

---

## I

**Impact**: the magnitude of harm if a risk materialises. Measured per domain (financial, regulatory, reputational, operational). See [Module 8.2](../modules/08-risk-management.md).

**Incident**: confirmed adverse event affecting CIA or privacy. Triggers IR. See [Module 11](../modules/11-incident-response-bcp.md).

**Inherent risk**: risk before any controls are applied. See [Module 0.4](../modules/00-foundation-pack.md).

**ISMS (Information Security Management System)**: the documented management system at the heart of ISO 27001. Clauses 4–10 + Annex A. See [Module 7.2](../modules/07-grc-frameworks.md).

---

## K

**KEK / DEK**: Key Encryption Key / Data Encryption Key. The basis of envelope encryption. See [Module 2](../modules/02-cryptography-for-humans.md).

**KPI vs KRI**: Key Performance Indicator (have we achieved a goal?) vs Key Risk Indicator (are we approaching danger?). See [Module 8.8](../modules/08-risk-management.md).

---

## L

**Lawful basis (GDPR)**: one of six legal grounds for processing personal data: consent, contract, legal obligation, vital interests, public task, legitimate interests. See [Module 6.1](../modules/06-privacy-laws.md).

**Likelihood**: probability that a risk materialises. Scored qualitatively (1–5) or quantitatively (probability ranges). See [Module 8.2](../modules/08-risk-management.md).

---

## M

**MFA (Multi-Factor Authentication)**: requiring at least two different authentication factors. See [Module 0.3](../modules/00-foundation-pack.md).

**Mitigate**: a risk treatment option: reduce likelihood or impact via controls. The other three: avoid, transfer, accept. See [Module 8.2](../modules/08-risk-management.md).

**MTD / RTO / RPO**: Maximum Tolerable Downtime / Recovery Time Objective / Recovery Point Objective. The three numbers a BIA produces. See [Module 11.5](../modules/11-incident-response-bcp.md).

---

## N

**NIST CSF 2.0**: voluntary cybersecurity framework with six functions: Govern, Identify, Protect, Detect, Respond, Recover. Updated 2024. See [Module 7.3](../modules/07-grc-frameworks.md).

**Non-conformity (NC)**: audit finding of a clause or control not being met. Major NC vs Minor NC. Distinct from observation/OFI. See [Module 9](../modules/09-audit-lifecycle.md).

---

## P

**Passkey**: phishing-resistant credential based on FIDO2/WebAuthn; replaces passwords. The future of authentication. See [Module 0.3](../modules/00-foundation-pack.md).

**PHI (Protected Health Information)**: HIPAA-regulated health data. See [Module 6.5](../modules/06-privacy-laws.md).

**PII (Personally Identifiable Information)**: data that can identify an individual. See [Module 5](../modules/05-privacy-fundamentals.md).

**Policy**: what and why. The top of the document hierarchy: policy → standard → procedure → guideline. See [Module 0.5](../modules/00-foundation-pack.md), [Module 10](../modules/10-policies.md).

**Privacy**: respecting how personal data is collected, used, shared, retained. Distinct from security. See [Module 0.1](../modules/00-foundation-pack.md), [Module 5](../modules/05-privacy-fundamentals.md).

**Processor (GDPR)**: processes data on behalf of the controller. Most SaaS vendors are processors to their customers. See [Module 6.1](../modules/06-privacy-laws.md).

---

## R

**RBAC / ABAC / ReBAC**: Role / Attribute / Relationship-Based Access Control. The three dominant authorisation models. See [Module 3](../modules/03-identity-and-access.md).

**Residual risk**: risk remaining after controls are applied. Executives care about this number, not the inherent one. Be ready to explain how you got from one to the other. See [Module 0.4](../modules/00-foundation-pack.md).

**Risk**: the expected loss: roughly Likelihood × Impact. The vocabulary security people work in. See [Module 0.4](../modules/00-foundation-pack.md).

**Risk appetite**: qualitative statement of how much risk the organisation is willing to take. See [Module 8.6](../modules/08-risk-management.md).

**Risk owner**: business leader accountable for a risk. Usually not security. See [Module 8.5](../modules/08-risk-management.md).

**Risk register**: the artefact listing identified risks with scores, owners and treatments. The heart of the GRC programme, and usually the first thing a new analyst is asked to clean up. See [Module 8.4](../modules/08-risk-management.md).

**Risk tolerance**: quantitative limits per risk. ("We tolerate up to 2 critical vulnerabilities unpatched for no more than 7 days.") See [Module 8.6](../modules/08-risk-management.md).

**ROPA (Records of Processing Activities)**: required GDPR Art. 30 record of all processing activities. See [Module 6.1](../modules/06-privacy-laws.md).

---

## S

**SAML / OIDC**: federation standards for SSO. SAML is older XML-based; OIDC is newer JSON-based on OAuth 2. See [Module 3](../modules/03-identity-and-access.md).

**Schrems II**: 2020 CJEU ruling that invalidated the EU-US Privacy Shield, requiring SCCs + Transfer Impact Assessments + supplementary measures for US transfers. See [Module 6.1](../modules/06-privacy-laws.md).

**SCC (Standard Contractual Clauses)**: pre-approved EU contract clauses enabling lawful transfers to non-adequate countries. See [Module 6.1](../modules/06-privacy-laws.md).

**SDF (Significant Data Fiduciary)**: DPDP designation for controllers with extra obligations (DPO, DPIA, algorithmic audit). See [Module 6.3](../modules/06-privacy-laws.md).

**SDLC (Software Development Lifecycle)**: the development process, with security-by-design integrated throughout (SSDLC). See [Module 14](../modules/14-appsec.md).

**Security**: protecting systems and data from unauthorised access, modification, destruction. See [Module 0.1](../modules/00-foundation-pack.md).

**Session**: a server's memory of "you" across requests, usually tracked by a session cookie. See [Module 0.2](../modules/00-foundation-pack.md).

**SIEM**: centralised log aggregation, correlation, and alerting. See [Module 11.4](../modules/11-incident-response-bcp.md).

**SLE / ARO / ALE**: Single Loss Expectancy / Annualised Rate of Occurrence / Annualised Loss Expectancy. The classic quantitative risk formulas. See [Module 0.4](../modules/00-foundation-pack.md).

**SoA (Statement of Applicability)**: ISO 27001 document listing all 93 Annex A controls, applicability, justification and status. The backbone of an ISO audit, and the first document an auditor opens. Keep it current outside audit season. See [Module 7.2](../modules/07-grc-frameworks.md).

**SOAR**: automation of response playbooks, layered on a SIEM. See [Module 11.4](../modules/11-incident-response-bcp.md).

**SOC 1 vs SOC 2 vs SOC 3**: AICPA reports: SOC 1 = financial reporting controls; SOC 2 = Trust Services Criteria; SOC 3 = public-facing summary of SOC 2. See [Module 7.1](../modules/07-grc-frameworks.md).

**SSO (Single Sign-On)**: one identity provider authenticates a user to many apps. Standards: SAML, OIDC. See [Module 0.3](../modules/00-foundation-pack.md).

**Standard**: specific rules implementing a policy. Below policy, above procedure. See [Module 0.5](../modules/00-foundation-pack.md).

**STRIDE**: threat-modelling mnemonic: Spoofing, Tampering, Repudiation, Information disclosure, Denial of service, Elevation of privilege. See [Module 1](../modules/01-security-first-principles.md), [Module 14](../modules/14-appsec.md).

---

## T

**Threat**: a potential cause of harm: a hacker, a storm, a careless employee. See [Module 0.4](../modules/00-foundation-pack.md).

**Threat actor**: a specific *who* behind the threat: organised crime, nation state, insider, script kiddie. See [Module 0.4](../modules/00-foundation-pack.md).

**TIA (Transfer Impact Assessment)**: post-Schrems II assessment required alongside SCCs for US transfers. See [Module 6.1](../modules/06-privacy-laws.md).

**TLS (Transport Layer Security)**: the encryption protocol underlying HTTPS. Replaced SSL. See [Module 0.2](../modules/00-foundation-pack.md), [Module 2](../modules/02-cryptography-for-humans.md).

**TPRM (Third-Party Risk Management)**: the discipline of managing vendor security and privacy risks. See [Module 12](../modules/12-vendor-risk.md).

**Trust Services Criteria (TSC)**: SOC 2's five criteria: Security (mandatory), Availability, Processing Integrity, Confidentiality, Privacy. See [Module 7.1](../modules/07-grc-frameworks.md).

**Type I vs Type II (SOC 2)**: Type I = control design at a point in time; Type II = design and operating effectiveness over a period (3–12 months). See [Module 7.1](../modules/07-grc-frameworks.md).

---

## V

**Vulnerability**: a weakness a threat could exploit. Distinct from risk. See [Module 0.4](../modules/00-foundation-pack.md).

---

## Z

**Zero Trust**: security model based on "never trust, always verify"; identity-driven, no implicit network trust. CISA Zero Trust Maturity Model 2.0 is the operational reference. See [Module 4](../modules/04-network-security.md), [Module 15](../modules/15-emerging-topics.md).

---

> *Missing a term? Open an issue. The glossary grows alongside the curriculum, and the corners where the law and the tech meet keep adding new vocabulary.*

# Module 15: Emerging Topics: AI Governance, Zero Trust, PETs, Post-Quantum

> **Audience:** 🟢🟡🔴 · **Time:** ~70 min · **Prereqs:** Modules 1–14 lightly

## Why this matters

These four themes dominate 2025–2027 security roadmaps. Every interview touches at least one. Every audit will start asking about them. You do not need to be an expert. You need to speak fluently and know what "good" looks like, separately from the marketing.

A word of caution. AI governance and zero trust both attract more hype than any other area I have worked in. Be the person in the room who can tell the difference between a real control and a slide deck.

---

## 15.1 AI governance and AI security

Two different problems, often confused:

- **AI governance**: should we build or deploy this? Is it fair, transparent, accountable, legal?
- **AI security**: is the model or system safe from attack?

Both are needed and the team that owns each is rarely the same.

### The reference frameworks worth knowing

| Framework | Nature | Scope | Use it for |
|----------|--------|-------|-----------|
| **EU AI Act** (in force 2024, phased through 2026) | Law | EU-market AI | Legal compliance; risk classification (Unacceptable / High / Limited / Minimal) |
| **NIST AI RMF 1.0** (2023) + Generative AI Profile (2024) | Voluntary framework | Any AI | Structure for governance; 4 functions: Govern, Map, Measure, Manage |
| **ISO/IEC 42001:2023** | Certifiable management system | AI Management System (AIMS) | Board-level auditable AI governance |
| **ISO/IEC 23894:2023** | Guidance | AI risk management | Practical risk treatment |
| **OECD AI Principles** | Principles | International | Policy alignment |
| **India: MeitY AI Advisory (March 2024) + DPDP Act** | Soft law plus law | India deployment | Consent, explainability, bias testing |
| **OWASP LLM Top 10 (2025)** | Checklist | LLM apps | Engineering controls |
| **MITRE ATLAS** | Adversary TTPs | ML systems | Threat modelling AI |
| **Google SAIF / Microsoft Responsible AI Standard** | Vendor framework | Model and app level | Practical controls |

A practical view: if your company sells into the EU, the EU AI Act is the binding one and everything else is a way of preparing for it. If you do not sell into the EU, NIST AI RMF plus ISO 42001 are a reasonable backbone. ISO 42001 is the certifiable one and customer questionnaires are starting to ask about it.

### Risk classes (EU AI Act, simplified)

- **Unacceptable**: social scoring, real-time biometric mass surveillance. Banned.
- **High risk**: hiring, credit, education, medical, critical infrastructure, law enforcement. Mandatory conformity assessment, logging, human oversight, accuracy, transparency.
- **Limited risk**: chatbots, deepfakes. Disclosure obligations.
- **Minimal**: spam filters, games. No specific obligations.
- **GPAI** (general-purpose AI / foundation models): separate obligations, stricter for "systemic risk."

In my experience, the most contested classification is "high risk" for hiring tools. Product teams will argue their tool is "decision support" not "decision making." Read the act, not the marketing. The line is thinner than people pretend.

### Attack surface unique to AI

| Attack | Example |
|--------|---------|
| **Prompt injection** (direct and indirect) | Malicious instructions smuggled via user input or retrieved documents |
| **Jailbreak** | Bypassing safety alignment |
| **Data poisoning** | Contaminating training data to corrupt the model |
| **Model inversion** | Reconstructing training data from queries |
| **Membership inference** | Detecting whether a record was in the training set |
| **Model theft / extraction** | Querying to replicate |
| **Backdoor / Trojan** | Planted trigger causes misclassification |
| **Evasion (adversarial examples)** | Tiny perturbations fool the classifier |
| **Supply chain** | Compromised model weights or fine-tune dataset |

Indirect prompt injection is the one people underestimate. If your LLM reads from a document store, that store is now part of your attack surface.

### Baseline controls for a company deploying LLMs

1. AI inventory and use-case register: owner, risk class, data, model, vendor.
2. Procurement gate for AI vendors: DPA, model card, training data provenance, retention, training opt-out.
3. Input sanitisation and output filtering for prompt injection. Both, not one.
4. Red-team before launch. Re-red-team on model or prompt changes.
5. Human-in-the-loop for high-impact decisions.
6. Logging of prompts and outputs. Watch PII. Encrypt. Set retention deliberately.
7. Transparency notice to users.
8. DPIA when personal data is involved.
9. Monitoring for drift, hallucination rate, harmful output rate.
10. Incident response playbook for AI-specific incidents. Standard IR runbooks miss this category.

---

## 15.2 Zero Trust: beyond the buzzword

Core idea (NIST 800-207): never trust, always verify, assume breach. No implicit trust from network location.

### The maturity model (CISA ZTMM 2.0)

Five pillars, each assessed across Traditional → Initial → Advanced → Optimal:

1. **Identity**: MFA everywhere, then phishing-resistant MFA, then continuous authentication with risk scoring.
2. **Devices**: inventory, EDR plus compliance, continuous attestation.
3. **Networks**: segmentation, micro-segmentation, encrypted-everywhere, no VPN perimeter.
4. **Applications and Workloads**: vulnerability management, software supply chain, continuous authorisation.
5. **Data**: classification, labelling plus DLP, automated context-aware access.

Cross-cutting: Visibility and Analytics, Automation and Orchestration, Governance.

### What Zero Trust is not

- Not a product. No vendor sells "Zero Trust" off the shelf, despite what their booth at RSA implies.
- Not "VPN plus MFA" rebranded.
- Not something you finish in a quarter. It is a 2 to 5-year architecture shift.

### What ZT actually looks like in a real shop

- SSO and phishing-resistant MFA (FIDO2) as the identity backbone.
- Device posture checks at every access decision.
- Reverse-proxy or identity-aware proxy (Cloudflare Access, Google BeyondCorp, Zscaler, Tailscale) replacing the VPN.
- Per-app authorisation via an ABAC or ReBAC policy engine.
- Micro-segmentation at workload level. Service mesh mTLS (Istio, Linkerd).
- Continuous session validation with risk-based re-auth.

The honest version: most companies are stuck at "MFA everywhere, SSO for most things." That is fine. It is a real improvement over the status quo of five years ago. Do not let perfect be the enemy of useful.

---

## 15.3 Privacy-Enhancing Technologies (PETs)

PETs let you use data without exposing it. Regulators (EDPB, UK ICO, India DPDP rules) increasingly reward their use.

| Technique | What it does | Trade-off | Example use |
|-----------|--------------|-----------|-------------|
| **Pseudonymisation** | Replace direct IDs with tokens | Still personal data; re-identifiable with key | Analytics, research |
| **Anonymisation** (k-anonymity, l-diversity, t-closeness) | Records indistinguishable from k-1 others | Utility loss; not always enough | Open data release |
| **Differential Privacy (DP)** | Adds calibrated noise; mathematical guarantee (ε) | Utility loss; needs expertise to set ε | US Census, Apple, Google |
| **Homomorphic Encryption (HE)** | Compute on ciphertext | Slow; specific operations | Private ML scoring |
| **Secure Multi-Party Computation (SMPC)** | Multiple parties jointly compute without sharing inputs | Complex; network-heavy | Banks sharing fraud signals |
| **Federated Learning** | Model trains on-device; only updates leave | Needs DP on gradients to be fully safe | Mobile keyboards, hospitals |
| **TEE / Confidential Computing** | Data encrypted in RAM; code attested | Side-channel history | Healthcare, finance cloud workloads |
| **Synthetic data** | Statistically similar fake data | Risk of memorisation; utility varies | Dev/test, ML training |
| **Zero-Knowledge Proofs (ZKP)** | Prove a statement without revealing the data | Complex; performance | Age verification, KYC-lite |
| **Tokenisation** | Replace value with surrogate; original in vault | Vault is still sensitive | Payment processing |

Rule of thumb: no PET is a silver bullet. Layer them with traditional controls (access, encryption, governance). And do not call data "anonymised" because you removed names. Re-identification research keeps embarrassing teams who did exactly that.

---

## 15.4 Post-Quantum Cryptography (PQC)

### The risk

A sufficiently large quantum computer breaks RSA and ECC via Shor's algorithm. It does not exist yet and estimates range from the 2030s to the 2040s. The reason to care now is "harvest now, decrypt later": adversaries can store encrypted traffic today and decrypt it once quantum is real.

### What happened in 2024

NIST finalised the first PQC standards:

| Standard | Algorithm | Replaces |
|----------|-----------|----------|
| **FIPS 203** | ML-KEM (Kyber) | Key encapsulation / key exchange (ECDH, RSA-KEM) |
| **FIPS 204** | ML-DSA (Dilithium) | Digital signatures (ECDSA, RSA) |
| **FIPS 205** | SLH-DSA (SPHINCS+) | Hash-based signatures (backup / firmware) |
| (later) | FN-DSA (Falcon) | Compact signatures |

Symmetric crypto (AES-256, SHA-256) is considered quantum-safe. Just use sufficiently large keys.

### What to do now

1. Crypto inventory. Know where TLS, signing, and at-rest encryption live across your stack.
2. Crypto-agility. Abstract crypto so algorithms can be swapped without rewriting the app.
3. Hybrid deployments. Classical plus PQC together. TLS 1.3 X25519 + ML-KEM is already live in Chrome and Cloudflare.
4. Prioritise long-lived confidentiality data first (healthcare, government, IP).
5. Ask vendors about their PQC roadmaps. It is a reasonable DDQ question in 2025.

For most SaaS companies, the practical answer right now is "wait for the platforms (cloud KMS, TLS libraries) to add PQC, then turn it on." Rolling your own PQC migration without that is a project for a much larger team.

---

## 15.5 Other things worth a paragraph each

- **SASE (Secure Access Service Edge)**: converged network and security as a cloud service (ZTNA + SWG + CASB + FWaaS + DLP). Vendors: Zscaler, Netskope, Palo Alto Prisma, Cloudflare, Cato.
- **SSE**: SASE minus the network (SD-WAN) side.
- **XDR**: cross-domain detection and response (endpoint + email + identity + cloud).
- **MDR**: managed detection and response. Outsourced SOC. Useful when you cannot staff one.
- **CTEM (Continuous Threat Exposure Management)**: Gartner model. Scope, discover, prioritise, validate, mobilise.
- **CAASM (Cyber Asset Attack Surface Management)**: know everything you own.
- **DSPM (Data Security Posture Management)**: find sensitive data wherever it lives and apply controls.
- **OT / ICS security**: manufacturing and utilities. IEC 62443. Very different culture from IT.
- **Space and satellite security**: emerging. NIST IR 8270 is a starting point.

---

## 15.6 Interview traps

- "Zero Trust versus Zero Knowledge?" ZT is an architecture model. ZK is a crypto technique. Unrelated.
- "Is differential privacy enough to anonymise data under GDPR?" Depends on ε and context. GDPR considers likelihood of re-identification. DP alone does not auto-make data non-personal.
- "If quantum computers do not exist, why care about PQC?" Harvest-now-decrypt-later plus the multi-year migration lead time.
- "Your CEO read about the EU AI Act. What is the first thing you do?" Inventory AI use cases. Classify by risk. Check whether any are "high risk." Gap-assess against obligations.

---

## 15.7 Go deeper

- 🏛 [EU AI Act: official consolidated text](https://artificialintelligenceact.eu/) · [Commission Q&A](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai)
- 🏛 [NIST AI RMF 1.0 + Gen-AI Profile](https://www.nist.gov/itl/ai-risk-management-framework)
- 🏛 [ISO/IEC 42001 overview](https://www.iso.org/standard/81230.html) (spec is paid; summaries free)
- 🏛 [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/) · [MITRE ATLAS](https://atlas.mitre.org/)
- 🏛 [Google SAIF](https://saif.google/) · [Microsoft Responsible AI Standard (PDF)](https://www.microsoft.com/en-us/ai/responsible-ai)
- 🏛 [NIST SP 800-207 Zero Trust Architecture](https://csrc.nist.gov/publications/detail/sp/800-207/final) · [CISA Zero Trust Maturity Model 2.0](https://www.cisa.gov/zero-trust-maturity-model)
- 🏛 [UK ICO: PETs guidance](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/data-sharing/privacy-enhancing-technologies/)
- 🏛 [NIST PQC Project](https://csrc.nist.gov/projects/post-quantum-cryptography) · [FIPS 203/204/205](https://csrc.nist.gov/pubs/fips/203/final)
- 📰 [Simon Willison's Weblog (LLM security)](https://simonwillison.net/) · [Embrace The Red (prompt-injection research)](https://embracethered.com/blog/)
- 🧪 [Gandalf by Lakera (prompt-injection CTF)](https://gandalf.lakera.ai/) · [Prompt Airlines](https://promptairlines.com/)

## Module 15: Glossary recap

AI RMF, EU AI Act, GPAI, Systemic risk, ISO 42001, OWASP LLM Top 10, MITRE ATLAS, Prompt injection (direct/indirect), Jailbreak, Data poisoning, Model inversion, Membership inference, Model extraction, Evasion, SAIF, Zero Trust, NIST 800-207, CISA ZTMM 2.0, Phishing-resistant MFA, Identity-aware proxy, Micro-segmentation, Service mesh mTLS, PETs, k-anonymity, l-diversity, t-closeness, Differential Privacy (ε), Homomorphic Encryption, SMPC, Federated Learning, TEE / Confidential Computing, Synthetic data, ZKP, Tokenisation, PQC, ML-KEM, ML-DSA, SLH-DSA, Crypto-agility, Harvest-now-decrypt-later, SASE, SSE, XDR, MDR, CTEM, CAASM, DSPM, OT/ICS, IEC 62443.

→ Next: [Module 16: Careers, Certifications & Interview Prep](16-careers-and-interviews.md)

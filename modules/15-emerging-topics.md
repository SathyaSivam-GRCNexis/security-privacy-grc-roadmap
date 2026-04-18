# Module 15 — Emerging Topics: AI Governance, Zero Trust, PETs, Post-Quantum

> **Audience:** 🟢🟡🔴 · **Time:** ~70 min · **Prereqs:** Modules 1–14 lightly

## Why this matters

These four themes dominate 2025–2027 security roadmaps. Every job interview touches at least one. Every audit will start asking about them. You don't need to be an expert — you need to speak fluently and know what "good" looks like.

---

## 15.1 AI governance & AI security

Two different problems, often confused:

- **AI governance** — *should* we build/deploy this? Is it fair, transparent, accountable, legal?
- **AI security** — *is* the model/system safe from attack?

Both are needed.

### The reference frameworks you must know

| Framework | Nature | Scope | Use it for |
|----------|--------|-------|-----------|
| **EU AI Act** (in force 2024, phased through 2026) | Law | EU-market AI | Legal compliance; risk-classification of AI systems (Unacceptable / High / Limited / Minimal) |
| **NIST AI RMF 1.0** (2023) + Generative AI Profile (2024) | Voluntary framework | Any AI | Structure for governance; 4 functions — **Govern, Map, Measure, Manage** |
| **ISO/IEC 42001:2023** | Certifiable management system | AI Management System (AIMS) | Board-level auditable AI governance |
| **ISO/IEC 23894:2023** | Guidance | AI risk management | Practical risk treatment |
| **OECD AI Principles** | Principles | International | Policy alignment |
| **India: MeitY AI Advisory (March 2024) + DPDP Act** | Soft law + law | India deployment | Consent, explainability, bias testing |
| **OWASP LLM Top 10** | Checklist | LLM apps | Engineering controls |
| **MITRE ATLAS** | Adversary TTPs | ML systems | Threat modelling AI |
| **Google SAIF / Microsoft Responsible AI Standard** | Vendor framework | Model & app level | Practical controls |

### Risk classes (EU AI Act, simplified)

- **Unacceptable** — social scoring, real-time biometric mass surveillance (banned).
- **High risk** — hiring, credit, education, medical, critical infra, law enforcement. Mandatory conformity assessment, logging, human oversight, accuracy, transparency.
- **Limited risk** — chatbots, deepfakes: disclosure obligations.
- **Minimal** — spam filters, games: none.
- **GPAI** (General-purpose AI / foundation models) — separate obligations, stricter for "systemic risk."

### Attack surface unique to AI

| Attack | Example |
|--------|---------|
| **Prompt injection** (direct + indirect) | Malicious instructions smuggled via user input or retrieved docs |
| **Jailbreak** | Bypassing safety alignment |
| **Data poisoning** | Contaminating training data to corrupt model |
| **Model inversion** | Reconstruct training data from model queries |
| **Membership inference** | Detecting if a record was in training set — a privacy violation |
| **Model theft / extraction** | Querying to replicate |
| **Backdoor / Trojan** | Planted trigger causes misclassification |
| **Evasion (adversarial examples)** | Tiny perturbations fool classifier |
| **Supply-chain** | Compromised model weights or fine-tune dataset |

### Baseline controls for a company deploying LLMs

1. AI inventory & use-case register (owner, risk class, data, model, vendor).
2. Procurement gate for AI vendors (DPA, model card, training-data provenance, retention).
3. Input sanitisation + output filtering for prompt injection.
4. Red-team before launch; re-red-team on model/prompt changes.
5. Human-in-the-loop for high-impact decisions.
6. Logging of prompts/outputs (mind PII — encrypt, retention policy).
7. Transparency notice to users.
8. DPIA when personal data is involved.
9. Monitoring for drift, hallucination rate, harmful output rate.
10. Incident response playbook for AI-specific incidents.

---

## 15.2 Zero Trust — beyond the buzzword

**Core idea (NIST 800-207):** never trust, always verify, assume breach. No implicit trust from network location.

### The maturity model (CISA ZTMM 2.0)

Five pillars, each assessed across **Traditional → Initial → Advanced → Optimal**:

1. **Identity** — MFA everywhere → phishing-resistant MFA → continuous authentication with risk scoring.
2. **Devices** — inventory → EDR + compliance → continuous attestation.
3. **Networks** — segmentation → micro-seg → encrypted all traffic, no VPN perimeter.
4. **Applications & Workloads** — vuln mgmt → software supply-chain → continuous authorisation.
5. **Data** — classification → labelling + DLP → automated, context-aware access.

Cross-cutting: **Visibility & Analytics**, **Automation & Orchestration**, **Governance**.

### What Zero Trust is *not*

- Not a product. No single vendor sells "Zero Trust."
- Not "VPN + MFA" rebranded.
- Not something you buy in a quarter; it's a 2–5-year architecture shift.

### What ZT actually looks like

- SSO + phishing-resistant MFA (FIDO2) as the identity backbone.
- Device posture checks at every access decision.
- Reverse-proxy / identity-aware proxy (Cloudflare Access, Google BeyondCorp, Zscaler, Tailscale) replacing VPN.
- Per-app authorisation (ABAC/ReBAC policy engine).
- Micro-segmentation at workload (service mesh mTLS — Istio, Linkerd).
- Continuous session validation (risk-based re-auth).

---

## 15.3 Privacy-Enhancing Technologies (PETs)

PETs let you *use* data without exposing it. Regulators (EDPB, UK ICO, India DPDP rules) increasingly reward their use.

| Technique | What it does | Trade-off | Example use |
|-----------|--------------|-----------|-------------|
| **Pseudonymisation** | Replace direct IDs with tokens | Still personal data; re-identifiable with key | Analytics, research |
| **Anonymisation (k-anonymity, l-diversity, t-closeness)** | Records indistinguishable from k-1 others | Utility loss; not always enough | Open data release |
| **Differential Privacy (DP)** | Adds calibrated noise; mathematical privacy guarantee (ε) | Utility loss; needs expertise to set ε | US Census, Apple, Google |
| **Homomorphic Encryption (HE)** | Compute on ciphertext | Slow; mostly for specific ops | Private ML scoring |
| **Secure Multi-Party Computation (SMPC)** | Multiple parties jointly compute without sharing inputs | Complex; network-heavy | Banks checking shared fraud signals |
| **Federated Learning** | Model trains on-device; only updates leave | Requires gradient privacy (DP) to be fully safe | Mobile keyboards, hospitals |
| **Trusted Execution Environments (TEE) / Confidential Computing** | Data encrypted even in RAM; code attested | Vendor side-channel history | Healthcare, finance cloud workloads |
| **Synthetic data** | Statistically similar fake data | Risk of memorisation; utility varies | Dev/test environments, ML training |
| **Zero-Knowledge Proofs (ZKP)** | Prove a statement without revealing underlying data | Complex; performance | Age verification, KYC-lite |
| **Tokenisation** | Replace value with surrogate; original in vault | Vault is still sensitive | Payment processing |

**Rule of thumb:** no PET is a silver bullet. Layer them with traditional controls (access, encryption, governance).

---

## 15.4 Post-Quantum Cryptography (PQC)

### The risk

A sufficiently large quantum computer breaks RSA and ECC via Shor's algorithm. Doesn't exist yet; estimates range 2030s–2040s. But: **"harvest now, decrypt later"** — adversaries can store encrypted traffic today and decrypt post-quantum.

### What happened (2024)

NIST finalised the first PQC standards:

| Standard | Algorithm | Replaces |
|----------|-----------|----------|
| **FIPS 203** | ML-KEM (Kyber) | Key encapsulation / key exchange (ECDH, RSA-KEM) |
| **FIPS 204** | ML-DSA (Dilithium) | Digital signatures (ECDSA, RSA) |
| **FIPS 205** | SLH-DSA (SPHINCS+) | Hash-based signatures (backup / firmware) |
| (2024) | FN-DSA (Falcon) coming | Compact signatures |

Symmetric crypto (AES-256, SHA-256) is considered quantum-safe; just use large enough keys.

### What to do now

1. **Crypto inventory** — know where TLS, signing, at-rest encryption live.
2. **Crypto-agility** — abstract crypto so algorithms can be swapped.
3. **Hybrid deployments** — classical + PQC together (TLS 1.3 X25519+ML-KEM is already live in Chrome/Cloudflare).
4. **Prioritise long-lived-confidentiality data** first (healthcare, government, IP).
5. **Ask vendors** about PQC roadmaps — it's a reasonable DDQ question now.

---

## 15.5 Other things worth a paragraph each

- **SASE (Secure Access Service Edge)** — converged network + security as cloud service (ZTNA + SWG + CASB + FWaaS + DLP). Vendors: Zscaler, Netskope, Palo Alto Prisma, Cloudflare, Cato.
- **SSE** — SASE minus the network (SD-WAN) side.
- **XDR** — cross-domain detection/response (endpoint + email + identity + cloud).
- **MDR** — managed detection and response (outsourced SOC).
- **CTEM (Continuous Threat Exposure Management)** — Gartner model; scope → discover → prioritise → validate → mobilise.
- **CAASM (Cyber Asset Attack Surface Management)** — know everything you own.
- **DSPM (Data Security Posture Management)** — find sensitive data wherever it lives, apply controls.
- **OT / ICS security** — manufacturing, utilities; IEC 62443; very different from IT.
- **Space / satellite security** — emerging; NIST IR 8270.

---

## 15.6 Interview traps

- *"Zero Trust vs Zero Knowledge?"* (ZT = architecture model; ZK = crypto technique. Unrelated.)
- *"Is differential privacy enough to anonymise data under GDPR?"* (Depends on ε and context; GDPR considers likelihood of re-identification. DP alone doesn't auto-make data non-personal.)
- *"If quantum computers don't exist, why care about PQC?"* (Harvest-now-decrypt-later + multi-year migration lead time.)
- *"Your CEO read about the EU AI Act — what's the first thing you do?"* (Inventory AI use cases, classify risk, check if any fall in "high-risk" category, gap-assess against obligations.)

---

## 15.7 Go deeper

- 🏛 [EU AI Act — official consolidated text](https://artificialintelligenceact.eu/) · [Commission Q&A](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai)
- 🏛 [NIST AI RMF 1.0 + Gen-AI Profile](https://www.nist.gov/itl/ai-risk-management-framework)
- 🏛 [ISO/IEC 42001 overview](https://www.iso.org/standard/81230.html) (spec is paid; summaries free)
- 🏛 [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/) · [MITRE ATLAS](https://atlas.mitre.org/)
- 🏛 [Google SAIF](https://saif.google/) · [Microsoft Responsible AI Standard (PDF)](https://www.microsoft.com/en-us/ai/responsible-ai)
- 🏛 [NIST SP 800-207 Zero Trust Architecture](https://csrc.nist.gov/publications/detail/sp/800-207/final) · [CISA Zero Trust Maturity Model 2.0](https://www.cisa.gov/zero-trust-maturity-model)
- 🏛 [UK ICO — PETs guidance](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/data-sharing/privacy-enhancing-technologies/)
- 🏛 [NIST PQC Project](https://csrc.nist.gov/projects/post-quantum-cryptography) · [FIPS 203/204/205](https://csrc.nist.gov/pubs/fips/203/final)
- 📰 [Simon Willison's Weblog (LLM security)](https://simonwillison.net/) · [Embrace The Red (prompt-injection research)](https://embracethered.com/blog/)
- 🧪 [Gandalf by Lakera (prompt-injection CTF)](https://gandalf.lakera.ai/) · [Prompt Airlines](https://promptairlines.com/)

## Module 15 — Glossary recap

AI RMF, EU AI Act, GPAI, Systemic risk, ISO 42001, OWASP LLM Top 10, MITRE ATLAS, Prompt injection (direct/indirect), Jailbreak, Data poisoning, Model inversion, Membership inference, Model extraction, Evasion, SAIF, Zero Trust, NIST 800-207, CISA ZTMM 2.0, Phishing-resistant MFA, Identity-aware proxy, Micro-segmentation, Service mesh mTLS, PETs, k-anonymity, l-diversity, t-closeness, Differential Privacy (ε), Homomorphic Encryption, SMPC, Federated Learning, TEE / Confidential Computing, Synthetic data, ZKP, Tokenisation, PQC, ML-KEM, ML-DSA, SLH-DSA, Crypto-agility, Harvest-now-decrypt-later, SASE, SSE, XDR, MDR, CTEM, CAASM, DSPM, OT/ICS, IEC 62443.

→ Next: [Module 16 — Careers, Certifications & Interview Prep](16-careers-and-interviews.md)

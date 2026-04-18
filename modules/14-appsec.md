# Module 14 — Application Security (AppSec)

> **Audience:** 🔴 primary · 🟡 concepts only · **Time:** ~80 min · **Prereqs:** Module 2, 13

## Why this matters

Most of what your company sells is software. Bugs in that software = the largest class of actual breaches (SQL injection, auth bypass, broken access control, dependency CVEs). If you work in security or GRC and can't talk to engineers about AppSec, you will be excluded from the conversations that matter.

---

## 14.1 Where AppSec fits in the SDLC

Old model: security test at the end, block release. New model: **shift left** AND **shield right** — controls at every stage.

| SDLC stage | Security activity |
|-----------|-------------------|
| Design | Threat modelling, privacy review, abuse cases |
| Code | IDE plugins, secure coding training, pre-commit hooks (secrets, lint) |
| Build | SAST, SCA, secrets scan, IaC scan, SBOM |
| Test | DAST, fuzzing, manual pen-test, API scans |
| Release | Signed artefacts, SLSA provenance, release gates |
| Run | RASP/WAF, runtime vuln mgmt, bug bounty, logging & alerting |
| Respond | Incident response, coordinated disclosure |

---

## 14.2 OWASP Top 10 — the canonical vuln list (Web, 2021)

| # | Category | One-liner |
|---|----------|-----------|
| A01 | **Broken Access Control** | Users can do things they shouldn't — most common real breach class. |
| A02 | **Cryptographic Failures** | Weak/no crypto, hardcoded keys, plaintext passwords. |
| A03 | **Injection** | SQLi, NoSQLi, OS command, LDAP, template injection. |
| A04 | **Insecure Design** | Missing threat modelling; design-level flaws can't be patched. |
| A05 | **Security Misconfiguration** | Default creds, verbose errors, exposed admin panels. |
| A06 | **Vulnerable & Outdated Components** | Log4Shell-style; supply chain. |
| A07 | **Identification & Authentication Failures** | Weak MFA, session fixation, credential stuffing. |
| A08 | **Software & Data Integrity Failures** | Unsigned updates, compromised CI/CD. |
| A09 | **Security Logging & Monitoring Failures** | Can't detect what you don't log. |
| A10 | **SSRF** | Server tricked into requesting internal URLs (Capital One breach class). |

### OWASP API Top 10 (2023)

APIs are a separate attack surface; the list differs:

1. Broken Object Level Authorization (BOLA/IDOR)
2. Broken Authentication
3. Broken Object Property Level Authorization
4. Unrestricted Resource Consumption
5. Broken Function Level Authorization
6. Unrestricted Access to Sensitive Business Flows
7. SSRF
8. Security Misconfiguration
9. Improper Inventory Management
10. Unsafe Consumption of APIs

### OWASP LLM Top 10 (2023/2025)

1. Prompt Injection
2. Insecure Output Handling
3. Training Data Poisoning
4. Model Denial of Service
5. Supply Chain Vulnerabilities
6. Sensitive Information Disclosure
7. Insecure Plugin Design
8. Excessive Agency
9. Overreliance
10. Model Theft

(See Module 15 for deeper AI-security treatment.)

---

## 14.3 The testing toolbox — what each acronym means

| Tool type | Finds | How | Stage | Free examples |
|-----------|-------|-----|-------|--------------|
| **SAST** (Static AST) | Code-level flaws | Analyses source code | Pre-merge / CI | **Semgrep**, Bandit (Python), gosec, Brakeman (Rails), SonarQube Community |
| **SCA** (Software Composition Analysis) | Vulnerable dependencies | Parses manifests vs CVE DB | CI | **Dependabot**, Renovate, **Trivy**, OWASP Dependency-Check, Grype |
| **DAST** (Dynamic) | Runtime web flaws | Crawls & attacks running app | Staging | **OWASP ZAP**, Nikto |
| **IAST** | Runtime from inside | Agent in test app | Staging | Contrast Community (limited) |
| **Secrets scanning** | Keys, tokens | Regex + entropy on code/history | Pre-commit + CI | **gitleaks**, **trufflehog**, GitHub secret scanning |
| **Container scan** | Image CVEs, misconfigs | Scans image layers | CI | **Trivy**, Grype, Clair |
| **IaC scan** | Cloud misconfig in code | Parses Terraform/CFN/K8s | CI | **Checkov**, tfsec, kube-linter |
| **Fuzzing** | Crashes, edge cases | Random/guided inputs | Nightly | AFL++, libFuzzer, OSS-Fuzz |
| **RASP** | Runtime attacks | In-app agent blocks | Prod | (mostly commercial) |
| **WAF** | Web attacks at edge | Rules + ML | Prod | ModSecurity + OWASP CRS |
| **API security** | API-specific issues | Traffic learning | Prod | (mostly commercial; APIsec.ai free tier) |
| **Pen-test** | Chained real-world attack | Humans | Annually / major release | (engage firm; CREST certified in India) |

### Beginner mistake

Running only SAST and calling it secure. SAST finds code flaws; it misses design flaws, infra flaws, runtime flaws, dependency flaws. You need the **layered set** above, not any single tool.

---

## 14.4 Secure coding fundamentals

### Input

- **Validate** on a strict allow-list, not deny-list.
- **Parse, don't sanitise** — use typed parsers.

### Output

- **Contextual output encoding** — HTML, attribute, JS, URL, CSS — each needs its own encoder.

### Database

- **Parameterised queries / prepared statements.** Never string-concatenate SQL.

### Authentication

- See Module 3. Use a vetted library (Passport, Spring Security, Devise); don't invent your own.

### Authorisation

- Centralise — a single policy engine (OPA, Cedar, Oso, custom) rather than `if user.role == "admin"` scattered everywhere.
- Test both positive (can do) and **negative** (can't do what they shouldn't).

### Session

- HttpOnly, Secure, SameSite cookies. Short idle timeout. Rotate on privilege change.

### Errors & logs

- No stack traces to users. No secrets in logs. Structured logs with correlation IDs.

### Dependencies

- Lockfiles committed. Automatic updates for security patches. Known-vulnerable versions fail the build.

### Cryptography

- See Module 2. Never roll your own. Use libsodium / platform crypto.

---

## 14.5 Secrets management

- No secrets in code, in env files committed to Git, in Dockerfiles, or in CI logs.
- Use a **vault** — HashiCorp Vault, AWS Secrets Manager, Azure Key Vault, GCP Secret Manager, Doppler, Infisical.
- **Dynamic secrets** where supported (DB creds generated per-session).
- **Rotation** — automated, with app tolerance for mid-session rotation.
- **Break-glass paths** documented.

---

## 14.6 Supply-chain security

Recent incidents (SolarWinds, Log4Shell, 3CX, xz-utils backdoor) made this a board-level topic.

- **SBOM** (Software Bill of Materials) — inventory of every component and version. Formats: **SPDX**, **CycloneDX**. Generate with Syft/cyclonedx-cli.
- **SLSA (Supply-chain Levels for Software Artifacts)** — levels 1–4 for build integrity.
- **Signed artefacts** — Sigstore/Cosign sign images; verify on deploy.
- **Provenance** — attestations describing how, when, by whom a build was produced.
- **Vulnerability feed** — VEX documents describing whether a CVE actually affects your product.

---

## 14.7 Maturity models — SAMM & BSIMM

Use these to benchmark your programme honestly.

- **OWASP SAMM 2** — 5 business functions (Governance, Design, Implementation, Verification, Operations), each with 3 practices, each with 3 maturity levels. Free, self-assessable.
- **BSIMM** — observational; data from real companies' AppSec programmes. Good for "what do mature programmes actually do."

Pattern: assess → pick 2–3 practices to raise one level → 6-month plan → reassess.

---

## 14.8 Bug bounties & vulnerability disclosure

- **VDP (Vulnerability Disclosure Policy)** — minimum bar. A `security.txt` file + a monitored mailbox. See [disclose.io](https://disclose.io/) for templates.
- **Private bug bounty** — invited researchers; paid.
- **Public bounty** — anyone. Needs triage capacity.
- Indian-origin platforms: Bugcrowd, HackerOne, Intigriti (Europe), YesWeHack.

Before launching public: run through SAMM, have IR tested, have CAPA working, have a lawyer who understands safe-harbour.

---

## 14.9 Common beginner mistakes

- Treating `git push --force` rewrite as a way to remove a secret (already cloned by attackers — **rotate the secret, do not rely on rewrite**).
- Assuming WAF protects against logic bugs — it doesn't.
- "We passed pen-test so we're secure." Pen-test is a snapshot.
- Using HTTPS and calling the app "encrypted end-to-end." TLS is hop-level, not E2E.
- "Open-source is insecure" or "open-source is secure" — neither is true by itself; it depends on maintenance, SBOM, and patch discipline.

---

## 14.10 Interview traps

- *"Difference between SAST, DAST, IAST, SCA?"*
- *"What's the OWASP Top 10 issue most commonly missed by automated scanners?"* (A04 Insecure Design & A01 Broken Access Control — both need humans.)
- *"Walk me through how you'd roll out SBOM generation in a 200-engineer company."*
- *"A dev says Dependabot is too noisy — what do you do?"* (Triage by exploitability/VEX, risk-based SLAs, auto-merge patch bumps for low-risk libs, group PRs, don't just mute.)

---

## 14.11 Mini-exercise (30 min)

Pick any public repo (your own or a sample). Run **three** free tools against it:

1. Semgrep with `p/default` ruleset → SAST findings.
2. Trivy `fs` scan → SCA + secret findings.
3. gitleaks → secret history scan.

Document: top 5 findings, severity, remediation path, and whether each would block release at your hypothetical company (and why).

---

## 14.12 Go deeper

- 🏛 [OWASP Top 10 (Web, API, LLM, Mobile, Proactive Controls, ASVS)](https://owasp.org/projects/)
- 🏛 [OWASP ASVS — Application Security Verification Standard](https://owasp.org/www-project-application-security-verification-standard/)
- 🏛 [OWASP SAMM](https://owaspsamm.org/) · [BSIMM report (annual free PDF)](https://www.blackduck.com/resources/analyst-reports/bsimm-trends-insights.html)
- 🏛 [NIST SSDF — Secure Software Development Framework (SP 800-218)](https://csrc.nist.gov/publications/detail/sp/800-218/final)
- 🏛 [SLSA framework](https://slsa.dev/) · [Sigstore](https://www.sigstore.dev/) · [CycloneDX](https://cyclonedx.org/) · [SPDX](https://spdx.dev/)
- 📘 [PortSwigger Web Security Academy — free, world-class](https://portswigger.net/web-security) · [Google Web Security Fundamentals](https://developers.google.com/web/fundamentals/security)
- 🧪 [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) · [DVWA](https://github.com/digininja/DVWA) · [WebGoat](https://owasp.org/www-project-webgoat/) · [HackTheBox Academy — free tier](https://academy.hackthebox.com/)
- 🧪 [Semgrep Playground](https://semgrep.dev/playground) · [OWASP ZAP](https://www.zaproxy.org/)
- 📰 [GitHub Security Lab](https://securitylab.github.com/) · [Trail of Bits blog](https://blog.trailofbits.com/) · [Snyk Learn](https://learn.snyk.io/)

## Module 14 — Glossary recap

SDLC, Shift-left, SAST, DAST, IAST, SCA, RASP, WAF, Fuzzing, OWASP Top 10, A01–A10, API Top 10, LLM Top 10, BOLA/IDOR, SSRF, Prepared statements, Output encoding, ASVS, SAMM, BSIMM, SSDF, SBOM (SPDX, CycloneDX), SLSA, Sigstore/Cosign, VEX, VDP, Bug bounty, Security.txt, Secrets vault, Dynamic secrets.

→ Next: [Module 15 — Emerging Topics: AI Governance, Zero Trust, PETs, PQC](15-emerging-topics.md)

# Module 14: Application Security (AppSec)

> **Audience:** 🔴 primary · 🟡 concepts only · **Time:** ~80 min · **Prereqs:** Module 2, 13

## Why this matters

Most of what your company sells is software. Bugs in that software produce the largest class of real breaches: SQL injection, auth bypass, broken access control, dependency CVEs. If you work in security or GRC and cannot hold a credible conversation with engineers about AppSec, you will be quietly excluded from the conversations that actually shape the product.

The other thing worth saying up front: AppSec is a relationship game. The tooling matters, but devs ignoring your findings is a bigger problem than the findings themselves.

---

## 14.1 Where AppSec fits in the SDLC

Old model: security tests at the end, blocks the release, everyone hates security. New model: controls at every stage, with most of the friction pushed as early as possible.

| SDLC stage | Security activity |
|-----------|-------------------|
| Design | Threat modelling, privacy review, abuse cases |
| Code | IDE plugins, secure coding training, pre-commit hooks (secrets, lint) |
| Build | SAST, SCA, secrets scan, IaC scan, SBOM |
| Test | DAST, fuzzing, manual pen-test, API scans |
| Release | Signed artefacts, SLSA provenance, release gates |
| Run | RASP/WAF, runtime vuln mgmt, bug bounty, logging and alerting |
| Respond | Incident response, coordinated disclosure |

"Shift left" became a slogan and then a curse word because it was done badly. Shifting left only works when developers trust the tooling. If your SAST tool throws 200 false positives in their first PR, you have lost them for a year.

---

## 14.2 OWASP Top 10: the canonical vulnerability list (Web, 2021)

| # | Category | One-liner |
|---|----------|-----------|
| A01 | **Broken Access Control** | Users can do things they shouldn't. Most common real-breach class. |
| A02 | **Cryptographic Failures** | Weak or missing crypto, hardcoded keys, plaintext passwords. |
| A03 | **Injection** | SQLi, NoSQLi, OS command, LDAP, template injection. |
| A04 | **Insecure Design** | Missing threat modelling. Design flaws cannot be patched later. |
| A05 | **Security Misconfiguration** | Default credentials, verbose errors, exposed admin panels. |
| A06 | **Vulnerable & Outdated Components** | Log4Shell-style. Supply chain. |
| A07 | **Identification & Authentication Failures** | Weak MFA, session fixation, credential stuffing. |
| A08 | **Software & Data Integrity Failures** | Unsigned updates, compromised CI/CD. |
| A09 | **Security Logging & Monitoring Failures** | You cannot detect what you do not log. |
| A10 | **SSRF** | Server tricked into requesting internal URLs. |

### OWASP API Top 10 (2023)

APIs are a separate attack surface and the list differs:

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

The OWASP LLM Top 10 2025 update tightens the language around agentic systems and adds clearer guidance on excessive agency. See Module 15 for the deeper AI security treatment.

---

## 14.3 The testing toolbox: what each acronym means

| Tool type | Finds | How | Stage | Free examples |
|-----------|-------|-----|-------|--------------|
| **SAST** | Code-level flaws | Analyses source code | Pre-merge / CI | **Semgrep**, Bandit, gosec, Brakeman, SonarQube Community |
| **SCA** | Vulnerable dependencies | Parses manifests against CVE DB | CI | **Dependabot**, Renovate, **Trivy**, OWASP Dependency-Check, Grype |
| **DAST** | Runtime web flaws | Crawls and attacks running app | Staging | **OWASP ZAP**, Nikto |
| **IAST** | Runtime from inside | Agent in test app | Staging | Contrast Community (limited) |
| **Secrets scanning** | Keys, tokens | Regex and entropy on code/history | Pre-commit + CI | **gitleaks**, **trufflehog**, GitHub secret scanning |
| **Container scan** | Image CVEs, misconfigs | Scans image layers | CI | **Trivy**, Grype, Clair |
| **IaC scan** | Cloud misconfig in code | Parses Terraform/CFN/K8s | CI | **Checkov**, tfsec, kube-linter |
| **Fuzzing** | Crashes, edge cases | Random or guided inputs | Nightly | AFL++, libFuzzer, OSS-Fuzz |
| **RASP** | Runtime attacks | In-app agent blocks | Prod | Mostly commercial |
| **WAF** | Web attacks at edge | Rules and ML | Prod | ModSecurity + OWASP CRS |
| **API security** | API-specific issues | Traffic learning | Prod | Mostly commercial; APIsec.ai free tier |
| **Pen-test** | Chained real-world attack | Humans | Annually / major release | Engage a firm; CREST-certified in India |

### The real problem is signal-to-noise

The textbook says: turn on SAST, DAST, SCA. The lived reality: every one of these tools produces noise, and dev teams will route around you if the noise dominates the signal.

What actually works:

- Tune rules to your stack before merging anything into CI. Pull out everything that does not apply to your language and frameworks.
- Triage findings yourself for the first month. Calibrate severity to your context, not the tool's default.
- Use VEX or equivalent annotations to mark not-exploitable CVEs explicitly. Do not just mute.
- Auto-merge patch bumps for low-risk libraries. Group PRs for the rest.
- Pick one or two metrics that matter: mean time to remediate by severity, percentage of repos with scanners on. Track them honestly.

Running only SAST and calling the app secure is the most common beginner mistake. SAST finds code flaws, but it misses design flaws, infra flaws, runtime flaws, and most dependency flaws. You need the layered set above, not a single tool.

---

## 14.4 Secure coding fundamentals

### Input

- Validate on a strict allow-list, never a deny-list.
- Parse, do not sanitise. Use typed parsers.

### Output

- Contextual output encoding. HTML, attribute, JS, URL, CSS each need their own encoder.

### Database

- Parameterised queries and prepared statements. Never concatenate SQL strings. There is no exception.

### Authentication

- See Module 3. Use a vetted library (Passport, Spring Security, Devise). Do not invent your own.

### Authorisation

- Centralise. A single policy engine (OPA, Cedar, Oso, or a well-designed custom service) rather than `if user.role == "admin"` scattered across 40 files.
- Test both positive ("can do") and negative ("cannot do what they shouldn't"). The negative tests are the ones that find IDOR.

### Session

- HttpOnly, Secure, SameSite cookies. Short idle timeout. Rotate on privilege change.

### Errors and logs

- No stack traces to users. No secrets in logs. Structured logs with correlation IDs.

### Dependencies

- Lockfiles committed. Automatic updates for security patches. Known-vulnerable versions fail the build, but with an exception path for false positives.

### Cryptography

- See Module 2. Do not roll your own. Use libsodium or platform crypto.

---

## 14.5 Secrets management

- No secrets in code, in committed env files, in Dockerfiles, or in CI logs.
- Use a vault: HashiCorp Vault, AWS Secrets Manager, Azure Key Vault, GCP Secret Manager, Doppler, Infisical.
- Dynamic secrets where supported (DB credentials generated per session).
- Rotation, automated, with the app tolerating mid-session rotation.
- Break-glass paths documented and tested.

A note on accidents: `git push --force` to rewrite history does not remove a leaked secret. Assume it was cloned the moment it was pushed. Rotate the secret. Do not rely on the rewrite.

---

## 14.6 Supply-chain security

Recent incidents (SolarWinds, Log4Shell, the 3CX compromise, the xz-utils backdoor) made this a board-level topic.

- **SBOM (Software Bill of Materials)**: inventory of every component and version. Formats: SPDX, CycloneDX. Generate with Syft or cyclonedx-cli.
- **SLSA (Supply-chain Levels for Software Artifacts)**: levels 1–4 for build integrity.
- Signed artefacts. Sigstore/Cosign sign images and verify on deploy.
- Provenance. Attestations describing how, when, and by whom a build was produced.
- Vulnerability feed. VEX documents describing whether a CVE actually affects your product.

A pragmatic sequence: SBOM first, then signed artefacts, then provenance. SLSA Level 3 in one quarter is a fantasy if you do not have a working build pipeline already.

---

## 14.7 Maturity models: SAMM and BSIMM

Use these to benchmark honestly, not to score-hunt.

- **OWASP SAMM 2**: 5 business functions (Governance, Design, Implementation, Verification, Operations), each with 3 practices and 3 maturity levels. Free and self-assessable.
- **BSIMM**: observational data from real companies' AppSec programmes. Good for "what do mature programmes actually do."

Working pattern: assess, pick 2 to 3 practices to raise one level, plan for 6 months, reassess. Anything more ambitious slips.

---

## 14.8 Bug bounties and vulnerability disclosure

- **VDP (Vulnerability Disclosure Policy)**: the minimum bar. A `security.txt` file plus a monitored mailbox. See [disclose.io](https://disclose.io/) for templates.
- **Private bug bounty**: invited researchers, paid.
- **Public bounty**: anyone. Needs serious triage capacity.
- Platforms: Bugcrowd, HackerOne, Intigriti (Europe), YesWeHack.

Do not launch public before you have a VDP, IR tested, a triage rota, and a lawyer who understands safe-harbour clauses. Public bounty without these is a self-inflicted DoS on your security team.

---

## 14.9 Common beginner mistakes

- Thinking `git push --force` removes a leaked secret. It does not.
- Assuming the WAF protects against logic bugs. It does not.
- "We passed pen-test so we are secure." Pen-test is a snapshot, often a narrow one.
- Calling the app "end-to-end encrypted" because it uses HTTPS. TLS is hop-level, not E2E.
- "Open source is insecure" or "open source is secure." Neither is true by itself. It depends on maintenance, SBOM, and patch discipline.
- Treating Dependabot noise by muting instead of triaging.

---

## 14.10 Interview traps

- "Difference between SAST, DAST, IAST, SCA?"
- "Which OWASP Top 10 issue is most commonly missed by automated scanners?" Usually A04 Insecure Design and A01 Broken Access Control. Both need humans.
- "Walk me through how you would roll out SBOM generation in a 200-engineer company."
- "A dev says Dependabot is too noisy. What do you do?" The right answer is triage by exploitability or VEX, risk-based SLAs, auto-merge patch bumps for low-risk libraries, group PRs. Do not just mute it.

---

## 14.11 Mini-exercise (30 min)

Pick any public repo, your own or a sample. Run three free tools against it:

1. Semgrep with the `p/default` ruleset for SAST findings.
2. Trivy `fs` scan for SCA and secret findings.
3. gitleaks for a secret history scan.

Document: top 5 findings, severity, remediation path, and whether each would block release at your hypothetical company and why. The "why" matters more than the "what."

---

## 14.12 Go deeper

- 🏛 [OWASP Top 10 (Web, API, LLM, Mobile, Proactive Controls, ASVS)](https://owasp.org/projects/)
- 🏛 [OWASP ASVS: Application Security Verification Standard](https://owasp.org/www-project-application-security-verification-standard/)
- 🏛 [OWASP SAMM](https://owaspsamm.org/) · [BSIMM report (annual free PDF)](https://www.blackduck.com/resources/analyst-reports/bsimm-trends-insights.html)
- 🏛 [NIST SSDF: Secure Software Development Framework (SP 800-218)](https://csrc.nist.gov/publications/detail/sp/800-218/final)
- 🏛 [SLSA framework](https://slsa.dev/) · [Sigstore](https://www.sigstore.dev/) · [CycloneDX](https://cyclonedx.org/) · [SPDX](https://spdx.dev/)
- 📘 [PortSwigger Web Security Academy: free, world-class](https://portswigger.net/web-security) · [Google Web Security Fundamentals](https://developers.google.com/web/fundamentals/security)
- 🧪 [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) · [DVWA](https://github.com/digininja/DVWA) · [WebGoat](https://owasp.org/www-project-webgoat/) · [HackTheBox Academy: free tier](https://academy.hackthebox.com/)
- 🧪 [Semgrep Playground](https://semgrep.dev/playground) · [OWASP ZAP](https://www.zaproxy.org/)
- 📰 [GitHub Security Lab](https://securitylab.github.com/) · [Trail of Bits blog](https://blog.trailofbits.com/) · [Snyk Learn](https://learn.snyk.io/)

## Module 14: Glossary recap

SDLC, Shift-left, SAST, DAST, IAST, SCA, RASP, WAF, Fuzzing, OWASP Top 10, A01–A10, API Top 10, LLM Top 10, BOLA/IDOR, SSRF, Prepared statements, Output encoding, ASVS, SAMM, BSIMM, SSDF, SBOM (SPDX, CycloneDX), SLSA, Sigstore/Cosign, VEX, VDP, Bug bounty, Security.txt, Secrets vault, Dynamic secrets.

→ Next: [Module 15: Emerging Topics: AI Governance, Zero Trust, PETs, PQC](15-emerging-topics.md)

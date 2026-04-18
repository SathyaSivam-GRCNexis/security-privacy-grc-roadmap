# Module 1 — Security First Principles

> **Audience:** 🟢 🟡 🔴 · **Time:** ~90 min · **Prereqs:** Module 0

## Why this matters

Tools and technologies change every year. **Principles don't.** Every good security decision — from a CEO's budget call to an engineer's code review — is a principle applied to a specific situation. Learn the principles and you'll think like a security professional even when the tech is new.

By the end of this module you will know:
- The **CIA triad** and why it's not enough alone.
- The difference between **threats, vulnerabilities, risks, and controls** (the four words people mix up every day).
- **Threat modelling** — how professionals systematically find security issues *before* they ship.
- The classic **security design principles** (least privilege, defence in depth, fail-secure, etc.).

---

## 1.1 The CIA triad — the ABC of security

Every security concept maps back to one of these three letters.

- **Confidentiality** — only authorised people/systems can see the data.
- **Integrity** — the data is what it should be; it hasn't been tampered with.
- **Availability** — the data and service are there when legitimate users need them.

### Real-world examples

| Incident | Which pillar failed? |
|----------|---------------------|
| Customer database leaked publicly | **Confidentiality** |
| Bank transfer amount altered mid-flight | **Integrity** |
| Website taken down by DDoS | **Availability** |
| Ransomware encrypts files | **Availability** (primary) + **Integrity** |
| Employee deletes records accidentally | **Integrity** + **Availability** |

Notice many incidents break *multiple* pillars. That's normal.

### The analogy: a hospital

- **Confidentiality:** only the assigned doctor sees your medical record.
- **Integrity:** your allergy list is accurate; nobody secretly edits it.
- **Availability:** the record is accessible 24×7 to treat you in an emergency.

A hospital that fails *any* of these fails its patients.

### Extensions you'll see in interviews

- **AAA** — **Authentication** (who are you?), **Authorisation** (what can you do?), **Accounting** (what did you do? — audit logs).
- **Non-repudiation** — you can't plausibly deny that you did an action (signatures, audit trails).
- **Parkerian Hexad** — adds **Possession/Control**, **Authenticity**, and **Utility** to the CIA triad. More nuanced, less commonly tested.

### Common beginner mistakes

- Forgetting **availability** is security. Many engineers think encryption = security. A system nobody can use is not secure — it's dead.
- Thinking "integrity" is about encryption. It's about **detecting change** — hashes, signatures, versioning, tamper-evident logs.

### Interview trap

- **Q:** "A database is encrypted at rest. Is the data secure?"
  **A:** Confidentiality is partly addressed (only if keys are well managed). Integrity and availability are not addressed at all by encryption. Backups, access logging, tamper detection, and uptime design are separate.

---

## 1.2 Threat vs Vulnerability vs Risk vs Control (say it out loud five times)

You learned these in Module 0 — here they get practised until reflexive.

### The four words, one paragraph

> A **threat** is a potential cause of harm. A **vulnerability** is a weakness that could be exploited by a threat. The combination produces a **risk**: the expected negative outcome. A **control** is anything you put in place to reduce the likelihood or impact of that risk.

### The airport analogy

- **Threat** — a person who wants to smuggle a weapon.
- **Vulnerability** — a gap in the security screening process.
- **Risk** — a weapon reaching the plane and being used mid-flight.
- **Control** — metal detectors, baggage X-ray, trained staff, intelligence lists, reinforced cockpit doors.

Each control reduces either the *likelihood* (harder to sneak through) or the *impact* (even if they get in, the cockpit is locked).

### The classification of controls (learn these; appear constantly)

By **function**:
- **Preventive** — stop the bad thing from happening (lock on the door).
- **Detective** — notice it happened (CCTV).
- **Corrective** — fix the damage (restore from backup).
- **Deterrent** — discourage attempts ("Guard dog on duty" sign).
- **Compensating** — an alternative when the "right" control isn't feasible.
- **Recovery** — restore operations (DR site).

By **type**:
- **Administrative** — policies, training, background checks.
- **Technical / logical** — firewalls, encryption, MFA.
- **Physical** — guards, fences, biometric doors.

Auditors love to ask "what type of control is *X*?" Internalise this 2×N matrix.

### Worked example

Control: "All employee laptops must have full-disk encryption."

- Function: **Preventive** (a thief can't read the disk).
- Type: **Technical**.
- Layer it sits in: **Endpoint**.
- Which pillar of CIA? **Confidentiality** (primarily).

### Common beginner mistakes

- Reporting "our vulnerability scanner flagged 1,000 risks." They aren't risks, they're *findings* that may or may not translate to risks depending on context.
- Confusing **compensating** controls with **alternative** controls. A compensating control deliberately replaces a required one because the required one can't be implemented; it must be *equivalent* in strength.

### Mini-exercise (10 min)

List 5 controls in your own home. Classify each by function (preventive/detective/etc.) and type (admin/technical/physical).

---

## 1.3 Threat modelling — thinking like an attacker, systematically

Threat modelling is the practice of **finding security issues in a system by structured brainstorming, before the attacker finds them**. You'll see this skill on every modern security job description.

### Shostack's four questions (memorise these)

1. **What are we building?** — draw the system.
2. **What can go wrong?** — enumerate threats.
3. **What are we going to do about it?** — propose mitigations.
4. **Did we do a good job?** — validate.

If you remember nothing else from this module, remember these four questions. You can threat-model a house, an app, an office process, an AI agent — same four questions.

### STRIDE — the most common threat taxonomy

Invented by Microsoft. Six letters, six threat categories, each maps to a CIA/AAA property it breaks.

| Letter | Threat | What it breaks | Example |
|--------|--------|----------------|---------|
| **S** | **Spoofing** | Authentication | Pretending to be another user |
| **T** | **Tampering** | Integrity | Altering data in transit or at rest |
| **R** | **Repudiation** | Non-repudiation | "I never sent that transaction" |
| **I** | **Information disclosure** | Confidentiality | Data leaks, log exposures |
| **D** | **Denial of service** | Availability | DDoS, resource exhaustion |
| **E** | **Elevation of privilege** | Authorisation | Normal user becoming admin |

When you see any design, walk through each letter and ask: *"How could this happen here?"*

### Worked example — threat-modelling a login page

> System: users submit email + password to `/login`; server checks against a hashed password in a database; if correct, sets a session cookie.

Apply STRIDE:

- **S (Spoofing):** Can an attacker log in as someone else? → phishing, credential stuffing, session-cookie theft. *Controls:* MFA, rate limiting, secure cookies, short sessions.
- **T (Tampering):** Can the attacker tamper with the request mid-flight? → insecure HTTP, tampered cookie. *Controls:* HTTPS, signed/HMAC cookies.
- **R (Repudiation):** Can a user deny they logged in? → no audit log. *Control:* immutable log of login events.
- **I (Info disclosure):** Could server leak info? → verbose error messages ("Wrong password" vs "Wrong email" lets attackers enumerate users). *Control:* generic errors.
- **D (DoS):** Can attacker lock out real users? → brute force triggering account lockouts. *Control:* progressive delays instead of hard lockouts; CAPTCHAs.
- **E (Elevation):** Can a logged-in user act as admin? → missing authorisation checks on admin endpoints. *Control:* server-side role checks on every admin request.

Six categories, six questions, eight concrete mitigations. That's a threat model.

### Other taxonomies you'll meet

- **DREAD** — a scoring model (Damage, Reproducibility, Exploitability, Affected users, Discoverability). Often criticised for being subjective.
- **PASTA** — Process for Attack Simulation and Threat Analysis. Heavier, attacker-centric, used in larger enterprises.
- **LINDDUN** — privacy-focused threat modelling (Linkability, Identifiability, Non-repudiation, Detectability, Disclosure, Unawareness, Non-compliance).
- **Attack trees** — graphical, goal-driven decomposition.
- **Kill chain / MITRE ATT&CK** — attacker-behaviour-focused; useful after threats, during detection engineering.

Start with STRIDE. Add the others when specific situations call for them (LINDDUN for privacy reviews, PASTA for big regulated products).

### When to threat-model

- On a new feature that touches auth, data, or integrations.
- On any change to trust boundaries (new third party, new network zone, new user type).
- On any design with new tech (AI agents, new protocols, new regions).
- Annually for critical existing systems.

### Common beginner mistakes

- Treating threat modelling as a document exercise. The *conversation* is the deliverable. A 30-minute whiteboard session with devs finds more than a 20-page report nobody reads.
- Enumerating every conceivable threat. You want *relevant* ones. Time-box.
- Skipping validation (question 4). Revisit the threat model after build — did the mitigations actually land?

### Interview trap

- **Q:** "Walk me through how you'd threat-model a payment API."
  Good answer: draw it on a whiteboard (users, API, DB, payment processor), identify trust boundaries, go STRIDE on each data flow, produce a prioritised mitigation list. Mentioning "trust boundaries" early is a signal of maturity.

### Mini-exercise (20 min)

Pick the EdTech platform's quiz feature (user submits answers, server scores them, leaderboard shows top scores). Draw a data-flow diagram in [draw.io](https://app.diagrams.net/). Run STRIDE on each flow. Produce a table of 6–10 threats with one mitigation each.

### Go deeper

- 📘 [Shostack — Threat Modeling resources](https://shostack.org/resources/threat-modeling) (the author of the canonical book, generous with free material).
- 📰 [Microsoft — STRIDE introduction](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats)
- 🧪 [OWASP Threat Dragon](https://owasp.org/www-project-threat-dragon/) — free, open-source modelling tool.
- 🧪 [Microsoft Threat Modeling Tool (free)](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool).
- 📰 [LINDDUN privacy threat modelling](https://linddun.org/).
- 📰 [MITRE ATT&CK](https://attack.mitre.org/) — an encyclopedia of real attacker techniques, organised by tactic. Bookmark it.

---

## 1.4 Classic security design principles

These are 50-year-old ideas and they still win audits. You should be able to recite and apply them.

### 1. Least privilege

Every user, service, or process gets the **minimum** permissions needed to do its job — and no more.

*Example:* A monitoring service that only needs to read logs should not have write access. A marketing analyst who needs customer emails doesn't need the ability to delete them.

### 2. Separation of duties (SoD)

No single person should be able to complete a high-risk action from start to finish. Example: the person who *requests* a payment shouldn't be the same person who *approves* it.

### 3. Defence in depth

Assume any single control will eventually fail. Layer them so one failure isn't catastrophic.

*Example layering for a web app:* WAF → rate limiting → input validation → parameterised queries → least-privileged DB account → encryption at rest → monitoring & alerts.

### 4. Fail securely / fail-safe

When a component fails, it should fail in a **safe** state (deny by default), not an open state.

*Anti-pattern:* "If auth service is down, let the user through." That's convenient and catastrophic.

### 5. Secure by default

Out-of-the-box configuration must be the safe one. A new S3 bucket should be *private*, not public. A new user should have *no* permissions, not admin.

### 6. Simplicity (KISS) — complexity is the enemy of security

Every added moving part is a potential vulnerability. Simpler systems have fewer hiding places for bugs.

### 7. Open design / no security through obscurity

Don't rely on keeping the *mechanism* secret. Rely on keeping **secrets** secret (keys, passwords). The algorithm can be public; the key must not be. Kerckhoffs's principle, from 1883 and still correct.

### 8. Complete mediation

Check permissions on **every** access, not just the first. Users' roles can change. An access that was valid a minute ago might not be valid now.

### 9. Psychological acceptability

Security that's too annoying gets bypassed. Forcing password rotation every 30 days leads to `Summer2025!`, `Summer2026!`. Usable security is secure security.

### 10. Economy of mechanism

Reuse well-tested components (mature libraries, standard crypto) rather than inventing your own. "Don't roll your own crypto."

### 11. Minimise attack surface

Every open port, endpoint, feature flag, or exposed service is an attack surface. If you don't need it, remove it.

### Worked example — applying principles

Imagine you're designing an admin dashboard for the EdTech platform.

- **Least privilege:** admins have read-only by default; write-admins are a separate tightly-controlled role.
- **SoD:** deleting a course requires a peer approval; the deleter and approver can't be the same person.
- **Defence in depth:** VPN-only access → SSO with MFA → per-action permission checks → audit log → anomaly alerts.
- **Fail securely:** if the permission service is unreachable, deny the action.
- **Secure by default:** new admin accounts start with no roles; roles must be explicitly granted.
- **Simplicity:** one permission model (RBAC), not five competing ones.
- **Complete mediation:** each API call re-checks permissions from a central service; no caching of "admin since 9 AM."
- **Minimise attack surface:** admin dashboard is on a separate subdomain behind VPN, not exposed to the public internet.

You've just designed something that would pass most audits — from principles alone.

### Common beginner mistakes

- Treating principles as abstract slogans. Pair each principle with a specific **control** in your design, always.
- Over-rotating on one principle at the expense of others (e.g., locking down so hard that nobody can work — that breaks *psychological acceptability*).

### Interview trap

- **Q:** "Your dev team wants admin access to production 'for debugging'. What do you do?"
  Good answer: *least privilege* + *SoD* + *complete mediation* + *accounting*. Grant **just-in-time** elevated access via an approval flow, time-bounded, fully logged. Don't let anyone sit on standing prod admin unless absolutely required; if so, it must be break-glass, alerted, and reviewed.

### Mini-exercise (15 min)

Take *any* system you use at work (ticketing, HR portal, the EdTech CMS). Evaluate it against these 11 principles. Score each from 1 (broken) to 5 (excellent). Identify the two worst-scoring and write one concrete improvement for each.

### Go deeper

- 🏛 [Saltzer & Schroeder — The Protection of Information in Computer Systems (1975)](https://web.mit.edu/Saltzer/www/publications/protection/) — the original paper. A bit formal, still the reference.
- 🏛 [NIST SP 800-12 Rev. 1 — Intro to Info Security](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-12r1.pdf)
- 🏛 [CISA — Secure by Design principles](https://www.cisa.gov/securebydesign)
- 📰 [OWASP — Security by Design principles](https://owasp.org/www-project-developer-guide/draft/foundations/security_principles/)

---

## Module 1 — Glossary recap

CIA triad, Confidentiality, Integrity, Availability, AAA, Authentication, Authorisation, Accounting, Non-repudiation, Parkerian Hexad, Preventive/Detective/Corrective/Deterrent/Compensating/Recovery controls, Administrative/Technical/Physical controls, Threat modelling, Shostack's 4 questions, STRIDE, DREAD, PASTA, LINDDUN, Attack trees, MITRE ATT&CK, Trust boundary, Least privilege, Separation of duties, Defence in depth, Fail securely, Secure by default, Complete mediation, Kerckhoffs's principle, Attack surface.

→ Next: [Module 2 — Cryptography for Humans](02-cryptography-for-humans.md)

# Module 1, Security First Principles

> **Audience:** 🟢 🟡 🔴 · **Time:** ~90 min · **Prereqs:** Module 0

## Why this matters

Tools and technologies change every year. Principles don't. Every good security decision, from a CEO's budget call to an engineer's pull-request review, is a principle applied to a specific situation. Learn the principles and you'll think like a security professional even when the tech is new and the vendors are loud.

By the end of this module you will know:
- The **CIA triad** and why it's not enough on its own.
- The difference between **threats, vulnerabilities, risks and controls**. The four words people mix up every day.
- **Threat modelling.** How professionals systematically find security issues before they ship.
- The classic **security design principles** (least privilege, defence in depth, fail-secure, and friends).

---

## 1.1 The CIA triad, the ABC of security

Every security concept maps back to one of these three letters. Textbooks treat the triad as the goal. In practice, it's a checklist you run silently in your head while listening to an architecture review.

- **Confidentiality.** Only authorised people or systems can see the data.
- **Integrity.** The data is what it should be. Nobody tampered with it.
- **Availability.** The data and service are there when legitimate users need them.

### Real-world examples

| Incident | Which pillar failed? |
|----------|---------------------|
| Customer database leaked publicly | **Confidentiality** |
| Bank transfer amount altered mid-flight | **Integrity** |
| Website taken down by DDoS | **Availability** |
| Ransomware encrypts files | **Availability** (primary) and **Integrity** |
| Employee deletes records accidentally | **Integrity** and **Availability** |

Notice that most real incidents break multiple pillars. That's normal. If a colleague tells you a breach hit "only" confidentiality, push back gently.

### The hospital analogy

- **Confidentiality.** Only the assigned doctor sees your medical record.
- **Integrity.** Your allergy list is accurate. Nobody secretly edits it.
- **Availability.** The record is accessible 24×7 to treat you in an emergency.

A hospital that fails any of these fails its patients.

### Extensions you'll see in interviews

- **AAA.** Authentication (who are you?), Authorisation (what can you do?), Accounting (what did you do? Audit logs).
- **Non-repudiation.** You can't plausibly deny that you did an action. Signatures, audit trails.
- **Parkerian Hexad.** Adds Possession/Control, Authenticity and Utility to the CIA triad. More nuanced, less commonly tested. Drop the name in if someone asks "what's after CIA?"

### What beginners typically miss

- Forgetting availability is security. Many engineers think encryption equals security. A system nobody can use is not secure. It's dead.
- Thinking "integrity" is about encryption. It's about detecting change. Hashes, signatures, versioning, tamper-evident logs.

In practice, availability is where security and SRE overlap, and where political turf wars start. Pick your battles.

### Interview trap

- **Q:** "A database is encrypted at rest. Is the data secure?"
  **A:** Confidentiality is partly addressed, and only if keys are well managed. Integrity and availability are not addressed at all by encryption. Backups, access logging, tamper detection and uptime design are separate problems.

---

## 1.2 Threat vs Vulnerability vs Risk vs Control (say it out loud five times)

You learned these in Module 0. Here they get practised until reflexive. People who get this wrong in audit calls lose credibility fast.

### The four words, one paragraph

> A **threat** is a potential cause of harm. A **vulnerability** is a weakness that could be exploited by a threat. The combination produces a **risk**: the expected negative outcome. A **control** is anything you put in place to reduce the likelihood or impact of that risk.

### The airport analogy

- **Threat.** A person who wants to smuggle a weapon.
- **Vulnerability.** A gap in the screening process.
- **Risk.** A weapon reaching the plane and being used mid-flight.
- **Control.** Metal detectors, baggage X-ray, trained staff, intelligence lists, reinforced cockpit doors.

Each control reduces either the likelihood (harder to sneak through) or the impact (even if they get in, the cockpit is locked).

### The classification of controls (learn these, they come up constantly)

By **function**:
- **Preventive.** Stop the bad thing from happening (lock on the door).
- **Detective.** Notice it happened (CCTV).
- **Corrective.** Fix the damage (restore from backup).
- **Deterrent.** Discourage attempts ("Guard dog on duty" sign).
- **Compensating.** An alternative when the right control isn't feasible.
- **Recovery.** Restore operations (DR site).

By **type**:
- **Administrative.** Policies, training, background checks.
- **Technical / logical.** Firewalls, encryption, MFA.
- **Physical.** Guards, fences, biometric doors.

Auditors love to ask "what type of control is X?" Internalise the matrix.

### Worked example

Control: "All employee laptops must have full-disk encryption."

- Function: **Preventive** (a thief can't read the disk).
- Type: **Technical**.
- Layer it sits in: **Endpoint**.
- Which pillar of CIA? **Confidentiality**, primarily.

### What beginners typically miss

- Reporting "our vulnerability scanner flagged 1,000 risks." They aren't risks. They're findings, and most won't translate to risk after context.
- Confusing compensating controls with alternative controls. A compensating control deliberately replaces a required one that can't be implemented, and it must be equivalent in strength. Auditors check this carefully.

### Mini-exercise (10 min)

List five controls in your own home. Classify each by function (preventive/detective/etc.) and type (admin/technical/physical).

---

## 1.3 Threat modelling, thinking like an attacker, systematically

Threat modelling is the practice of finding security issues in a system by structured brainstorming, before the attacker finds them. It's on every modern security job description. It's also the skill most people fake in interviews.

Sequencing tip: learn STRIDE first. Then learn the four questions. Then practice. You'll see other taxonomies (LINDDUN, PASTA), but if you can't run STRIDE on a login page from memory, the others won't save you.

### Shostack's four questions (memorise these)

1. **What are we building?** Draw the system.
2. **What can go wrong?** Enumerate threats.
3. **What are we going to do about it?** Propose mitigations.
4. **Did we do a good job?** Validate.

If you remember nothing else, remember these four questions. You can threat-model a house, an app, an office process, an AI agent. Same four questions.

### STRIDE, the most common threat taxonomy

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

### Worked example, threat-modelling a login page

> System: users submit email and password to `/login`. Server checks against a hashed password in a database. If correct, sets a session cookie.

Apply STRIDE:

- **S (Spoofing).** Can an attacker log in as someone else? Phishing, credential stuffing, session-cookie theft. *Controls:* MFA, rate limiting, secure cookies, short sessions.
- **T (Tampering).** Can the attacker tamper with the request mid-flight? Insecure HTTP, tampered cookie. *Controls:* HTTPS, signed/HMAC cookies.
- **R (Repudiation).** Can a user deny they logged in? No audit log. *Control:* immutable log of login events.
- **I (Info disclosure).** Could the server leak info? Verbose error messages ("Wrong password" vs "Wrong email" lets attackers enumerate users). *Control:* generic errors.
- **D (DoS).** Can an attacker lock out real users? Brute force triggering account lockouts. *Control:* progressive delays instead of hard lockouts. CAPTCHAs.
- **E (Elevation).** Can a logged-in user act as admin? Missing authorisation checks on admin endpoints. *Control:* server-side role checks on every admin request.

Six categories, six questions, eight concrete mitigations. That's a threat model.

### Other taxonomies you'll meet

- **DREAD.** A scoring model (Damage, Reproducibility, Exploitability, Affected users, Discoverability). Often criticised for being subjective.
- **PASTA.** Process for Attack Simulation and Threat Analysis. Heavier, attacker-centric, used in larger enterprises.
- **LINDDUN.** Privacy-focused threat modelling (Linkability, Identifiability, Non-repudiation, Detectability, Disclosure, Unawareness, Non-compliance).
- **Attack trees.** Graphical, goal-driven decomposition.
- **Kill chain / MITRE ATT&CK.** Attacker-behaviour-focused, useful after threats, during detection engineering.

Strong opinion: start with STRIDE and stay there until you've done at least ten. Add the others when specific situations call for them (LINDDUN for privacy reviews, PASTA for big regulated products). Most teams who claim to "do PASTA" are doing STRIDE with a longer document.

### When to threat-model

- On a new feature that touches auth, data or integrations.
- On any change to trust boundaries. New third party, new network zone, new user type.
- On any design with new tech. AI agents, new protocols, new regions.
- Annually for critical existing systems.

### What beginners typically miss

- Treating threat modelling as a document exercise. The conversation is the deliverable. A 30-minute whiteboard session with devs finds more than a 20-page report nobody reads. In one SaaS environment I worked in, replacing the giant Word template with a 1-page whiteboard photo doubled the number of models produced per quarter.
- Enumerating every conceivable threat. You want the relevant ones. Time-box.
- Skipping validation (question 4). Revisit the threat model after build. Did the mitigations actually land?

### Interview trap

- **Q:** "Walk me through how you'd threat-model a payment API."
  Good answer: draw it on a whiteboard (users, API, DB, payment processor), identify trust boundaries, go STRIDE on each data flow, produce a prioritised mitigation list. Mentioning "trust boundaries" early is a signal of maturity.

### Mini-exercise (20 min)

Pick the EdTech platform's quiz feature (user submits answers, server scores them, leaderboard shows top scores). Draw a data-flow diagram in [draw.io](https://app.diagrams.net/). Run STRIDE on each flow. Produce a table of 6–10 threats with one mitigation each.

### Go deeper

- 📘 [Shostack, Threat Modeling resources](https://shostack.org/resources/threat-modeling). The author of the canonical book, generous with free material.
- 📰 [Microsoft, STRIDE introduction](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats)
- 🧪 [OWASP Threat Dragon](https://owasp.org/www-project-threat-dragon/). Free, open-source modelling tool.
- 🧪 [Microsoft Threat Modeling Tool (free)](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool).
- 📰 [LINDDUN privacy threat modelling](https://linddun.org/).
- 📰 [MITRE ATT&CK](https://attack.mitre.org/). An encyclopedia of real attacker techniques, organised by tactic. Bookmark it.

---

## 1.4 Classic security design principles

These are 50-year-old ideas and they still win audits. You should be able to recite and apply them.

### 1. Least privilege

Every user, service or process gets the minimum permissions needed to do its job, and no more.

*Example.* A monitoring service that only needs to read logs should not have write access. A marketing analyst who needs customer emails doesn't need the ability to delete them.

Reality check: least privilege is easy to write in a policy and painful to enforce. Standing admin rights creep back into production faster than you can disable them. Plan for ongoing access reviews, not a one-off cleanup.

### 2. Separation of duties (SoD)

No single person should be able to complete a high-risk action from start to finish. Example: the person who requests a payment shouldn't be the same person who approves it.

### 3. Defence in depth

Assume any single control will eventually fail. Layer them so one failure isn't catastrophic.

*Example layering for a web app.* WAF, then rate limiting, then input validation, then parameterised queries, then a least-privileged DB account, then encryption at rest, then monitoring and alerts.

The textbook version sounds tidy. In practice, defence in depth means each layer is owned by a different team and someone has to chase all of them. Common pattern: the WAF is owned by infra, validation by devs, monitoring by SOC. Nobody owns "is the chain end-to-end working?". That's your job.

### 4. Fail securely / fail-safe

When a component fails it should fail in a safe state (deny by default), not an open state.

*Anti-pattern.* "If the auth service is down, let the user through." Convenient and catastrophic.

### 5. Secure by default

Out-of-the-box configuration must be the safe one. A new S3 bucket should be private, not public. A new user should have no permissions, not admin.

### 6. Simplicity (KISS)

Complexity is the enemy of security. Every added moving part is a potential vulnerability. Simpler systems have fewer hiding places for bugs.

### 7. Open design, no security through obscurity

Don't rely on keeping the mechanism secret. Rely on keeping secrets secret (keys, passwords). The algorithm can be public. The key must not be. Kerckhoffs's principle, from 1883, still correct.

### 8. Complete mediation

Check permissions on every access, not just the first. Users' roles can change. An access that was valid a minute ago might not be valid now.

### 9. Psychological acceptability

Security that's too annoying gets bypassed. Forcing password rotation every 30 days leads to `Summer2025!`, `Summer2026!`. Usable security is secure security.

### 10. Economy of mechanism

Reuse well-tested components (mature libraries, standard crypto) rather than inventing your own. "Don't roll your own crypto."

### 11. Minimise attack surface

Every open port, endpoint, feature flag or exposed service is an attack surface. If you don't need it, remove it.

### Worked example, applying principles

Imagine you're designing an admin dashboard for an EdTech platform.

- **Least privilege.** Admins have read-only by default. Write-admins are a separate tightly-controlled role.
- **SoD.** Deleting a course requires a peer approval. The deleter and approver can't be the same person.
- **Defence in depth.** VPN-only access, SSO with MFA, per-action permission checks, audit log, anomaly alerts.
- **Fail securely.** If the permission service is unreachable, deny the action.
- **Secure by default.** New admin accounts start with no roles. Roles must be explicitly granted.
- **Simplicity.** One permission model (RBAC), not five competing ones.
- **Complete mediation.** Each API call re-checks permissions from a central service. No caching of "admin since 9 AM."
- **Minimise attack surface.** Admin dashboard is on a separate subdomain behind VPN, not exposed to the public internet.

You've just designed something that would pass most audits, from principles alone.

### What beginners typically miss

- Treating principles as abstract slogans. Pair each principle with a specific control in your design, always.
- Over-rotating on one principle at the expense of others. Locking down so hard that nobody can work breaks psychological acceptability.

### Interview trap

- **Q:** "Your dev team wants admin access to production for debugging. What do you do?"
  Good answer: least privilege plus SoD plus complete mediation plus accounting. Grant just-in-time elevated access via an approval flow, time-bounded, fully logged. Don't let anyone sit on standing prod admin unless absolutely required. If so, it must be break-glass, alerted and reviewed.

### Mini-exercise (15 min)

Take any system you use at work (ticketing, HR portal, the EdTech CMS). Evaluate it against these 11 principles. Score each from 1 (broken) to 5 (excellent). Identify the two worst-scoring and write one concrete improvement for each.

### Go deeper

- 🏛 [Saltzer & Schroeder, The Protection of Information in Computer Systems (1975)](https://web.mit.edu/Saltzer/www/publications/protection/). The original paper. A bit formal, still the reference.
- 🏛 [NIST SP 800-12 Rev. 1, Intro to Info Security](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-12r1.pdf)
- 🏛 [CISA, Secure by Design principles](https://www.cisa.gov/securebydesign)
- 📰 [OWASP, Security by Design principles](https://owasp.org/www-project-developer-guide/draft/foundations/security_principles/)

---

## Module 1, Glossary recap

CIA triad, Confidentiality, Integrity, Availability, AAA, Authentication, Authorisation, Accounting, Non-repudiation, Parkerian Hexad, Preventive/Detective/Corrective/Deterrent/Compensating/Recovery controls, Administrative/Technical/Physical controls, Threat modelling, Shostack's 4 questions, STRIDE, DREAD, PASTA, LINDDUN, Attack trees, MITRE ATT&CK, Trust boundary, Least privilege, Separation of duties, Defence in depth, Fail securely, Secure by default, Complete mediation, Kerckhoffs's principle, Attack surface.

→ Next: [Module 2, Cryptography for Humans](02-cryptography-for-humans.md)

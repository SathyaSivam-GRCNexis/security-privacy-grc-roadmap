# Module 0 — Foundation Pack (Zero to Literate)

> **Audience:** 🟢 Non-tech · 🟡 Career-switcher · 🔴 Tech
> **Time:** ~90 minutes · **Prereqs:** none

## Why this matters

Most security/privacy/GRC courses assume you already speak the language. This module assumes **nothing**. By the end, you will:

- Tell the difference between *security*, *privacy*, *compliance*, and *GRC* — and stop using them interchangeably.
- Read a news headline about a data breach and understand what actually happened.
- Know the basic plumbing of the internet well enough that no future module feels magical.
- Speak the language of **risk** (which is the language security people actually use at work).

---

## 0.1 The 4-box model: Security vs Privacy vs Compliance vs GRC

People use these words as if they mean the same thing. They do not. Getting this right on day one saves you months of confusion.

### The paragraph definition

- **Security** is about *protecting* systems and data from unauthorised access, modification, or destruction. It answers: *"Can a bad actor get in?"*
- **Privacy** is about *respecting* how **personal** data is collected, used, shared, and retained. It answers: *"Should we even have this data, are we honest about it, and do individuals still have rights over it?"*
- **Compliance** is about *proving* you meet a specific external rulebook — a law (like GDPR), a standard (like ISO 27001), or a contract (like your big customer's security addendum). It answers: *"Can you show the auditor?"*
- **GRC** — Governance, Risk, Compliance — is the **management discipline** that sits above all three. Governance decides the direction and accountability; Risk decides what to worry about and how much; Compliance keeps you on the rulebook. GRC is the committee; security and privacy are the builders.

### The house analogy

> Imagine your data is a house.
>
> - **Security** is the lock on the door, the alarm, the reinforced glass. It stops intruders.
> - **Privacy** is the *promise* you made to the people whose things are stored inside — "I won't rummage through your suitcase; I won't let my cousin borrow your watch."
> - **Compliance** is the government inspection certificate on the wall that says "this building passed the fire-safety audit on 12th March."
> - **GRC** is the building-management committee that decided you needed a lock, a privacy promise, and an inspector in the first place — and allocated the budget for all three.

### Worked example: a fitness app leaks user workouts

- **Security failure?** Yes — their database was publicly exposed.
- **Privacy failure?** Also yes — they had promised workouts wouldn't be shared with third parties.
- **Compliance failure?** Possibly — GDPR, HIPAA (if US health), and DPDP Act (if Indian users) may all have been breached.
- **GRC failure?** Definitely — *nobody* in management had decided "what's our policy if a developer accidentally makes a bucket public?"

One event; four different lenses. Your job in this field is to recognise *all four* and know which hat you're wearing.

### Common beginner mistakes

- Saying "we're GDPR compliant" when you mean "we have a cookie banner." Compliance is a **system of proof**, not a checkbox.
- Thinking privacy is only about personal data *leakage*. It's also about **collection** (did you even need it?), **use** (did you tell them?), and **retention** (are you keeping it forever without reason?).
- Thinking security = IT's problem and privacy = legal's problem. They both have business, legal, IT, HR, and vendor components.

### Interview traps

- **Q:** "We're encrypting everything — are we privacy-compliant?"
  **A:** No. Encryption is a *security* control. Privacy asks whether you should have collected the data, whether users know, and whether they can delete it. Encryption is necessary, never sufficient.

### Mini-exercise (10 min)

Pick one app or website you used today. Write one sentence each for:
- A **security** question you'd ask its engineers.
- A **privacy** question you'd ask its product team.
- A **compliance** question you'd ask its legal team.

### Go deeper

- 🏛 [NIST Glossary — csrc.nist.gov/glossary](https://csrc.nist.gov/glossary) — authoritative definitions of every term you'll meet.
- 📰 [IAPP — What is privacy?](https://iapp.org/about/what-is-privacy/) (International Association of Privacy Professionals)
- 📰 [ISACA — GRC fundamentals articles](https://www.isaca.org/resources/isaca-journal) (search "GRC fundamentals")
- 🎥 [IBM Technology — "What is cybersecurity?"](https://www.youtube.com/@IBMTechnology) (start with their 5-minute explainers)

---

## 0.2 How the internet works — a security-eye view

You cannot understand security without a mental model of how a web request travels. This is the 5-minute version.

### The journey of a click

You type `https://thesimplifiedtech.com` in your browser. Here's roughly what happens:

1. **DNS lookup.** Your computer has a hostname, not an address. It asks a DNS server: *"What's the IP address for thesimplifiedtech.com?"* DNS says `104.21.x.x`. Think of DNS as the internet's phonebook.
2. **TCP connection.** Your browser opens a conversation ("TCP connection") to that IP on port 443 (the standard port for HTTPS).
3. **TLS handshake.** Because it's HTTPS, your browser and the server negotiate a secure channel. The server presents a **certificate** — a cryptographic ID card — signed by a trusted authority. Your browser checks the signature. If valid, they agree on a secret key.
4. **HTTP request.** Now encrypted, your browser sends `GET /` — "please send me the homepage."
5. **Server responds** with HTML. Your browser renders it, fetches CSS/JS/images, maybe sends cookies for your login session.

Five steps. Every security topic later — phishing, MITM, cert pinning, DDoS, DNS hijacking — attacks one of these five steps.

### Key terms, translated

- **IP address** — numerical address of a computer on the internet.
- **DNS** — translates names to IPs.
- **TCP** — the protocol that ensures packets arrive in order.
- **Port** — a number on a computer indicating *which application* a packet is for. 80 = HTTP, 443 = HTTPS, 22 = SSH.
- **HTTP / HTTPS** — how browsers and servers talk. HTTPS is HTTP over TLS (encrypted).
- **TLS** — the encryption protocol (replaced the older SSL). Gives you confidentiality, integrity, server authenticity.
- **Certificate** — a signed document proving the server is who it claims to be. Issued by a **Certificate Authority (CA)** like Let's Encrypt.
- **Cookie** — a small piece of data the server asks your browser to remember and send back next time. Used for login sessions, personalisation, tracking.
- **Session** — a server's memory of "you" across multiple requests, usually tracked by a session cookie.

### Attack mapped to step

| Step | Example attack |
|------|---------------|
| DNS lookup | DNS hijacking / cache poisoning → you reach attacker's server |
| TCP connect | SYN-flood DDoS → server can't accept real users |
| TLS handshake | Expired/forged cert → man-in-the-middle reads traffic |
| HTTP request | SQL injection, XSS — attacks on the *content* of your request |
| Response/cookies | Session hijacking — stealing the cookie that proves you're logged in |

### Common beginner mistakes

- Confusing **encoding** (Base64) with **encryption** (TLS). Encoding is for format translation; anyone can reverse it. Encryption needs a key.
- Thinking the padlock icon means "this site is safe." It means "the connection is encrypted." The site itself could still be a scam.

### Mini-exercise (15 min)

1. Open your browser's DevTools (`F12` or `Cmd+Opt+I`).
2. Go to the **Network** tab, load any website, pick one request.
3. Identify: the request URL, the method (GET/POST), the response status code, at least one cookie, and one header starting with `Strict-Transport-Security` or `Content-Security-Policy`.

### Go deeper

- 📘 [Cloudflare Learning Center](https://www.cloudflare.com/learning/) — the single best free explainer for DNS, TLS, DDoS, CDN, zero trust. Start with "What is DNS?" and "What happens in a TLS handshake?".
- 📘 [MDN Web Docs — HTTP basics](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP)
- 🎥 [Professor Messer — Network Fundamentals free course](https://www.professormesser.com/)
- 📰 [How HTTPS Works (comic)](https://howhttps.works/) — genuinely fun.

---

## 0.3 Identity 101 — how computers know who you are

Almost every breach starts with "someone got in as someone else." Identity is the foundation.

### The three factors of authentication

Authentication is proving you are who you say you are. There are three historical "factors":

1. **Something you know** — password, PIN, answer to a secret question.
2. **Something you have** — phone, hardware token, smart card.
3. **Something you are** — fingerprint, face, iris.

A fourth is sometimes added: **somewhere you are** (location) or **something you do** (typing pattern).

**MFA (Multi-Factor Authentication)** = requiring at least two *different* factors. Two passwords isn't MFA. Password + SMS code is MFA, but weak. Password + a hardware key (YubiKey) is strong MFA.

### Why passwords fail

- People reuse them. One leaked site → all their accounts at risk ("credential stuffing").
- They're guessed (`Password123!`).
- They're phished (fake login page).

**Fixes that actually work:**
- **Password manager** (1Password, Bitwarden) — generates unique long random passwords per site.
- **Passkeys / FIDO2 / WebAuthn** — cryptographic, phishing-resistant, no password to steal. This is the future.
- **MFA** on everything, preferably TOTP (Google Authenticator, Authy) or a hardware key — **not** SMS if you can avoid it (SIM-swap attacks).

### Sessions and SSO

- After you log in, the server gives your browser a **session token** (usually in a cookie). That token proves "you are still logged in" for every request until it expires.
- **SSO (Single Sign-On)** — one identity provider (Google, Okta, Azure AD) logs you into many apps. Fewer passwords, one strong point to protect. Standards: SAML, OIDC (OpenID Connect).

### Worked example: "Forgot password" as an attack path

An attacker doesn't need your password; they need to reset it. So they try:
1. Target your email → if they own your email, they own every account.
2. Target your phone via SIM swap → intercept the SMS code.
3. Answer security questions (often guessable from social media).

**Defences:** use a unique email for critical accounts, remove SMS-only recovery where possible, use a password manager that can also store security-question answers (which you can make random gibberish).

### Common beginner mistakes

- Using the same "strong" password on many sites. Uniqueness matters more than complexity.
- Screenshotting MFA QR codes to "remember" them. That's a permanent shared secret — protect like a password.
- Believing "biometrics are the ultimate security." You can't change your fingerprint if it leaks.

### Interview traps

- **Q:** "Is HTTPS enough to protect login?"
  **A:** No. HTTPS protects the channel. It doesn't help if the user's password is `123456`, or if the login page is phished, or if the server stores passwords in plain text.

### Mini-exercise (10 min)

- List every account you have that still uses SMS as its only MFA. Switch to an authenticator app or hardware key for the top 3 you care about most (email, bank, primary work account).

### Go deeper

- 🏛 [NIST SP 800-63B — Digital Identity Guidelines](https://pages.nist.gov/800-63-3/sp800-63b.html) — the definitive, modern guidance on passwords and MFA. Readable.
- 📰 [EFF Surveillance Self-Defense](https://ssd.eff.org/) — plain-English identity hygiene for normal people.
- 📰 [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html) — for when you're the one building login.
- 📰 [FIDO Alliance — Passkeys explained](https://fidoalliance.org/passkeys/)

---

## 0.4 The language of risk

Security people talk in risk. Learn this vocabulary now and every later module becomes easier.

### Definitions (memorise these)

- **Asset** — something of value (data, a server, a reputation).
- **Threat** — a potential cause of harm (a hacker, a storm, a careless employee).
- **Threat actor** — a specific *who* behind the threat (organised crime, nation state, insider, script kiddie).
- **Vulnerability** — a weakness that a threat could exploit (unpatched software, weak password, door propped open).
- **Exploit** — the technique used to actually take advantage of the vulnerability.
- **Risk** — the *expected loss*: roughly, *Likelihood × Impact*.
- **Control** (a.k.a. safeguard, countermeasure) — something you put in place to reduce risk.
- **Inherent risk** — risk before any controls.
- **Residual risk** — risk remaining *after* controls.
- **Risk appetite** — how much risk the organisation is willing to take.
- **Risk tolerance** — the acceptable variance around appetite for a specific risk.

### Worked example

*Scenario: A small SaaS company stores customer emails.*

- **Asset:** customer email database.
- **Threat:** a credential-stuffing attacker.
- **Vulnerability:** admin login has no MFA and no rate limiting.
- **Risk:** "Attacker gains admin access and exfiltrates 100k emails." Likelihood = **Medium**, Impact = **High** (regulatory fines + reputation) → **High risk**.
- **Controls:** enforce MFA on admin (reduces likelihood), add rate limiting (reduces likelihood), encrypt data at rest (reduces impact if stolen), enable anomaly alerts (reduces impact by faster detection).
- **Residual risk after controls:** Low.

### The risk formula, demystified

Two common ways to score risk:

**Qualitative** — categories like Low / Medium / High / Critical. Fast, subjective, fine for most organisations.

**Quantitative** — numbers, often:
- **SLE (Single Loss Expectancy)** = Asset value × Exposure factor.
- **ARO (Annualised Rate of Occurrence)** = how many times per year.
- **ALE (Annualised Loss Expectancy)** = SLE × ARO.

*Example:* Asset value = $1M. A breach wipes 20% of value → SLE = $200k. Expected once every 5 years → ARO = 0.2. ALE = $40k/year. So a control costing up to $40k/year is, in principle, economically rational.

A more modern quantitative framework is **FAIR (Factor Analysis of Information Risk)** — worth knowing the name.

### Common beginner mistakes

- Calling an unpatched server "a risk." It's a **vulnerability**. The *risk* is "unauthenticated remote code execution leading to data exfiltration" — the outcome, not the weakness.
- Treating "High-High" on every risk. If everything is a top priority, nothing is. Force yourself to differentiate.
- Reporting only inherent risk. Executives care about **residual** — what's left after money spent.

### Interview traps

- **Q:** "You find a vulnerability scanner reports 3,000 findings. What's the risk?"
  **A:** *It depends.* 3,000 findings on internet-facing production systems with customer data is catastrophic. The same 3,000 on an isolated lab network is noise. Context (asset, exposure, impact) turns findings into risk.

### Mini-exercise (15 min)

Take a real daily activity (e.g., "I log into my bank on café Wi-Fi"). Write out: asset, threat, vulnerability, likelihood, impact, residual risk after two realistic controls.

### Go deeper

- 🏛 [NIST SP 800-30 Rev. 1 — Guide for Conducting Risk Assessments](https://csrc.nist.gov/pubs/sp/800/30/r1/final) (free PDF, ~95 pages, very readable).
- 📘 [FAIR Institute — free introductory resources](https://www.fairinstitute.org/resources)
- 📰 [ENISA Risk Management pages](https://www.enisa.europa.eu/topics/threat-risk-management)

---

## 0.5 Reading a policy without panicking

If you enter GRC, 30% of your reading life will be policies. Here's the universal shape.

### Every policy has the same skeleton

1. **Purpose** — why this policy exists.
2. **Scope** — to whom and to what it applies.
3. **Roles & responsibilities** — who does what.
4. **Policy statements** — the actual rules (must / shall / may).
5. **Exceptions process** — how to deviate legitimately.
6. **Enforcement & consequences** — what happens if you don't follow it.
7. **Review cycle** — usually annual.
8. **Version history & approvals.**

When you skim a policy, find those seven sections *first*, in order. Read details only for the ones that apply to you.

### "Must", "Should", "May" — the magic words

Borrowed from RFC 2119:
- **MUST / SHALL** — mandatory, no exceptions without formal approval.
- **SHOULD** — strongly recommended, deviations documented.
- **MAY** — permitted, discretionary.

Mis-reading these is the #1 audit finding.

### Policy vs standard vs procedure vs guideline

- **Policy** — *what* and *why*. ("All access to production data must be authenticated and logged.")
- **Standard** — *specific rules*. ("Passwords must be ≥14 characters, rotated on suspected compromise.")
- **Procedure** — *step-by-step how*. ("To onboard a new engineer, step 1 … step 2 …")
- **Guideline** — *recommended, not mandatory*. ("Consider using a password manager.")

You'll see this hierarchy in every mature organisation. Mixing them up causes real audit findings.

### Mini-exercise (10 min)

Find any company's public security policy (Google "[company name] security policy site:*.com"). Map it to the 8-section skeleton above. Note which sections are missing or weak.

### Go deeper

- 📰 [SANS Information Security Policy Templates](https://www.sans.org/information-security-policy/) — 40+ free templates, the gold standard.
- 📰 [RFC 2119 — Keywords for use in RFCs](https://www.ietf.org/rfc/rfc2119.txt)

---

## Module 0 — Glossary recap

Asset, Threat, Threat actor, Vulnerability, Exploit, Risk, Inherent risk, Residual risk, Control, Likelihood, Impact, Risk appetite, Risk tolerance, SLE, ARO, ALE, FAIR, DNS, IP, TCP, Port, HTTP, HTTPS, TLS, Certificate, Cookie, Session, Authentication, Authorisation, MFA, TOTP, FIDO2, Passkey, SSO, SAML, OIDC, Policy, Standard, Procedure, Guideline.

If fewer than half feel comfortable, re-read the relevant section. These words will appear in **every** remaining module.

→ Next: [Module 1 — Security First Principles](01-security-first-principles.md)

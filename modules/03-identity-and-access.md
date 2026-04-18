# Module 3 — Identity, Access & Authentication

> **Audience:** 🟢 🟡 🔴 · **Time:** ~60 min · **Prereqs:** Modules 0–2

## Why this matters

Verizon's Data Breach Investigations Report year after year says the same thing: **stolen credentials and privilege abuse cause the majority of breaches.** Identity is the new perimeter. Get this module right and you'll reduce 60–70 % of real-world risk for most organisations.

---

## 3.1 The IAM mental model: IAAA

Think of every access event as a four-step pipeline:

1. **Identification** — "Who *claim* you are" (a user-id, email).
2. **Authentication** — "Prove it." (password + MFA, passkey, cert).
3. **Authorisation** — "What are you allowed to do?" (role, policy).
4. **Accounting / Auditing** — "What did you actually do?" (logs).

Every step needs controls. Most breaches skip one of them.

### The library analogy

- **Identification:** "I'm Riya."
- **Authentication:** library card + a photo-ID match.
- **Authorisation:** "You can borrow 3 books; reference section requires permission."
- **Accounting:** the checkout log.

Take away any step and the library breaks.

---

## 3.2 Authentication factors (deeper pass)

Recall from Module 0:
- Something you **know** — password.
- Something you **have** — phone, hardware key.
- Something you **are** — biometrics.
- Something you **do** / **somewhere you are** — behavioural, geolocation.

### Comparing MFA methods (weakest to strongest)

| Method | Phishable? | Replayable? | Notes |
|--------|-----------|-------------|-------|
| SMS OTP | ✅ yes (SIM swap) | partially | Better than nothing; avoid for high-value accounts |
| Email OTP | ✅ yes | yes | Don't use |
| Push notifications (e.g., Duo, Okta Verify) | ⚠️ "MFA fatigue" attacks | no | Better; add number-matching |
| TOTP (Google Authenticator, Authy) | ✅ can be relay-phished | no | Good baseline |
| Hardware OTP (RSA SecurID) | ⚠️ | no | Legacy |
| **FIDO2 / WebAuthn / Passkeys** | ❌ phishing-resistant | no | **Best** — bound to origin, uses public-key crypto |

If you remember one sentence: **move critical users to FIDO2/passkeys**. That class of attack simply stops working against them.

### Passwordless vs Passkeys

**Passwordless** is a UX goal — no password in the user journey. **Passkeys** are a specific standard (built on FIDO2/WebAuthn) that syncs credentials across devices (e.g., iCloud Keychain, Google Password Manager, 1Password). Most "passwordless" is passkeys under the hood.

---

## 3.3 Password policy (finally catching up to reality)

Old rules ("rotate every 90 days, 1 uppercase, 1 number, 1 symbol") are **obsolete**. Modern guidance (NIST SP 800-63B):

- **Length ≥ 8** for users, ≥14 for admins. Longer is better.
- **No mandatory periodic rotation** unless compromise suspected.
- **Check against breached-password corpora** (Have I Been Pwned API is free).
- **No forced composition rules** (mix of symbols). They encourage patterns, not entropy.
- **No password hints / knowledge questions.**
- **Support password managers** (allow paste).

Pair this with **rate limiting**, **MFA**, and **anomaly detection** on login and you've eliminated most credential attacks.

---

## 3.4 Authorisation models

### RBAC — Role-Based Access Control

Assign **permissions to roles**, users get **roles**. Simple, auditable. 80 % of enterprises run on RBAC.

Example: `support_agent` role → can `read:tickets`, `update:tickets`. Add 200 agents to the role; later remove/edit in one place.

### ABAC — Attribute-Based Access Control

Decisions based on **attributes** of user, resource, action, and environment.

Example: *"Allow read access to patient records where `record.clinic == user.clinic` and `time BETWEEN 8am AND 8pm` and `user.training_complete == true`."*

More expressive; more complex. Great when rules are dynamic (OPA, AWS IAM policies are ABAC-ish).

### ReBAC — Relationship-Based Access Control

Decisions based on **relationships** ("you can read a doc if you're in the team that owns the folder that contains it"). Google's Zanzibar (powers Drive/YouTube permissions) is the canonical example. Fast-growing pattern for SaaS.

### PBAC, DAC, MAC

- **PBAC** (Policy-Based) — a broader umbrella.
- **DAC** (Discretionary) — resource owners choose who accesses (Unix file perms).
- **MAC** (Mandatory) — system enforces (government classification: Top Secret, Secret, etc.).

### Least privilege + separation of duties, applied

- **Least privilege:** every permission must be justified by a job need.
- **SoD:** high-risk actions require two different people (e.g., creating a user and approving their admin role).
- **JIT access** (Just-in-Time): standing admin is replaced by time-bound, approved elevation. Tools: Azure PIM, AWS IAM Identity Center, CyberArk, Teleport.

### Common beginner mistakes

- Role sprawl — 500 roles, nobody knows what each means. Do periodic **role mining / access reviews**.
- Mixing RBAC and ABAC without a policy model — hard to audit.
- "Admin" everywhere "just in case."

---

## 3.5 SSO, Federation, SAML, OIDC, SCIM

### SSO (Single Sign-On)

One identity provider (IdP) authenticates you; multiple apps (service providers, SPs) trust that authentication. Benefits: one strong auth point, centralised MFA, easy deprovisioning, better UX.

### Federation protocols

- **SAML 2.0** — enterprise legacy; XML-based; still dominant in corporate SSO.
- **OIDC (OpenID Connect)** — modern; built on OAuth 2.0; JSON/JWT-based; used by consumer apps and newer B2B SaaS.
- **OAuth 2.0** — authorisation protocol (for delegating access), often confused with authentication. *"Log in with Google"* uses OIDC, not plain OAuth 2.
- **SCIM** — System for Cross-domain Identity Management: standardises **provisioning / deprovisioning** users across apps (create/update/suspend/delete). Crucial for offboarding.

### The JWT minute

JSON Web Token — a signed (sometimes encrypted) token containing claims (`sub`, `iat`, `exp`, roles, etc.). Used as access tokens, ID tokens. Must verify signature on every request; check expiry; don't store sensitive data (it's signed, not encrypted by default).

### IdP providers

- **Enterprise:** Okta, Microsoft Entra ID (Azure AD), Google Workspace, Ping Identity, JumpCloud.
- **Consumer:** Google, Apple, Facebook, LinkedIn (via OIDC).
- **Open-source:** Keycloak, Authentik, Ory.

---

## 3.6 Privileged Access Management (PAM)

Admins are the crown jewels. PAM is the subset of IAM focused on protecting privileged accounts.

Typical PAM controls:
- **Vaulting** — admin passwords/keys rotated in a vault; admins check out, never see, passwords.
- **Session recording** — every admin console session is video-logged.
- **Just-in-time elevation** with approval workflow.
- **Break-glass accounts** — a tiny number of emergency accounts, heavily audited.
- **Separate admin identities** — you log in as *you* normally; `admin-you` is a separate account used only for admin work.

Tools: CyberArk, BeyondTrust, Delinea, HashiCorp Boundary, Teleport.

---

## 3.7 Identity lifecycle & access reviews

The **Joiner–Mover–Leaver (JML)** process:
- **Joiner:** baseline role + manager-approved extras, on day 1.
- **Mover:** when people change teams, remove old access before granting new.
- **Leaver:** revoke *everything* within hours, ideally before they're told. Disable email, VPN, SSO session, cloud tokens, mobile MDM, badges.

**Access review** (aka recertification): quarterly or biannual, each manager confirms their team's access is still justified. Required under SOC 2 CC6.x, ISO 27001 A.5.18, and most regulated frameworks.

### Worked example — leaver process

1. HR marks employee as terminated in HRIS (e.g., Workday).
2. SCIM triggers deprovisioning in Okta → cascades to 50+ apps.
3. Revoke session tokens (important — SSO logout isn't enough if apps have long JWTs).
4. Rotate any shared secrets the employee might know.
5. Reclaim devices; wipe via MDM.
6. Update `oncall` rotations and access groups.
7. Archive data per retention policy.
8. Record completion for audit.

A missed step here is the #1 source of "ex-employee still has access" audit findings.

---

## 3.8 Identity threats in the wild

- **Credential stuffing** — attacker uses leaked username/password combos from other sites.
- **Password spraying** — try a few common passwords across many accounts (evades lockout).
- **Phishing** — fake login page captures credentials (and sometimes MFA codes).
- **MFA fatigue / push bombing** — spam push approvals until a tired user taps "Accept."
- **Session hijacking** — steal the session cookie; bypass MFA entirely.
- **Golden/Silver ticket** (Active Directory) — forged Kerberos tickets grant unlimited access.
- **OAuth consent phishing** — trick user into granting a malicious app broad token scopes.

Defences include FIDO2, number-matching in push, short session lifetimes, phishing-resistant MFA for admins, and continuous session validation.

### Mini-exercise (15 min)

List, for your own work account: which IdP you use, which MFA methods are allowed, your session timeout, last time your manager reviewed your access, and whether your employer has a documented leaver checklist. You've just done a mini IAM audit.

---

## 3.9 Go deeper

- 🏛 [NIST SP 800-63-3 suite](https://pages.nist.gov/800-63-3/) — identity, authentication, federation. Read 63B first.
- 📰 [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
- 📰 [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- 📰 [FIDO Alliance — Passkeys](https://fidoalliance.org/passkeys/)
- 📘 [Auth0 Identity Labs](https://auth0.com/blog/) — excellent free deep-dives on OIDC, SAML, JWT.
- 📰 [Have I Been Pwned Passwords API (free)](https://haveibeenpwned.com/Passwords)
- 📰 [CISA Phishing-Resistant MFA guide](https://www.cisa.gov/MFA)

---

## Module 3 — Glossary recap

IAAA, Identification, Authentication, Authorisation, Accounting, MFA, TOTP, SMS OTP, Push MFA, Number-matching, MFA fatigue, FIDO2, WebAuthn, Passkey, Passwordless, RBAC, ABAC, ReBAC, PBAC, DAC, MAC, Least privilege, Separation of duties, JIT access, PAM, Break-glass, Vaulting, Session recording, SSO, IdP, SP, SAML, OIDC, OAuth 2.0, SCIM, JWT, Federation, Joiner-Mover-Leaver, Access review / recertification, Credential stuffing, Password spraying, OAuth consent phishing.

→ Next: [Module 4 — Networks & Infrastructure Security](04-network-security.md)

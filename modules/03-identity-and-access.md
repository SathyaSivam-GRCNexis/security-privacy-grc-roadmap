# Module 3, Identity, Access & Authentication

> **Audience:** 🟢 🟡 🔴 · **Time:** ~60 min · **Prereqs:** Modules 0–2

## Why this matters

Verizon's Data Breach Investigations Report says the same thing year after year. Stolen credentials and privilege abuse cause the majority of breaches. Identity is the new perimeter. Get this module right and you remove a large slice of real-world risk for most organisations.

Sequencing tip: in a real role, learn **privileged access** first, then SSO/MFA rollout, then the rest. Most of the damage in breaches comes from a small number of admin accounts. And here's the part bootcamps never tell you: rolling out SSO and MFA is mostly **change management**, not engineering. The hard part is the VP who refuses to enrol a hardware key, not the Okta config.

---

## 3.1 The IAM mental model: IAAA

Think of every access event as a four-step pipeline:

1. **Identification.** Who you claim to be (a user-id, email).
2. **Authentication.** Prove it (password plus MFA, passkey, cert).
3. **Authorisation.** What you're allowed to do (role, policy).
4. **Accounting / Auditing.** What you actually did (logs).

Every step needs controls. Most breaches skip one of them, usually accounting (no logs) or authorisation (someone has more access than they should).

### The library analogy

- **Identification.** "I'm Riya."
- **Authentication.** Library card plus a photo-ID match.
- **Authorisation.** "You can borrow 3 books. The reference section requires permission."
- **Accounting.** The checkout log.

Take away any step and the library breaks.

---

## 3.2 Authentication factors (deeper pass)

Recall from Module 0:
- Something you **know.** Password.
- Something you **have.** Phone, hardware key.
- Something you **are.** Biometrics.
- Something you **do** or **somewhere you are.** Behavioural, geolocation.

### Comparing MFA methods (weakest to strongest)

| Method | Phishable? | Replayable? | Notes |
|--------|-----------|-------------|-------|
| SMS OTP | ✅ yes (SIM swap) | partially | Better than nothing. Avoid for high-value accounts |
| Email OTP | ✅ yes | yes | Don't use |
| Push notifications (Duo, Okta Verify) | ⚠️ "MFA fatigue" attacks | no | Better. Add number-matching |
| TOTP (Google Authenticator, Authy) | ✅ can be relay-phished | no | Good baseline |
| Hardware OTP (RSA SecurID) | ⚠️ | no | Legacy |
| **FIDO2 / WebAuthn / Passkeys** | ❌ phishing-resistant | no | **Best.** Bound to origin, uses public-key crypto |

If you remember one sentence: move critical users to FIDO2 or passkeys. That class of attack stops working against them. In one SaaS environment I worked in, moving the ~30 most privileged users to hardware keys cut "suspicious login" alert volume by more than half within a quarter.

### Passwordless vs Passkeys

Passwordless is a UX goal: no password in the user flow. Passkeys are a specific standard (built on FIDO2/WebAuthn) that syncs credentials across devices via iCloud Keychain, Google Password Manager, 1Password. Most "passwordless" is passkeys underneath.

---

## 3.3 Password policy (finally catching up to reality)

The old rules (rotate every 90 days, 1 uppercase, 1 number, 1 symbol) are obsolete. Modern guidance (NIST SP 800-63B):

- **Length ≥ 8** for users, ≥14 for admins. Longer is better.
- **No mandatory periodic rotation** unless compromise is suspected.
- **Check against breached-password corpora.** The Have I Been Pwned API is free.
- **No forced composition rules** (mix of symbols). They encourage patterns, not entropy.
- **No password hints or knowledge questions.**
- **Support password managers.** Allow paste.

Pair this with rate limiting, MFA and anomaly detection on login and you've eliminated most credential attacks.

Friction note: telling a CISO who's been around since 2010 that you no longer want quarterly password rotation usually starts an argument. Bring the NIST guidance link. It helps.

---

## 3.4 Authorisation models

### RBAC, Role-Based Access Control

Assign permissions to roles. Users get roles. Simple, auditable. Most enterprises run on RBAC.

Example: `support_agent` role can `read:tickets`, `update:tickets`. Add 200 agents to the role. Later remove or edit in one place.

### ABAC, Attribute-Based Access Control

Decisions based on attributes of user, resource, action and environment.

Example: *"Allow read access to patient records where `record.clinic == user.clinic` and `time BETWEEN 8am AND 8pm` and `user.training_complete == true`."*

More expressive. More complex. Great when rules are dynamic (OPA, AWS IAM policies are ABAC-ish).

### ReBAC, Relationship-Based Access Control

Decisions based on relationships. "You can read a doc if you're in the team that owns the folder that contains it." Google's Zanzibar (powers Drive and YouTube permissions) is the canonical example. Growing fast in SaaS.

### PBAC, DAC, MAC

- **PBAC** (Policy-Based). A broader umbrella.
- **DAC** (Discretionary). Resource owners choose who accesses. Unix file perms.
- **MAC** (Mandatory). System enforces. Government classification: Top Secret, Secret, etc.

### Least privilege and separation of duties, applied

- **Least privilege.** Every permission must be justified by a job need.
- **SoD.** High-risk actions require two different people. For example, creating a user and approving their admin role.
- **JIT access** (Just-in-Time). Standing admin is replaced by time-bound, approved elevation. Tools: Azure PIM, AWS IAM Identity Center, CyberArk, Teleport.

### What beginners typically miss

- Role sprawl. 500 roles, nobody knows what each means. Do periodic role mining and access reviews.
- Mixing RBAC and ABAC without a policy model. Hard to audit.
- "Admin" everywhere "just in case." This is the most common audit finding I write in real life.

---

## 3.5 SSO, Federation, SAML, OIDC, SCIM

### SSO (Single Sign-On)

One identity provider (IdP) authenticates you. Multiple apps (service providers, SPs) trust that authentication. Benefits: one strong auth point, centralised MFA, easy deprovisioning, better UX.

The real benefit isn't UX. It's that when someone leaves, you can kill access to 80+ apps from one screen. Without SSO, leavers are a graveyard of orphaned accounts.

### Federation protocols

- **SAML 2.0.** Enterprise legacy. XML-based. Still dominant in corporate SSO.
- **OIDC (OpenID Connect).** Modern. Built on OAuth 2.0. JSON/JWT-based. Used by consumer apps and newer B2B SaaS.
- **OAuth 2.0.** Authorisation protocol for delegating access. Often confused with authentication. "Log in with Google" uses OIDC, not plain OAuth 2.
- **SCIM.** System for Cross-domain Identity Management. Standardises provisioning and deprovisioning users across apps (create/update/suspend/delete). Crucial for offboarding.

If a vendor says "we support SSO" and means "you can sign in with Google", that's not enterprise SSO. Ask for SAML or OIDC with SCIM. Verify before signing.

### The JWT minute

JSON Web Token. A signed (sometimes encrypted) token containing claims (`sub`, `iat`, `exp`, roles, etc.). Used as access tokens and ID tokens. Verify the signature on every request. Check expiry. Don't store sensitive data in the JWT body. It's signed, not encrypted by default.

### IdP providers

- **Enterprise.** Okta, Microsoft Entra ID (Azure AD), Google Workspace, Ping Identity, JumpCloud.
- **Consumer.** Google, Apple, Facebook, LinkedIn (via OIDC).
- **Open-source.** Keycloak, Authentik, Ory.

---

## 3.6 Privileged Access Management (PAM)

Admins are the crown jewels. PAM is the subset of IAM focused on protecting privileged accounts. If you only have budget for one identity initiative this year, this is probably the one.

Typical PAM controls:
- **Vaulting.** Admin passwords and keys rotated in a vault. Admins check out, never see, passwords.
- **Session recording.** Every admin console session is video-logged.
- **Just-in-time elevation** with approval workflow.
- **Break-glass accounts.** A tiny number of emergency accounts, heavily audited.
- **Separate admin identities.** You log in as you normally. `admin-you` is a separate account used only for admin work.

Tools: CyberArk, BeyondTrust, Delinea, HashiCorp Boundary, Teleport.

Reality check: full PAM rollouts take 6–18 months and are deeply political. Start with read-only inventory of who has prod admin rights today. That alone is usually a surprise.

---

## 3.7 Identity lifecycle and access reviews

The **Joiner–Mover–Leaver (JML)** process:
- **Joiner.** Baseline role plus manager-approved extras, on day 1.
- **Mover.** When people change teams, remove old access *before* granting new.
- **Leaver.** Revoke everything within hours, ideally before they're told. Disable email, VPN, SSO session, cloud tokens, mobile MDM, badges.

**Access review** (also called recertification). Quarterly or biannual. Each manager confirms their team's access is still justified. Required under SOC 2 CC6.x, ISO 27001 A.5.18 and most regulated frameworks.

Honest hedge: access reviews are usually theatre in their first year. Managers tick "approve" without reading. That's still better than not running them. By year two you can start adding micro-frictions, like flagging entitlements unused for 90 days, that force a real decision.

### Worked example, leaver process

1. HR marks employee as terminated in HRIS (e.g., Workday).
2. SCIM triggers deprovisioning in Okta, cascades to 50+ apps.
3. Revoke session tokens. Important. SSO logout isn't enough if apps have long JWTs.
4. Rotate any shared secrets the employee might know.
5. Reclaim devices. Wipe via MDM.
6. Update on-call rotations and access groups.
7. Archive data per retention policy.
8. Record completion for audit.

A missed step here is the number one source of "ex-employee still has access" audit findings.

---

## 3.8 Identity threats in the wild

- **Credential stuffing.** Attacker uses leaked username/password combos from other sites.
- **Password spraying.** Try a few common passwords across many accounts. Evades lockout.
- **Phishing.** Fake login page captures credentials (and sometimes MFA codes).
- **MFA fatigue / push bombing.** Spam push approvals until a tired user taps Accept.
- **Session hijacking.** Steal the session cookie. Bypass MFA entirely.
- **Golden / Silver ticket** (Active Directory). Forged Kerberos tickets grant unlimited access.
- **OAuth consent phishing.** Trick a user into granting a malicious app broad token scopes. Common pattern in M365 environments.

Defences include FIDO2, number-matching in push, short session lifetimes, phishing-resistant MFA for admins, and continuous session validation.

### Mini-exercise (15 min)

List, for your own work account: which IdP you use, which MFA methods are allowed, your session timeout, the last time your manager reviewed your access, and whether your employer has a documented leaver checklist. You've just done a mini IAM audit.

---

## 3.9 Go deeper

- 🏛 [NIST SP 800-63-3 suite](https://pages.nist.gov/800-63-3/). Identity, authentication, federation. Read 63B first.
- 📰 [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
- 📰 [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- 📰 [FIDO Alliance, Passkeys](https://fidoalliance.org/passkeys/)
- 📘 [Auth0 Identity Labs](https://auth0.com/blog/). Free deep-dives on OIDC, SAML, JWT.
- 📰 [Have I Been Pwned Passwords API (free)](https://haveibeenpwned.com/Passwords)
- 📰 [CISA Phishing-Resistant MFA guide](https://www.cisa.gov/MFA)

---

## Module 3, Glossary recap

IAAA, Identification, Authentication, Authorisation, Accounting, MFA, TOTP, SMS OTP, Push MFA, Number-matching, MFA fatigue, FIDO2, WebAuthn, Passkey, Passwordless, RBAC, ABAC, ReBAC, PBAC, DAC, MAC, Least privilege, Separation of duties, JIT access, PAM, Break-glass, Vaulting, Session recording, SSO, IdP, SP, SAML, OIDC, OAuth 2.0, SCIM, JWT, Federation, Joiner-Mover-Leaver, Access review / recertification, Credential stuffing, Password spraying, OAuth consent phishing.

→ Next: [Module 4, Networks & Infrastructure Security](04-network-security.md)

# Module 10: Policies, Standards & Procedures

> **Audience:** 🟡 🔴 · **Time:** ~45 min · **Prereqs:** Modules 0, 7

## Why this matters

Policies are where regulation meets reality. They are also the most common Major NC in ISO audits ("policy not updated," "policy not approved," "no evidence of training"). A company can have great tooling and still fail because its policy set is a mess.

A strong opinion to start with: **there are two kinds of policies, the ones nobody reads, and the ones engineering actually follows**. The ones nobody reads pass audits and protect nothing. The ones engineering follows protect the company but only get written when GRC stops drafting in isolation and co-authors with the team that has to live with the rules. Aim for the second kind. Auditors can usually tell within five minutes which one they're looking at.

---

## 10.1 The documentation hierarchy

| Level | What it is | Audience | Review cadence |
|-------|-----------|----------|----------------|
| **Policy** | What and why (mandatory) | All employees | Annually |
| **Standard** | Specific rules (e.g., "passwords ≥14 chars") | Relevant teams | Annually |
| **Procedure** | Step-by-step how | Doers of the task | On change |
| **Guideline** | Recommendations | Optional | As needed |
| **Record** | Evidence of execution | Auditors | Retained per schedule |

### Worked example

- **Policy:** Access Control Policy. "All access must be authenticated, least-privilege, and reviewed periodically."
- **Standard:** Authentication Standard. "Passwords ≥14 chars. MFA required on SSO."
- **Procedure:** Admin Elevation Procedure. "1) raise ticket, 2) manager approval, 3) PIM grants role for 4 hours, 4) audit log captured."
- **Guideline:** "Consider passkeys for all production systems."
- **Record:** the ticket, the approval, the PIM log. Evidence the procedure ran.

Mixing these is the single largest source of audit confusion. A policy that lists exact password lengths is a policy you have to re-approve every time NIST changes its recommendation. Keep the specifics in the standard.

---

## 10.2 Anatomy of a good policy

Every policy should have, in this order:

1. **Document metadata**: title, version, owner, approver, effective date, next review date, distribution.
2. **Purpose**: one paragraph.
3. **Scope**: people, systems, locations.
4. **Definitions**: only as needed.
5. **Policy statements**: using **MUST / SHOULD / MAY** precisely.
6. **Roles & responsibilities**: RACI if helpful.
7. **Exceptions**: who can grant, how to request, time bounds.
8. **Enforcement & consequences**: what happens on violation.
9. **Related documents**: other policies, standards, regulations.
10. **Version history**: every change, date, approver.

Keep it short. Three to five pages. Long policies aren't read. Short policies are.

### RFC 2119 keywords

- **MUST / SHALL**: mandatory.
- **MUST NOT / SHALL NOT**: prohibited.
- **SHOULD**: strongly recommended. Deviations documented.
- **SHOULD NOT**: not recommended unless justified.
- **MAY**: permitted.

Use these literally. "All developers should enable MFA" is ambiguous. "All developers MUST enable MFA on SSO" is enforceable.

---

## 10.3 The baseline policy set (for SOC 2 / ISO 27001)

You'll typically need around 20 policies. Group by theme:

### Governance & risk
- Information Security Policy (top-level)
- Risk Management Policy
- Exception Management Policy

### People
- Acceptable Use Policy
- HR Security Policy (screening, onboarding, offboarding, disciplinary)
- Security Awareness & Training Policy
- Remote Work Policy

### Asset & data
- Asset Management Policy
- Data Classification & Handling Policy
- Data Retention & Disposal Policy
- Privacy Policy (internal) + Privacy Notice (external)

### Access & identity
- Access Control Policy
- Privileged Access Policy

### Operations & engineering
- Change Management Policy
- Secure Development / SDLC Policy
- Cryptography & Key Management Policy
- Vulnerability & Patch Management Policy
- Logging & Monitoring Policy
- Backup Policy

### Third-party & physical
- Vendor / Third-Party Risk Policy
- Physical Security Policy

### Resilience & response
- Incident Response Policy
- Business Continuity & Disaster Recovery Policy

### Emerging
- AI Acceptable Use Policy (added 2023+)
- BYOD / Mobile Device Policy

If you have these 20, versioned, approved, trained, and reviewed, you have eliminated roughly a third of typical audit findings before you start.

---

## 10.4 Lifecycle of a policy

1. **Author**: draft.
2. **Review**: legal, security, privacy, impacted business owners. Co-authoring with the team that will follow it is the difference between a real policy and shelfware.
3. **Approve**: appropriate authority (CISO, CEO, board for top-level).
4. **Publish**: central repository (SharePoint, Confluence, Notion).
5. **Communicate**: all-hands, training module, acknowledgement.
6. **Enforce**: tooling, audits, disciplinary path.
7. **Review**: annually, or on triggers (regulatory change, incident, org change).
8. **Retire**: when superseded. Kept in archive for evidence.

### The signatures that matter

- Policy *owner* (accountable).
- Policy *approver* (authority).
- Employee *acknowledgement*. You must be able to prove every employee saw the policy and accepted it. HR onboarding flow plus an annual re-acknowledgement is the cheapest way to do this.

---

## 10.5 Exception management: the quiet killer

Every policy needs an exception process. Without one, teams silently ignore the policy and the gap surfaces during the next audit, usually as a Minor NC.

Exception requests capture:
- What's being excepted. Which policy clause.
- Why (business justification).
- Risk introduced and compensating controls.
- Approver (risk owner plus security plus privacy as relevant).
- Expiry date (time-bound, usually ≤12 months).
- Review conditions.

Track all exceptions in a register. Expired exceptions either get renewed with fresh justification or close (control returns).

The honest truth about exceptions: there should be some. A policy with zero exceptions across a real engineering org probably isn't being followed. Some exceptions, well-documented, with expiry dates, is healthier than none.

---

## 10.6 Training and awareness

Policies you don't train are policies you don't have.

- **On hire**: baseline security and privacy training, plus role-specific (devs get SDLC, admins get privileged access).
- **Annually**: refresher.
- **On change**: material updates trigger recommunication.
- **Phishing simulations**: quarterly.
- **Role-based deep dives**: engineering (secure coding), HR (background checks), customer support (social engineering), finance (BEC).

Track completion. Most audits sample training records.

---

## 10.7 Using templates correctly

Free templates are a fine starting point. They become harmful if you drop them in unchanged.

- **Customise scope, roles, and statements** to your org.
- **Remove sections that don't apply.** Do not list HSMs if you have none.
- **Align numbers** with reality. Do not write "backups tested quarterly" if you do it annually.
- **Language consistency.** British vs American English, defined terms capitalised.
- **Legal review** for anything external-facing (Privacy Notice, ToS).

SANS templates are excellent starting points. Plan to rewrite at least half. Auditors recognise unmodified SANS text from across the room.

---

## 10.8 Common beginner mistakes

- Policies written by security alone, disconnected from how work happens. Co-author with the team that will comply.
- Over-long policies nobody reads. Move standards and procedures into separate docs.
- Version numbers that never change despite edits. Use a real document-control system, even a simple Notion property.
- No approval record. An unsigned policy is, for audit purposes, not a policy.
- Policies contradicting each other. Review annually for consistency. Cross-references break silently.
- Treating the annual review as a date change. If you didn't actually re-read it, the reviewer date is a lie.

## 10.9 Mini-exercise (30 min)

1. Open a [SANS Acceptable Use Policy template](https://www.sans.org/information-security-policy/).
2. Rewrite it for a 50-person EdTech startup. Scope, roles, 8 to 12 policy statements using MUST/SHOULD, exception process, enforcement.
3. Keep it to 3 pages.
4. Identify one other policy that would need to reference yours.

## 10.10 Go deeper

- 📰 [SANS Policy Templates (40+ free)](https://www.sans.org/information-security-policy/)
- 🏛 [NIST OLIR / policy examples](https://csrc.nist.gov/projects/olir)
- 📰 [Advisera free ISO 27001 policy samples](https://advisera.com/27001academy/free-downloads/)
- 📰 [Vanta / Drata / Secureframe free policy packs](https://www.vanta.com/collection/soc-2)
- 🏛 [RFC 2119: MUST/SHOULD/MAY](https://www.ietf.org/rfc/rfc2119.txt)

## Module 10: Glossary recap

Policy, Standard, Procedure, Guideline, Record, RFC 2119, MUST / SHOULD / MAY, RACI, Policy owner, Policy approver, Employee acknowledgement, Exception register, Time-boxed exception, Compensating control, Annual policy review, Training completion, SANS templates.

→ Next: [Module 11: Incident Response & Business Continuity](11-incident-response-bcp.md)

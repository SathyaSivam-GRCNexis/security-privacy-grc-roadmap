# Vendor Security & Privacy Assessment

> **Template note.** This is the form you complete, or have the vendor complete, before a contract is signed, and again at renewal. The depth of the assessment matches the risk tier. A vendor that holds your customer's personal data is Tier 1 and gets the full form. A vendor that prints your office stationery is Tier 3 and gets a five-line entry in the register.
>
> Keep a vendor inventory separately. This template is the per-vendor file.
>
> **Vendor name:** _________ · **Vendor ID:** VND-_________ · **Assessment date:** _________ · **Assessor:** _________ · **Next review:** _________

---

## 1. Vendor profile

**Legal entity name:** _________

**Operating name (if different):** _________

**Country of incorporation:** _________

**Headquarters address:** _________

**Primary regulator (if regulated entity):** _________

**Public-company / private:** _________

**Years in operation:** _________

**Approximate headcount:** _________

**Vendor's primary website:** _________

**Vendor's trust centre URL (if any):** _________

---

## 2. Service description

**Service the vendor provides to us:** _________

**Business owner internally:** _________

**Contract owner internally:** _________

**Replaces / complements internal capability:** _________

**Contract value (annual):** _________

**Contract term and renewal:** _________

---

## 3. Data exposure

| Data category | Will the vendor hold this data? | Volume / scope | Notes |
|---|---|---|---|
| Personal data (general) | Yes / No | | |
| Special category / sensitive personal data | Yes / No | | |
| Children's data | Yes / No | | |
| Financial data (cards / accounts) | Yes / No | | PCI-DSS scope check needed |
| Authentication credentials | Yes / No | | |
| Health data | Yes / No | | |
| Biometric data | Yes / No | | |
| Confidential business data | Yes / No | | |
| Source code / IP | Yes / No | | |

**Geographies of data subjects affected:** _________

**Geographies where the vendor will store and process data:** _________

**Cross-border transfer mechanism:** SCCs / adequacy decision / DPDP-permitted / BCRs / not applicable

---

## 4. Risk tier

Tier the vendor before going further. The depth of the rest of this assessment depends on the tier.

- **Tier 1, Critical.** Holds personal data at scale, integrates with production systems, or an outage hits revenue or safety. Full assessment, annual review, signed DPA, evidence of independent attestation, and a documented exit plan.
- **Tier 2, Significant.** Holds limited personal data or supports an important business process. Standard assessment, biennial review, signed DPA where applicable.
- **Tier 3, Standard.** No personal data, no production integration, low operational impact. Lightweight check at onboarding and at renewal.

**This vendor's tier:** _________

**Rationale:** _________

If Tier 3, complete sections 5 and 11 only and stop. If Tier 1 or 2, complete the rest.

---

## 5. Compliance attestations

| Attestation / certification | Held? | Scope | Date issued | Expiry | Evidence reviewed |
|---|---|---|---|---|---|
| SOC 2 Type II | Yes / No | | | | |
| ISO 27001:2022 | Yes / No | | | | |
| ISO 27701 | Yes / No | | | | |
| PCI-DSS (level) | Yes / No | | | | |
| HIPAA / HITECH | Yes / No | | | | |
| FedRAMP | Yes / No | | | | |
| CSA STAR | Yes / No | | | | |
| Country-specific (e.g., MeitY empanelment) | Yes / No | | | | |

**If no independent attestation,** does the vendor accept a security questionnaire (SIG / CAIQ / HECVAT)? _________

**If no questionnaire either,** the vendor is unsuitable for Tier 1. Record the recommendation.

---

## 6. Security controls (questionnaire summary)

For Tier 1 vendors, ask for the most recent SOC 2 or ISO report under NDA. For Tier 2, the questionnaire is enough. The questions below are the minimum.

### Identity and access management

- Does the vendor enforce MFA for all employees with access to customer data? _________
- How is privileged access managed and reviewed? _________
- Is there a joiner-mover-leaver process documented? _________

### Encryption

- Is data encrypted at rest? Algorithm and key management? _________
- Is data encrypted in transit? Minimum TLS version? _________
- Does the vendor support BYOK or HYOK if requested? _________

### Vulnerability management

- Frequency of internal vulnerability scans? _________
- Frequency and scope of external pen-tests? _________
- Patching SLAs by severity? _________

### Logging and monitoring

- What is logged from the customer's data perimeter? _________
- Retention period? _________
- Is logging tamper-evident? _________

### Incident response

- Documented incident response plan? Last tested when? _________
- Notification SLA to customers in the event of a breach? _________
- Which regulators is the vendor obliged to notify, on what clock? _________

### Personnel

- Pre-employment background screening? _________
- Annual security training? _________
- Confidentiality agreements signed by all staff with data access? _________

### Sub-processors

- Does the vendor publish a sub-processor list? _________
- Notification mechanism for new sub-processors? _________
- Right to object to new sub-processors? _________

### Business continuity and resilience

- Documented BCP? RTO and RPO commitments? _________
- Last DR test? _________
- Geographic redundancy of customer data? _________

### Secure development (where vendor builds the service)

- SDLC with security gates? _________
- Code reviews mandatory? _________
- SAST and DAST in CI/CD? _________
- Threat modelling for new features handling personal data? _________

---

## 7. Privacy and contractual obligations

- Is a Data Processing Agreement (DPA) in place? _________
- Does the DPA include GDPR Art. 28 obligations? _________
- Does the DPA address DPDP Act 2023 processor obligations? _________
- Are SCCs (or equivalent) attached for cross-border transfers? _________
- Is there a documented sub-processor approval process? _________
- Right-to-audit clause in the contract? _________
- Breach-notification SLA in the contract? Hours: _________
- Data return / deletion obligation at contract end? _________
- Liability cap and carve-outs reviewed by legal? _________

---

## 8. Concentration and exit risk

- Is this vendor a single source for the function? _________
- If yes, what is the failover plan? _________
- Estimated time to migrate to an alternative vendor? _________
- Data portability mechanism (export format, frequency, automation)? _________
- Is there a documented exit plan? _________

---

## 9. Findings

| # | Finding | Severity (H/M/L) | Recommendation | Owner | Target date |
|---|---|---|---|---|---|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |

---

## 10. Recommendation

- [ ] Approve.
- [ ] Approve with conditions (list below).
- [ ] Reject.
- [ ] Defer pending further information.

**Conditions:** _________

**Sign-off:**

| Role | Name | Decision | Date |
|---|---|---|---|
| Security assessor | | | |
| DPO | | | |
| Procurement | | | |
| Business owner | | | |
| Legal | | | |

---

## 11. Renewal review

| Review date | Reviewer | Outcome | Notes |
|---|---|---|---|
| | | | |
| | | | |

---

*Filed in vendor register VND-_________. Linked to contract CON-_________ and (if applicable) DPA DPA-_________.*

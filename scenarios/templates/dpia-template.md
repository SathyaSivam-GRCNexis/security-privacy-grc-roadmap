# Data Protection Impact Assessment (DPIA)

> **Template note.** A DPIA is required under GDPR Article 35, and recommended under DPDP Act 2023 (mandatory for Significant Data Fiduciaries), where processing is **likely to result in a high risk to the rights and freedoms of natural persons**. The usual triggers: large-scale processing of special-category data, systematic monitoring, automated decisions with legal effect, and new tech (AI, biometrics).
>
> Fill in every section. Empty sections are themselves a finding. If a section genuinely does not apply, write "Not applicable" and say why in one sentence. Auditors and DPAs read DPIAs in order. They notice the gaps before they notice the answers.
>
> Date: _________ · DPIA reference: _________ · Version: _________

---

## 1. Processing overview

**Name of processing activity:** _________

**Owner (business):** _________

**Owner (DPO / privacy):** _________

**Date initiated:** _________ · **Target launch date:** _________

**One-paragraph plain-English description.** What are you doing, with whose data, and why? Write it as if you were explaining to a non-technical board member who has fifteen seconds.

---

## 2. Necessity and proportionality

**Why is this processing necessary?** State the business outcome. "Product wants it" is not an answer.

**Lawful basis (GDPR Art. 6):** _________ (consent / contract / legal obligation / vital interests / public task / legitimate interests)

**For special category data (Art. 9), additional condition:** _________

**For DPDP Act 2023:** Has consent been obtained under Sec. 6, or does a "legitimate use" under Sec. 7 apply? State which: _________

**Could you get the same outcome with less data?** Walk through the data-minimisation analysis honestly. If yes, why are you not doing that?

**Could you get the same outcome with anonymised or pseudonymised data?** _________

---

## 3. Data inventory

| Data category | Specific fields | Volume (approx) | Source | Retention |
|---|---|---|---|---|
| | | | | |
| | | | | |

**Data subjects affected:** customers / employees / minors / vulnerable groups / others (specify): _________

**Estimated number of data subjects:** _________

**Geographic scope of data subjects:** _________

---

## 4. Data flows

**Describe the flow.** Where does the data originate, where does it travel, where does it land, and where does it leave the organisation? A diagram is better than prose. Annotate the boundaries (controller-to-processor, intra-group, cross-border).

**Cross-border transfers:** Yes / No. If yes:

- Countries: _________
- Transfer mechanism (SCCs / adequacy decision / DPDP "negative list" check / BCRs): _________
- Transfer impact assessment completed: Yes / No / In progress

**Sub-processors involved:** _________ (link to sub-processor register)

---

## 5. Stakeholders consulted

| Stakeholder | Role | Input received | Date |
|---|---|---|---|
| DPO | | | |
| Information Security | | | |
| Legal | | | |
| Engineering | | | |
| Product | | | |
| Affected business unit | | | |
| Data subjects (where appropriate) | | | |

**Was a representative of data subjects consulted?** GDPR Art. 35(9) recommends this where appropriate. If you skipped it, say why on the record.

---

## 6. Risk assessment

For each identified risk, score likelihood (1–5) and severity to the data subject (1–5). Score is L × S.

| # | Risk to the data subject | Likelihood | Severity | Score | Mitigations | Residual risk |
|---|---|---|---|---|---|---|
| 1 | Unauthorised access by internal staff | | | | | |
| 2 | Unauthorised disclosure to third party | | | | | |
| 3 | Loss of availability (data subject cannot exercise rights) | | | | | |
| 4 | Re-identification of pseudonymised data | | | | | |
| 5 | Discrimination / unfair outcome from automated decision | | | | | |
| 6 | Function creep (purpose expanded later) | | | | | |
| 7 | Excessive retention | | | | | |
| 8 | Children or vulnerable persons exposed disproportionately | | | | | |

Add scenario-specific risks. A DPIA copied from a generic template is itself a finding.

---

## 7. Mitigations and safeguards

For each Medium or High residual risk, describe the safeguard:

| Risk # | Safeguard | Owner | Implementation date | Evidence |
|---|---|---|---|---|
| | | | | |

Common safeguards: encryption at rest and in transit, access controls and audit logging, pseudonymisation, retention limits with auto-deletion, role-based access, signed DPA with the vendor, training for handlers, rate-limits on bulk export, vendor SOC 2 or ISO 27001 attestation reviewed, opt-out mechanism, human-in-the-loop for automated decisions.

---

## 8. Data subject rights impact

Confirm and describe how each right is operationally enabled for this processing:

- **Right to be informed** (privacy notice updated): _________
- **Right of access:** _________
- **Right to rectification:** _________
- **Right to erasure:** _________ (note any exemptions claimed, e.g., GDPR Art. 17(3))
- **Right to restriction:** _________
- **Right to portability:** _________
- **Right to object:** _________
- **Rights related to automated decision-making (Art. 22):** _________

---

## 9. Conclusion

**Residual risk rating:** Low / Medium / High

**Recommendation:** Proceed / Proceed with conditions / Do not proceed / Consult supervisory authority (Art. 36, required where high residual risk remains after mitigation)

**Conditions (if any):** _________

**Sign-off:**

| Role | Name | Decision | Date | Signature |
|---|---|---|---|---|
| DPO | | | | |
| CISO | | | | |
| Process owner | | | | |
| Legal | | | | |

---

## 10. Review schedule

DPIAs are living documents. Review when:

- The processing changes materially.
- A new sub-processor is added.
- A breach affects this processing.
- Annually, regardless.

**Next scheduled review:** _________

---

*Filed alongside the records of processing activities (Art. 30 ROPA). Available to the supervisory authority on request.*

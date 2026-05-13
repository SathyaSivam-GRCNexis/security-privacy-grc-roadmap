# Statement of Applicability (SoA)

> **Template note.** The Statement of Applicability is the central artefact of an ISO 27001 ISMS. It lists every Annex A control (93 in the 2022 revision), states whether it is applicable, justifies inclusions and exclusions, and points to where each control is implemented. Auditors live in this document. So should you.
>
> Maintain it in a spreadsheet for real use. The Markdown below shows the columns and a sample of rows. Copy the structure into Excel, Google Sheets, or your GRC tool, and complete all 93 rows.
>
> **Organisation:** _________ · **ISMS scope:** _________ · **SoA version:** _________ · **Date:** _________ · **Approved by:** _________

---

## How to fill this in

For each Annex A control:

1. **Applicable?** Yes or No. If No, you must justify the exclusion (Clause 6.1.3 d). "We are too small" is not a justification. "We do not develop software in-house, therefore secure development controls do not apply" is.
2. **Justification for inclusion or exclusion.** One or two sentences. Reference the risks the control addresses.
3. **Implementation status.** Implemented / Partially implemented / Planned / Not implemented.
4. **Implementation reference.** The policy, procedure, system, or evidence that shows the control. Use a stable reference, not "see Confluence."
5. **Control owner.** A named role, not a team.
6. **Last reviewed.** Date.

---

## Sample rows (Annex A 2022, 93 controls across 4 themes)

### A.5 Organizational controls (37 controls)

| Control ID | Control Name | Applicable? | Justification | Implementation Status | Implementation Reference | Control Owner | Last Reviewed |
|---|---|---|---|---|---|---|---|
| A.5.1 | Policies for information security | Yes | Required by the ISMS. Addresses governance risk. | Implemented | POL-001 Information Security Policy v3.1 | CISO | 17/04/2026 |
| A.5.2 | Information security roles and responsibilities | Yes | Defines accountability across the ISMS. | Implemented | POL-001 §4; RACI matrix RACI-002 | CISO | 17/04/2026 |
| A.5.3 | Segregation of duties | Yes | Prevents fraud and error in privileged operations. | Implemented | IAM-PROC-004; quarterly access review | Head of IT | 17/04/2026 |
| A.5.7 | Threat intelligence | Yes | Covers emerging threats relevant to a SaaS estate. | Partially implemented | Subscribed to CERT-In and vendor feeds; no formal triage SOP yet | Security Operations Lead | 17/04/2026 |
| A.5.19 | Information security in supplier relationships | Yes | Addresses supply-chain risk (R-001, R-007). | Implemented | Vendor Management Policy POL-012; due-diligence checklist | Procurement Lead | 17/04/2026 |
| A.5.21 | Managing ICT supply chain | Yes | Single-vendor concentration risk. | Partially implemented | Tier-1 vendor list maintained; failover plans in progress | Procurement Lead | 17/04/2026 |
| A.5.23 | Information security for use of cloud services | Yes | Primary deployment is AWS; cloud-specific controls required. | Implemented | Cloud Security Standard STD-003 | Head of Infrastructure | 17/04/2026 |
| A.5.30 | ICT readiness for business continuity | Yes | Required by the BCP programme. | Partially implemented | DR runbook RUN-009; restore test pending (R-006) | Head of Infrastructure | 17/04/2026 |
| A.5.31 | Legal, statutory, regulatory and contractual requirements | Yes | DPDP, GDPR and contractual obligations apply. | Implemented | Compliance Register REG-001 | DPO | 17/04/2026 |
| A.5.34 | Privacy and protection of PII | Yes | Personal data is core to the processing we do. | Implemented | Privacy Policy POL-005; ROPA REG-002 | DPO | 17/04/2026 |

### A.6 People controls (8 controls)

| Control ID | Control Name | Applicable? | Justification | Implementation Status | Implementation Reference | Control Owner | Last Reviewed |
|---|---|---|---|---|---|---|---|
| A.6.1 | Screening | Yes | Background checks required pre-hire. | Implemented | HR-PROC-002 | Head of People | 17/04/2026 |
| A.6.3 | Information security awareness, education and training | Yes | All staff handle data; training mandatory. | Implemented | Annual mandatory training; phishing simulations quarterly | Head of People | 17/04/2026 |
| A.6.4 | Disciplinary process | Yes | Consequence management for policy breaches. | Implemented | HR-POL-006 | Head of People | 17/04/2026 |
| A.6.7 | Remote working | Yes | Workforce is hybrid. | Implemented | Remote Working Policy POL-009 | Head of People | 17/04/2026 |

### A.7 Physical controls (14 controls)

| Control ID | Control Name | Applicable? | Justification | Implementation Status | Implementation Reference | Control Owner | Last Reviewed |
|---|---|---|---|---|---|---|---|
| A.7.1 | Physical security perimeters | Yes | Office hosts admin staff; managed access required. | Implemented | Building access control; visitor log | Office Manager | 17/04/2026 |
| A.7.4 | Physical security monitoring | Yes | CCTV and access logs in office. | Implemented | CCTV log retained 90 days | Office Manager | 17/04/2026 |
| A.7.9 | Security of assets off-premises | Yes | Laptops issued; MDM enrolled. | Implemented | MDM enforced; full-disk encryption | Head of IT | 17/04/2026 |
| A.7.14 | Secure disposal or re-use of equipment | Yes | End-of-life devices contain data. | Implemented | Certified disposal vendor; certificates retained | Head of IT | 17/04/2026 |

### A.8 Technological controls (34 controls)

| Control ID | Control Name | Applicable? | Justification | Implementation Status | Implementation Reference | Control Owner | Last Reviewed |
|---|---|---|---|---|---|---|---|
| A.8.1 | User end point devices | Yes | All staff use endpoints. | Implemented | EDR deployed; MDM enforced | Head of IT | 17/04/2026 |
| A.8.5 | Secure authentication | Yes | Authentication is critical (R-002). | Partially implemented | MFA deployed; not yet enforced for all users | CISO | 17/04/2026 |
| A.8.7 | Protection against malware | Yes | Endpoint and email risks. | Implemented | EDR + email gateway scanning | Security Operations Lead | 17/04/2026 |
| A.8.8 | Management of technical vulnerabilities | Yes | Vulnerability lifecycle required. | Implemented | Quarterly internal scans; annual pen-test | AppSec Lead | 17/04/2026 |
| A.8.13 | Information backup | Yes | Required for DR. | Partially implemented | Daily backups taken; restore test pending (R-006) | SRE Lead | 17/04/2026 |
| A.8.15 | Logging | Yes | Required for detection and investigation. | Implemented | Centralised SIEM; 1-year retention | Security Operations Lead | 17/04/2026 |
| A.8.23 | Web filtering | Yes | Reduces malware and phishing risk. | Implemented | DNS-layer filtering | Head of IT | 17/04/2026 |
| A.8.24 | Use of cryptography | Yes | TLS 1.2+ for all data in transit; AES-256 at rest. | Implemented | Cryptography Standard STD-005 | CISO | 17/04/2026 |
| A.8.25 | Secure development life cycle | Yes | In-house development. | Implemented | SDLC standard STD-007; security gates in CI/CD | CTO | 17/04/2026 |
| A.8.28 | Secure coding | Yes | Developers produce code that handles personal data. | Partially implemented | Coding standards published; training inconsistent | AppSec Lead | 17/04/2026 |
| A.8.29 | Security testing in development and acceptance | Yes | Required to catch vulnerabilities pre-production. | Partially implemented | SAST in CI; DAST quarterly; pen-test annual | AppSec Lead | 17/04/2026 |

---

## Example exclusion (with valid justification)

| Control ID | Control Name | Applicable? | Justification |
|---|---|---|---|
| A.7.10 | Storage media | No | Organisation does not use removable storage media. USB ports disabled by MDM policy POL-014. No business process requires media transfer. |

> Auditors will challenge exclusions. The justification must be defensible in writing and consistent with how the organisation actually operates. If you exclude A.7.10 but a developer hands a USB drive to a contractor, you have a non-conformity.

---

## Approval

This Statement of Applicability is reviewed annually and after any material change to the ISMS scope, significant risk treatment decision, or organisational restructure.

| Role | Name | Signature | Date |
|---|---|---|---|
| ISMS Manager | | | |
| CISO | | | |
| Top Management Representative | | | |

---

*The full SoA covers all 93 Annex A controls. The above is a representative sample. Keep the operational version in your GRC tool or a controlled spreadsheet, version-locked, with change history.*

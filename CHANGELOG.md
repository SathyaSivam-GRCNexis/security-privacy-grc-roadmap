# Changelog

All notable changes to this repository are recorded here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this repository follows a date-led versioning scheme: each release is dated, and minor edits between releases are tracked by commit.

---

## [1.0.0] — 17 April 2026

### Added

- Initial public structure of the repository.
- Top-level entry points: `README.md`, `START-HERE.md`, `faq.md`.
- Persona-specific learning paths in `paths/`:
  - `00-non-tech-beginner.md` (10 to 14 weeks, 80 to 100 hours).
  - `01-career-shifter.md` (16 to 20 weeks, 140 to 180 hours).
  - `02-tech-professional.md` (10 to 14 weeks, 100 to 130 hours).
- All 18 modules from the source course material in `modules/`:
  - 00 Foundation pack
  - 01 Security first principles
  - 02 Cryptography for humans
  - 03 Identity and access
  - 04 Network security
  - 05 Privacy fundamentals
  - 06 Privacy laws
  - 07 GRC frameworks
  - 08 Risk management
  - 09 Audit lifecycle
  - 10 Policies
  - 11 Incident response and BCP
  - 12 Vendor risk
  - 13 Cloud security
  - 14 Application security
  - 15 Emerging topics (AI, quantum, supply chain)
  - 16 Careers and interviews
  - 17 Practice scenarios and capstones
- Reference materials in `reference/`:
  - `glossary.md` — alphabetised plain-English definitions.
  - `acronyms.md` — A to Z acronym dictionary.
  - `frameworks-cross-map.md` — 33-row cross-map across SOC 2, ISO 27001:2022, NIST CSF 2.0, NIST 800-53 Rev 5, PCI-DSS v4, and HIPAA.
  - `regulator-clocks.md` — breach clocks, DSR clocks, and audit cycles for major regulators.
  - `free-resources.md` — curated free learning resources (preserved from source appendix).
- Interview preparation in `interview-prep/`:
  - `question-bank.md` — approximately 116 questions across role buckets.
  - `30-second-drills.md` — approximately 85 rapid-fire interview drills.
  - `star-and-c4r-frameworks.md` — answer frameworks with worked examples.
  - `india-market-snapshot.md` — employers, salary bands, and demand trends.
- Practice scenarios scaffolding in `scenarios/`:
  - `README.md` — usage guide and portfolio framing.
  - `templates/risk-register.csv` — pre-populated risk register.
  - `templates/dpia-template.md` — Data Protection Impact Assessment template.
  - `templates/statement-of-applicability.md` — ISO 27001 SoA template with sample rows.
  - `templates/ir-runbook.md` — incident response runbook (credential-stuffing variant).
  - `templates/vendor-assessment.md` — full vendor security and privacy assessment.
  - `templates/policy-template.md` — generic policy template.
  - `templates/audit-evidence-pack.md` — per-control evidence pack template.
- Repository governance:
  - `CONTRIBUTING.md` — voice, structure, and contribution rules.
  - `LICENSE` — dual licence (CC BY-SA 4.0 for content; MIT for templates).
  - `CODE_OF_CONDUCT.md`.
  - `.gitignore`.
  - `.github/ISSUE_TEMPLATE/content-error.md`.
  - `.github/ISSUE_TEMPLATE/new-topic-request.md`.
  - `.github/PULL_REQUEST_TEMPLATE.md`.

### Notes

- All content is current as at 17 April 2026. Regulatory references (DPDP Act 2023, GDPR, CERT-In Directions 2022, ISO 27001:2022, NIST CSF 2.0, PCI-DSS v4) reflect the position on that date.
- The repository was published privately at first to allow content review before opening to the public.

---

## How releases will be numbered

- **Major (X.0.0)** — restructure of the repository, removal or replacement of modules, or material changes to the persona paths.
- **Minor (1.X.0)** — new modules, new templates, or significant rewrites of existing material.
- **Patch (1.0.X)** — corrections, link fixes, regulatory updates that do not change structure.

Between numbered releases, the commit history is the source of truth.

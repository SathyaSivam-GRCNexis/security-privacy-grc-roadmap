# Contributing

Thanks for considering a contribution. This roadmap exists because GRC, security, and privacy resources are either too academic to be useful or too vendor-flavoured to be trusted. The goal is a practitioner's reference that holds up under interview pressure and survives contact with a real audit. Help me keep it that way.

---

## What I will accept

- **Corrections.** A factual error, an out-of-date regulation, a broken link, a typo. File these as issues or PRs. Always welcome.
- **Updates to regulators, frameworks, and clocks.** Where a control reference, a deadline, or a regulator's process has changed since publication, send the change with a citation.
- **New worked examples.** If you have run a real-world scenario (sanitised) that illustrates a concept better than what is here, send it.
- **Translations.** I am open to translations of the modules into other languages, provided the translator commits to maintaining the file alongside the English version for at least 12 months.
- **New free resources.** For `reference/free-resources.md`, I accept additions that are genuinely free (not freemium-with-a-trap) and high-quality. No vendor blogs masquerading as guides.

## What I will not accept

- **Promotional content.** No vendor pitches, no "here is why my product solves this," no SEO link-building. PRs of this nature are closed without comment.
- **Certification cram material.** This is not a CISSP / CISA / CIPP exam dump. There are better resources for that.
- **AI-generated drafts pasted in unedited.** You can use AI to help you think and draft. You cannot submit raw model output. I can tell, and so can readers.
- **Style overhauls.** Please do not "modernise" the prose, add emoji, or restructure modules to fit a different teaching framework. The voice is deliberate. See "Voice and style" below.

---

## Voice and style

The writing in this repository follows specific conventions. Match them in any contribution.

- **British English.** Organisation, recognised, behaviour, centre, programme. Not organization, recognized, behavior, center, program. (Quoted regulator names retain their original spelling, e.g., NIST CSF, FedRAMP.)
- **Direct second person.** "You will encounter," not "one might encounter."
- **Plain English over jargon.** When a technical term is unavoidable, define it the first time it appears and add it to [`reference/glossary.md`](reference/glossary.md) and [`reference/acronyms.md`](reference/acronyms.md).
- **Short sentences.** If a sentence is over 25 words, it can almost always be split.
- **No em-dashes.** Use commas, full stops, or " — " (spaced en-dash) sparingly.
- **No emoji** except the persona markers (🟢 🟡 🔴) and the resource-type legend (🏛 📘 📰 🎥 🧪 💰).
- **Honest about trade-offs.** Where a topic has competing schools of thought, name them and pick one with reasoning. Do not paper over disagreement.
- **No marketing voice.** Phrases like "leverage," "best-in-class," "robust solutions," and "in today's rapidly evolving landscape" are auto-rejected.

A useful test before submitting: read your contribution aloud. If it sounds like a webinar transcript, rewrite it.

---

## Module structure

If you contribute a new module or a substantial section, follow the established structure:

1. **Why this matters.** One paragraph. Real-world stake.
2. **Core definitions.** Plain English first, formal second.
3. **Analogy.** A concrete, non-technical comparison.
4. **Concept teaching.** The substantive content.
5. **Worked example.** Where possible, reuse the recurring fictional EdTech platform that appears across Modules 1, 5, 6, 8, and 10, so readers build cumulative familiarity. If your topic does not fit that example, pick a clearly fictional company and stay consistent within the module.
6. **Comparison table.** Where relevant.
7. **Beginner mistakes.** What people get wrong.
8. **Interview traps.** What hiring managers test for.
9. **Mini-exercise.** Something the reader can do in 30 to 60 minutes.
10. **Go deeper.** Curated links with the resource-type legend (🏛 official, 📘 book, 📰 article, 🎥 video, 🧪 hands-on, 💰 paid).
11. **Glossary recap.** Terms introduced in the module.
12. **Next pointer.** A one-line bridge to the next module.

---

## How to submit

### For small fixes (typos, broken links, single-line corrections)

Open a PR directly. Title it `fix: <short description>`. One PR per fix.

### For larger contributions (new sections, updated tables, new examples)

1. Open an issue first using the **New topic request** template. Describe what you want to add and why. This avoids you doing significant work on something I have already declined or planned differently.
2. Wait for a response. I usually reply within a week.
3. If accepted, branch from `main`, name the branch `feat/<short-description>`, and open the PR when ready.

### PR checklist

Before opening a PR, confirm:

- [ ] Spelling is British English.
- [ ] No em-dashes; voice matches the existing tone.
- [ ] New terms added to `glossary.md` and `acronyms.md` if relevant.
- [ ] Cross-references use relative links (e.g., `../modules/06-privacy-laws.md`), not absolute URLs.
- [ ] External links are to authoritative sources (regulator, standards body, recognised publisher, recognised practitioner blog). No marketing pages.
- [ ] Dates, where present, are in `DD MonthName YYYY` format (e.g., `17 April 2026`).
- [ ] You have read the section you are editing in full before changing it.
- [ ] You have not introduced AI-generated text without rewriting it in your own voice.
- [ ] If your change affects a reference table (regulator clocks, frameworks cross-map), the supporting citation is in the PR description.

PRs that do not meet the checklist will be commented on, not closed. We will work it out.

---

## Reporting content errors

If you find an error and do not want to open a PR, file an issue using the **Content error** template. Be specific:

- File and section.
- The current (incorrect) statement.
- The correct statement.
- A citation for the correction.

Reports without a citation are still welcome but will be slower to action.

---

## Code of conduct

Contributions are governed by the [Code of Conduct](CODE_OF_CONDUCT.md). The summary: be a serious adult, disagree with ideas not people, and assume good faith until repeatedly proven otherwise.

---

## Attribution and licensing

By contributing, you agree that your contribution is licensed under the same terms as the rest of the repository:

- Prose, modules, and reference material: **CC BY-SA 4.0**.
- Templates in `scenarios/templates/`: **MIT**.

You retain authorship; you grant the licence. See [LICENSE](LICENSE) for the full text.

If your contribution is substantial (new module, significant rewrite of a section), you will be named in the module's footer and in the changelog. Smaller contributions are acknowledged in commit history.

---

## Questions

If anything here is unclear, open an issue tagged `question` or reach out via [LinkedIn](https://www.linkedin.com/in/sathya-sivam). I do not have a fixed turnaround but I read everything.

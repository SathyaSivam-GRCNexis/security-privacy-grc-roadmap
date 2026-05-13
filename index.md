---
layout: default
title: Overview
---

# Overview

A free, opinionated, India-aware curriculum for getting into Security, Privacy, or GRC. Built around the questions hiring managers actually ask, and the artefacts auditors actually open.

If you are not sure where to start, read [Start Here]({{ '/START-HERE/' | relative_url }}) first. Five minutes, and you will know which path is yours.

<h1 id="paths">Persona Paths</h1>

Three kinds of people pick up this material. The route is different for each. Pick wrong and you will waste a month on the wrong modules.

<div class="project-grid">
{% for p in site.paths %}
  <a class="project-card" href="{{ p.slug | append: '/' | relative_url }}">
    <div class="head"><span class="num">{{ p.num }}</span><span class="cat">Path</span></div>
    <h3>{{ p.title }}</h3>
    <p><strong>{{ p.who }}.</strong><br>{{ p.focus }}</p>
    <span class="open">Open path &rarr;</span>
  </a>
{% endfor %}
</div>

<h1 id="modules">Modules</h1>

Eighteen modules, in order. Each is self-contained: concepts, real-world context, the interview traps that catch people out, and free resources. Skip around if you must, but the sequencing exists for a reason.

<div class="project-grid">
{% for m in site.modules %}
  <a class="project-card" href="{{ m.slug | append: '/' | relative_url }}">
    <div class="head"><span class="num">{{ m.num }}</span><span class="cat">{{ m.group }}</span></div>
    <h3>{{ m.title }}</h3>
    <p>Module {{ m.num }} of 17.</p>
    <span class="open">Open module &rarr;</span>
  </a>
{% endfor %}
</div>

<h1 id="reference">Reference</h1>

The pages I keep open in a tab during the working day. Bookmark them.

<div class="project-grid">
{% for r in site.references %}
  <a class="project-card" href="{{ r.slug | append: '/' | relative_url }}">
    <div class="head"><span class="cat">Reference</span></div>
    <h3>{{ r.title }}</h3>
    <p>{{ r.note }}</p>
    <span class="open">Open reference &rarr;</span>
  </a>
{% endfor %}
</div>

<h1 id="interview-prep">Interview Prep</h1>

The pieces hiring managers actually probe. Drills, answer structures, and the India market reality with numbers attached.

<div class="project-grid">
{% for i in site.interview %}
  <a class="project-card" href="{{ i.slug | append: '/' | relative_url }}">
    <div class="head"><span class="cat">Interview</span></div>
    <h3>{{ i.title }}</h3>
    <p>{{ i.note }}</p>
    <span class="open">Open &rarr;</span>
  </a>
{% endfor %}
</div>

# Coverage scope

Current to **April 2026**. Every framework and regulation is cited with version numbers, because in my experience that is what gets you through an interview. "ISO 27001" is a wrong answer. "ISO 27001:2022, Annex A is now 93 controls organised into four themes" is the right one.

**Privacy and data protection:** GDPR (EU), UK GDPR, DPDP Act 2023 + DPDP Rules 2025 (India), CCPA/CPRA (California), HIPAA (US healthcare), COPPA, FERPA, PCI-DSS 4.0.1.

**GRC frameworks and standards:** SOC 2 (TSC 2017, revised 2022), ISO/IEC 27001:2022, ISO/IEC 27701:2019, ISO/IEC 27017, ISO/IEC 27018, ISO/IEC 22301:2019, ISO/IEC 42001:2023, NIST CSF 2.0, NIST SP 800-53 Rev 5, NIST SP 800-171 Rev 3, NIST AI RMF 1.0 + Generative AI Profile.

**Technical and operational:** OWASP Top 10 (Web 2021, API 2023, LLM 2025), MITRE ATT&CK, MITRE ATLAS, CIS Controls v8.1, CISA Zero Trust Maturity Model 2.0, FIPS 140-3, FIPS 203/204/205 (post-quantum cryptography).

**Regulatory landmarks:** EU AI Act (in force August 2024, GPAI obligations August 2025), SEC Cybersecurity Disclosure Rule (Item 1.05, 4-business-day clock), CERT-In Directions 2022 (6-hour incident reporting in India), RBI Cybersecurity Framework, SEBI CSCRF, DORA (EU).

# What you will produce

By the time you finish, your public GitHub will hold real artefacts. Not certificates of attendance.

- A populated **risk register** with owners, scores, treatment plans, review dates
- A filled **DPIA** for a realistic scenario
- A **Statement of Applicability** showing which controls you implemented and why
- An **incident response runbook** for at least one playbook
- A **vendor risk assessment** with tiering and a real questionnaire response
- A **policy set** mapped to ISO 27001:2022 and SOC 2
- An **audit evidence pack** that an auditor could actually open

This is the stuff hiring managers ask to see. Not your cert count. In my experience, a candidate who can walk through one populated risk register beats three who only have CC and Security+ on the wall.

# What this is not

- Not a cert dump. Certifications open doors. The job is still about chasing evidence.
- Not a tool tutorial. Tools change every 18 months. The underlying concepts do not.
- Not a CTF. Capture-the-flag is fun, and unrelated to most security and GRC work.
- Not US-only. India, EU, and UK regulatory contexts are first-class throughout.
- Not affiliate-link bait. There is nothing to sell you.
- Not chatbot output. Written by someone who has sat through audits and argued with engineering.

# Maintenance

References are re-verified every quarter. Last full check: **17 April 2026**. If you find a stale link, an outdated regulation, or a framework that has shipped a new version, open an [issue](https://github.com/{{ site.repository }}/issues). Regulators move. This page tries to keep up.

> *Compliance is the floor, not the ceiling. Passing an audit means you met the minimum someone else wrote down. It does not mean you are safe.*

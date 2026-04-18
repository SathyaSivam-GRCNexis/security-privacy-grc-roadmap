# Module 4 — Networks & Infrastructure Security

> **Audience:** 🟡 🔴 · **Time:** ~75 min · **Prereqs:** Modules 0–3

## Why this matters

Even in the cloud era, **every attack still travels over a network**. Understanding network security lets you reason about how data moves, where it can be intercepted, and how controls like firewalls and segmentation actually stop attacks. This is also a recurring audit area — "describe your network segmentation" is on almost every questionnaire.

---

## 4.1 A fast recap of networking for security

- **OSI model** — 7 layers (Physical → Data Link → Network → Transport → Session → Presentation → Application). You only need to remember 1, 2, 3, 4, and 7 day-to-day.
- **Layer 2:** MAC addresses, switches, VLANs.
- **Layer 3:** IP addresses, routers, subnets.
- **Layer 4:** TCP (reliable, ordered, slow-to-setup) and UDP (fast, lossy).
- **Layer 7:** applications — HTTP, DNS, SSH, SMTP.

Different controls operate at different layers:

| Control | Layer it operates at |
|---------|---------------------|
| MAC filtering | 2 |
| Network ACL / stateless firewall | 3/4 |
| Stateful firewall / Security Group | 3/4 |
| WAF (Web Application Firewall) | 7 |
| IDS/IPS (signature-based or ML) | 3–7 |
| DLP (Data Loss Prevention) | 7 (content) |

Knowing which layer a control lives at tells you what it can and can't see.

---

## 4.2 Network segmentation — the single highest-value design decision

**Segmentation** = dividing your network into zones with controlled traffic between them. When (not if) an attacker lands on one box, segmentation limits how far they spread ("lateral movement").

### Classic zones

- **Public internet** — untrusted.
- **DMZ (Demilitarised Zone)** — public-facing services (web, email), isolated from internal.
- **Internal / corporate** — employee workstations, file shares.
- **Production** — customer-serving workloads.
- **Management** — admin consoles, bastion hosts.
- **Data / crown jewels** — databases, secret stores, backups.

Traffic between zones is controlled by firewalls and should default to **deny**.

### Micro-segmentation

Instead of big zones, apply firewall rules between individual workloads. Typical in Kubernetes (NetworkPolicies), service meshes (Istio, Linkerd), and zero-trust network architectures.

### Worked example: lateral movement blocked by segmentation

Attacker compromises a marketing web server via a plugin vuln. Without segmentation, that server can reach the customer database over the internal network — catastrophic. With micro-segmentation, the web server is allowed to talk only to its own backend API on a single port, not the DB. The blast radius is one server.

---

## 4.3 Firewalls, IDS, IPS, WAF — know the difference

| Tool | What it does | Example |
|------|--------------|---------|
| **Firewall** | Allow/block traffic based on rules (IP, port, protocol). | iptables, pf, AWS Security Group, Cisco ASA |
| **IDS (Intrusion Detection System)** | Watches traffic; **alerts** on suspicious patterns. | Snort, Suricata, Zeek |
| **IPS (Intrusion Prevention System)** | Same as IDS but **blocks** inline. | Snort inline mode, Palo Alto |
| **WAF (Web Application Firewall)** | Understands HTTP; blocks web attacks (SQLi, XSS). | AWS WAF, Cloudflare, ModSecurity |
| **NGFW (Next-Gen Firewall)** | Firewall + IDS/IPS + app-awareness in one box. | Palo Alto PA-series, Fortinet |
| **DDoS protection** | Absorbs flood attacks. | Cloudflare, AWS Shield, Akamai |
| **DLP (Data Loss Prevention)** | Detects/blocks sensitive data leaving. | Symantec DLP, Microsoft Purview |

Stateful firewalls track connection state (the returning traffic of a TCP session is automatically allowed); stateless firewalls evaluate each packet independently.

### Signature- vs behaviour-based detection

- **Signature-based** — matches known patterns. Fast, predictable, blind to new attacks.
- **Anomaly / behaviour-based** — detects deviations from baseline. Catches novel attacks; noisy.
- **Modern approach:** combine both, plus threat-intel feeds, plus ML classifiers.

---

## 4.4 VPNs, bastions, and zero trust

### VPN

Encrypts traffic between a user and the corporate network. Gives the user an IP inside the corporate network. Strong for legacy apps, weak when attackers have stolen credentials (they inherit full network access).

### Bastion / jump host

A hardened, narrowly-scoped host that admins connect to first, then hop to internal servers. Ideally:
- MFA required.
- Session recorded.
- Inbound from specific IPs only.
- Ephemeral (no persistent state).

Tools: AWS Systems Manager Session Manager, Azure Bastion, GCP IAP, Teleport.

### Zero Trust (NIST SP 800-207)

Principle: **never trust, always verify** — trust is evaluated per request, not per network location. Implications:

- Internal network is *not* trusted by default.
- Every request is authenticated + authorised.
- Micro-segmentation.
- Continuous evaluation (device posture, user risk, session context).
- Perimeter = identity + device + context, not IP ranges.

Practical implementations: BeyondCorp (Google), Cloudflare Zero Trust, Zscaler, Azure Entra Private Access.

### Mini-exercise

Draw your organisation's network on one page: internet, DMZ, internal, prod, data. Where are the trust boundaries? What controls enforce them? Score each boundary 1–5 for strength.

---

## 4.5 Wi-Fi, mobile, and physical

- **WPA3** is the current Wi-Fi standard. WPA2 is acceptable; WEP and open Wi-Fi are not.
- **Enterprise Wi-Fi** uses 802.1X with a RADIUS server tied to your IdP (per-user credentials, revocable).
- **Guest networks** must be strictly isolated from corporate.
- **Rogue access points** — a cheap router someone plugs in. Detect with wireless intrusion detection.

Mobile device management (MDM) enforces device posture — disk encryption, passcode, OS patch level, remote wipe.

Physical security: badge access, visitor logs, server-room controls, cable locks, USB restrictions. Don't let physical access rot — an attacker with a data-centre badge beats a lot of software controls.

---

## 4.6 Endpoint security

Endpoints (laptops, mobiles, servers) are the last mile.

Stack:
- **EDR (Endpoint Detection & Response)** — CrowdStrike, SentinelOne, Microsoft Defender. Goes beyond old AV with behaviour analysis, rollback.
- **Disk encryption** — FileVault, BitLocker, LUKS.
- **OS hardening** — apply CIS Benchmarks, disable unused services.
- **Patch management** — defined SLAs (critical in 7 days, high in 30, etc.).
- **Application allowlisting** — only approved apps can run. Effective but operationally heavy.

---

## 4.7 DNS security

DNS is the weakest link in plain-text internet. Attacks:
- **Cache poisoning** — inject fake records.
- **Hijacking** — compromise registrar, point domain elsewhere.
- **Typosquatting / homoglyphs** — `amazón.com`, `micros0ft.com`.
- **DNS tunnelling** — exfiltrate data in DNS queries.

Controls:
- **DNSSEC** — signs DNS records cryptographically (adoption patchy).
- **DoH / DoT** — DNS over HTTPS / TLS; encrypts DNS queries.
- **Protective DNS** — services like Cisco Umbrella, Cloudflare Gateway — block known-malicious domains.
- **Registrar lock** — requires extra steps to transfer domain (Google "registrar lock" for yours — you should have it).

---

## 4.8 Email security

Email is still the top initial access vector. Three related SMTP standards you *must* configure on your domain:

- **SPF (Sender Policy Framework)** — which servers may send mail for your domain.
- **DKIM (DomainKeys Identified Mail)** — cryptographically signs outgoing mail.
- **DMARC** — policy on what to do when SPF/DKIM fail + reporting.

Deploy all three in reject mode (`p=reject`) to block domain spoofing. Tools: free checkers at [MXToolbox](https://mxtoolbox.com/), [DMARC Analyzer](https://www.dmarcanalyzer.com/).

Plus: secure email gateway (SEG) for inbound filtering, BEC (business-email-compromise) detection, user phishing training.

---

## 4.9 Logging, monitoring, and SIEM (preview — more in Module 11)

No control is trusted without logs. You need:
- **Host logs** — OS, auth, process execution.
- **Network logs** — flow logs, DNS, proxy.
- **App logs** — business events, errors.
- **Cloud logs** — CloudTrail, Azure Activity, GCP Audit.
- **Identity logs** — IdP sign-ins, MFA challenges, admin changes.

Ship to a central **SIEM** (Splunk, Elastic, Sentinel, Sumo Logic, Chronicle) with retention of 1 year hot + 1–7 years cold (regulation-driven). Correlate into alerts; track **MTTD / MTTR** (mean time to detect / respond).

---

## 4.10 Go deeper

- 🏛 [NIST SP 800-207 — Zero Trust Architecture](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-207.pdf)
- 🏛 [CISA Zero Trust Maturity Model 2.0](https://www.cisa.gov/zero-trust-maturity-model)
- 📘 [Professor Messer — Network+ videos (free)](https://www.professormesser.com/)
- 🧪 [TryHackMe — Network Fundamentals path (free rooms)](https://tryhackme.com/)
- 📰 [Cloudflare Learning — Zero Trust hub](https://www.cloudflare.com/learning/access-management/what-is-zero-trust/)
- 📰 [SANS — Network Security Reading Room](https://www.sans.org/white-papers/?focus-area=network-security)
- 🧪 [DNS checkup — dnsviz.net](https://dnsviz.net/) · [MXToolbox SPF/DKIM/DMARC](https://mxtoolbox.com/)

---

## Module 4 — Glossary recap

OSI model, TCP, UDP, VLAN, Subnet, DMZ, Micro-segmentation, Lateral movement, Firewall (stateful/stateless), NACL, Security Group, IDS, IPS, WAF, NGFW, DLP, DDoS, Signature vs anomaly detection, VPN, Bastion / jump host, Zero Trust, BeyondCorp, WPA3, 802.1X, RADIUS, MDM, EDR, Disk encryption, CIS Benchmarks, DNS, DNSSEC, DoH, DoT, Protective DNS, Registrar lock, SPF, DKIM, DMARC, BEC, SIEM, MTTD, MTTR.

→ Next: [Module 5 — Privacy Fundamentals](05-privacy-fundamentals.md)

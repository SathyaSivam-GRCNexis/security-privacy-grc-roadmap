# Module 4, Networks & Infrastructure Security

> **Audience:** 🟡 🔴 · **Time:** ~75 min · **Prereqs:** Modules 0–3

## Why this matters

The old model was a fortress with a perimeter. That model is mostly dead. Today your "network" is a mesh of SaaS apps, cloud VPCs, contractors' laptops and somebody's tablet on a hotel Wi-Fi. The good news: most attacks still travel over a network, so the basics still apply. The bad news: "set up a firewall and you're done" hasn't been true for a decade.

Strong opinion: stop thinking perimeter, start thinking **segmentation** and **identity**. Cloud-first reality is that you assume the attacker is already inside somewhere, and you make sure they can't move sideways to anything that matters. Almost every audit questionnaire still asks "describe your network segmentation", so you also need the words.

---

## 4.1 A fast recap of networking for security

- **OSI model.** Seven layers (Physical → Data Link → Network → Transport → Session → Presentation → Application). You only need 1, 2, 3, 4 and 7 day-to-day.
- **Layer 2.** MAC addresses, switches, VLANs.
- **Layer 3.** IP addresses, routers, subnets.
- **Layer 4.** TCP (reliable, ordered, slow-to-setup) and UDP (fast, lossy).
- **Layer 7.** Applications. HTTP, DNS, SSH, SMTP.

Different controls operate at different layers:

| Control | Layer it operates at |
|---------|---------------------|
| MAC filtering | 2 |
| Network ACL / stateless firewall | 3/4 |
| Stateful firewall / Security Group | 3/4 |
| WAF (Web Application Firewall) | 7 |
| IDS/IPS (signature-based or ML) | 3–7 |
| DLP (Data Loss Prevention) | 7 (content) |

Knowing which layer a control sits at tells you what it can and can't see. A network ACL can't read a JSON payload. A WAF can.

---

## 4.2 Network segmentation, the single highest-value design decision

Segmentation means dividing your network into zones with controlled traffic between them. When (not if) an attacker lands on one box, segmentation limits how far they spread. The term you'll hear is "lateral movement".

### Classic zones

- **Public internet.** Untrusted.
- **DMZ (Demilitarised Zone).** Public-facing services (web, email), isolated from internal.
- **Internal / corporate.** Employee workstations, file shares.
- **Production.** Customer-serving workloads.
- **Management.** Admin consoles, bastion hosts.
- **Data / crown jewels.** Databases, secret stores, backups.

Traffic between zones is controlled by firewalls and should default to deny.

### Micro-segmentation

Instead of big zones, apply firewall rules between individual workloads. Typical in Kubernetes (NetworkPolicies), service meshes (Istio, Linkerd) and zero-trust network architectures.

Honest take: micro-segmentation is glorious on a slide and painful in operations. In one SaaS environment I worked in, the initial NetworkPolicy rollout broke three internal services because nobody had inventoried which pod talked to which. Start with deny-all at the namespace level and open ports as you discover them, ideally in audit mode first.

### Worked example, lateral movement blocked by segmentation

Attacker compromises a marketing web server via a plugin vulnerability. Without segmentation, that server can reach the customer database over the internal network. Catastrophic. With micro-segmentation, the web server is allowed to talk only to its own backend API on a single port. Not the DB. Blast radius: one server.

---

## 4.3 Firewalls, IDS, IPS, WAF, know the difference

| Tool | What it does | Example |
|------|--------------|---------|
| **Firewall** | Allow/block traffic based on rules (IP, port, protocol). | iptables, pf, AWS Security Group, Cisco ASA |
| **IDS (Intrusion Detection System)** | Watches traffic. Alerts on suspicious patterns. | Snort, Suricata, Zeek |
| **IPS (Intrusion Prevention System)** | Same as IDS but blocks inline. | Snort inline mode, Palo Alto |
| **WAF (Web Application Firewall)** | Understands HTTP. Blocks web attacks (SQLi, XSS). | AWS WAF, Cloudflare, ModSecurity |
| **NGFW (Next-Gen Firewall)** | Firewall plus IDS/IPS plus app-awareness in one box. | Palo Alto PA-series, Fortinet |
| **DDoS protection** | Absorbs flood attacks. | Cloudflare, AWS Shield, Akamai |
| **DLP (Data Loss Prevention)** | Detects or blocks sensitive data leaving. | Symantec DLP, Microsoft Purview |

Stateful firewalls track connection state (return traffic of a TCP session is automatically allowed). Stateless firewalls evaluate each packet independently.

### Signature vs behaviour-based detection

- **Signature-based.** Matches known patterns. Fast, predictable, blind to new attacks.
- **Anomaly / behaviour-based.** Detects deviations from baseline. Catches novel attacks. Noisy.
- **Modern approach.** Combine both, plus threat-intel feeds, plus ML classifiers.

In practice, anomaly-based detection without good baselining produces alert fatigue. Get the SOC to tune signatures first. Behaviour analytics is a year-two project.

---

## 4.4 VPNs, bastions and zero trust

### VPN

Encrypts traffic between a user and the corporate network. Gives the user an IP inside the corporate network. Strong for legacy apps, weak when attackers have stolen credentials, because they inherit full network access. Treat a corporate VPN as one factor, not a security boundary.

### Bastion / jump host

A hardened, narrowly-scoped host that admins connect to first, then hop to internal servers. Ideally:
- MFA required.
- Session recorded.
- Inbound from specific IPs only.
- Ephemeral, no persistent state.

Tools: AWS Systems Manager Session Manager, Azure Bastion, GCP IAP, Teleport.

### Zero Trust (NIST SP 800-207)

Principle: never trust, always verify. Trust is evaluated per request, not per network location. Implications:

- Internal network is not trusted by default.
- Every request is authenticated and authorised.
- Micro-segmentation.
- Continuous evaluation (device posture, user risk, session context).
- Perimeter is identity plus device plus context, not IP ranges.

Practical implementations: BeyondCorp (Google), Cloudflare Zero Trust, Zscaler, Azure Entra Private Access.

Reality check: nobody is "fully zero trust." Every org you'll meet is on a spectrum. CISA's Zero Trust Maturity Model is a useful sanity check when a vendor claims you'll be zero trust by Q3.

### Mini-exercise

Draw your organisation's network on one page: internet, DMZ, internal, prod, data. Where are the trust boundaries? What controls enforce them? Score each boundary 1–5 for strength.

---

## 4.5 Wi-Fi, mobile and physical

- **WPA3** is the current Wi-Fi standard. WPA2 is acceptable. WEP and open Wi-Fi are not.
- **Enterprise Wi-Fi** uses 802.1X with a RADIUS server tied to your IdP. Per-user credentials, revocable.
- **Guest networks** must be strictly isolated from corporate.
- **Rogue access points.** A cheap router someone plugs in. Detect with wireless intrusion detection.

Mobile Device Management (MDM) enforces device posture. Disk encryption, passcode, OS patch level, remote wipe.

Physical security: badge access, visitor logs, server-room controls, cable locks, USB restrictions. Don't let physical access rot. An attacker with a data-centre badge beats a lot of software controls. This matters more for India offices than cloud-only teams sometimes remember.

---

## 4.6 Endpoint security

Endpoints (laptops, mobiles, servers) are the last mile.

Stack:
- **EDR (Endpoint Detection & Response).** CrowdStrike, SentinelOne, Microsoft Defender. Goes beyond old AV with behaviour analysis and rollback.
- **Disk encryption.** FileVault, BitLocker, LUKS.
- **OS hardening.** Apply CIS Benchmarks. Disable unused services.
- **Patch management.** Defined SLAs (critical in 7 days, high in 30, etc.).
- **Application allowlisting.** Only approved apps can run. Effective but operationally heavy.

Common pattern: EDR is rolled out but nobody actually monitors the console. Buying tools without staffing them is the most expensive bad habit in security.

---

## 4.7 DNS security

DNS is the weakest link in plain-text internet. Attacks:
- **Cache poisoning.** Inject fake records.
- **Hijacking.** Compromise the registrar, point the domain elsewhere.
- **Typosquatting / homoglyphs.** `amazón.com`, `micros0ft.com`.
- **DNS tunnelling.** Exfiltrate data in DNS queries.

Controls:
- **DNSSEC.** Signs DNS records cryptographically. Adoption patchy.
- **DoH / DoT.** DNS over HTTPS or TLS. Encrypts DNS queries.
- **Protective DNS.** Services like Cisco Umbrella, Cloudflare Gateway. Block known-malicious domains.
- **Registrar lock.** Requires extra steps to transfer the domain. Google "registrar lock" for yours. You should have it.

If you do nothing else after reading this section, check that your production domain has registrar lock enabled and that the registrar account itself has MFA. Domain takeovers happen, and they're catastrophic.

---

## 4.8 Email security

Email is still the top initial access vector. Three related SMTP standards you must configure on your domain:

- **SPF (Sender Policy Framework).** Which servers may send mail for your domain.
- **DKIM (DomainKeys Identified Mail).** Cryptographically signs outgoing mail.
- **DMARC.** Policy on what to do when SPF/DKIM fail, plus reporting.

Deploy all three in reject mode (`p=reject`) to block domain spoofing. Tools: free checkers at [MXToolbox](https://mxtoolbox.com/), [DMARC Analyzer](https://www.dmarcanalyzer.com/).

Common pattern: SPF and DKIM configured, DMARC stuck at `p=none` for years because nobody owns moving it to quarantine then reject. That's the project. Pick it up. It's a quick GRC win.

Plus: secure email gateway (SEG) for inbound filtering, BEC (business-email-compromise) detection, user phishing training.

---

## 4.9 Logging, monitoring and SIEM (preview, more in Module 11)

No control is trusted without logs. You need:
- **Host logs.** OS, auth, process execution.
- **Network logs.** Flow logs, DNS, proxy.
- **App logs.** Business events, errors.
- **Cloud logs.** CloudTrail, Azure Activity, GCP Audit.
- **Identity logs.** IdP sign-ins, MFA challenges, admin changes.

Ship to a central SIEM (Splunk, Elastic, Sentinel, Sumo Logic, Chronicle) with retention of 1 year hot plus 1–7 years cold, regulation-driven. Correlate into alerts. Track MTTD and MTTR (mean time to detect, mean time to respond).

---

## 4.10 Go deeper

- 🏛 [NIST SP 800-207, Zero Trust Architecture](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-207.pdf)
- 🏛 [CISA Zero Trust Maturity Model 2.0](https://www.cisa.gov/zero-trust-maturity-model)
- 📘 [Professor Messer, Network+ videos (free)](https://www.professormesser.com/)
- 🧪 [TryHackMe, Network Fundamentals path (free rooms)](https://tryhackme.com/)
- 📰 [Cloudflare Learning, Zero Trust hub](https://www.cloudflare.com/learning/access-management/what-is-zero-trust/)
- 📰 [SANS, Network Security Reading Room](https://www.sans.org/white-papers/?focus-area=network-security)
- 🧪 [DNS checkup, dnsviz.net](https://dnsviz.net/) · [MXToolbox SPF/DKIM/DMARC](https://mxtoolbox.com/)

---

## Module 4, Glossary recap

OSI model, TCP, UDP, VLAN, Subnet, DMZ, Micro-segmentation, Lateral movement, Firewall (stateful/stateless), NACL, Security Group, IDS, IPS, WAF, NGFW, DLP, DDoS, Signature vs anomaly detection, VPN, Bastion / jump host, Zero Trust, BeyondCorp, WPA3, 802.1X, RADIUS, MDM, EDR, Disk encryption, CIS Benchmarks, DNS, DNSSEC, DoH, DoT, Protective DNS, Registrar lock, SPF, DKIM, DMARC, BEC, SIEM, MTTD, MTTR.

→ Next: [Module 5, Privacy Fundamentals](05-privacy-fundamentals.md)

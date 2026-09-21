# NOTES — External References & Standards

**Project ID:** NVIDIA-REG-RD-2026-1106  
**Reference Guide:** Links to frameworks, standards, and external resources  
**Status:** INFORMATIONAL

---

## Regulatory Frameworks

### ISO 27001:2022 — Information Security Management System

- **Official Standard:** https://www.iso.org/standard/27001
- **Current Version:** ISO/IEC 27001:2022 (released October 2022)
- **Scope:** Information security controls across 14 categories (Clauses 5–8, Annex A)
- **Reference:** This audit maps findings to ISO 27001:2022 Annex A controls (A.5.1–A.8.24)
- **Key Clauses for This Audit:**
  - A.5.17: Authentication information management
  - A.5.19–5.21: Supplier relationships (supply chain security)
  - A.8.2: Privileged access management
  - A.8.5: Secure authentication (SSH, RADIUS)
  - A.8.13: Backup controls
  - A.8.14: Redundancy / high availability
  - A.8.15: Logging (syslog)
  - A.8.16: Monitoring
  - A.8.20: Network security
  - A.8.22: Segregation of networks (VLANs)

**How to Access:**
- Purchase from ISO: https://www.iso.org/standard/27001 (€198 CHF)
- Free preview/summary: NIST CSF correlation charts online
- This audit references specific clauses; consult the standard for full text

**Audit Mapping:** See `applicability-note.md` (Section 2.1) for full control mapping

---

### GDPR (General Data Protection Regulation)

- **Legal Instrument:** EU Regulation 2016/679
- **Current Date:** December 2023 (as amended; no major changes since 2016 text)
- **Scope:** Personal data protection for EU residents
- **Official Text:** https://eur-lex.europa.eu/eli/reg/2016/679/oj
- **Key Articles for This Audit:**
  - Art. 5: Principles (lawfulness, fairness, transparency, confidentiality, integrity)
  - Art. 30: Records of Processing Activities (data inventory)
  - Art. 32: Technical & organisational measures
  - Art. 33: Breach notification to authority (72 hours)
  - Art. 34: Notification to individuals

**GDPR Article 32 (Technical Measures):**
- Encryption in transit (SSH for management) — **F-GAP-04: PARTIAL**
- Encryption at rest — Out of scope (not documented)
- Pseudonymization/anonymization — Out of scope
- Confidentiality, integrity, resilience — **F-GAP-01: CRITICAL (logging missing)**
- Availability & restoration capability — **F-GAP-07: MEDIUM (backup missing)**

**How to Access:**
- Full text (official EU source): https://eur-lex.europa.eu/eli/reg/2016/679/oj
- Practical guide: EDPB (European Data Protection Board) www.edpb.eu
- Belgium-specific guidance: https://www.autoriteprotectiondonnees.be/

**Audit Mapping:** See `applicability-note.md` (Section 2.2)

---

### NIS2 Directive (Network & Information Security 2)

- **Legal Instrument:** EU Directive 2022/2555
- **Effective Date:** 12 October 2024 (compliance deadline: 12 Oct 2024)
- **Scope:** Network & information security for critical operators (Essential & Important entities)
- **Official Text:** https://eur-lex.europa.eu/eli/dir/2022/2555/oj
- **Belgium Implementation:** Loi relative à la sécurité des réseaux et systèmes d'information (26 April 2024)
  - Belgian text (Justel): https://www.justel.be/

**NIS2 Classification:**
- NVIDIA Regional R&D Hub: **Annex II "Important Entity"** (not Essential)
- Sector: Manufacturing & Research/Technology
- Size: 48 workstations + infrastructure = triggers NIS2 for Important entities

**NIS2 Article 21(2) — Technical Measures (Key for This Audit):**

| Article | Requirement | Audit Finding |
|---|---|---|
| **(b)** | Incident detection & response | F-GAP-01: CRITICAL (no logging) |
| **(c)** | Business continuity & backup | F-GAP-07: MEDIUM (no backup design) |
| **(d)** | Supply chain security | F-GAP-10: MEDIUM (no vendor audit rights) |
| **(h)** | Cryptographic techniques | F-GAP-04: HIGH (SSH incomplete), F-GAP-06: HIGH (cleartext FTP) |
| **(i)** | Access control & authentication | F-GAP-05: HIGH (RADIUS broken, weak credentials) |

**NIS2 Belgium Act (26 April 2024):**
- Amends Belgian law on national security
- Requires "appropriate" security measures (Article 21)
- Monitoring authority: **Centre de Cybersécurité Belgique (CCB)**
- Incident notification: **24 hours** (reduced from 72h in NIS1)

**Contact:** CCB — https://www.cert.be/ (Belgian national cybersecurity center)

**Audit Mapping:** See `applicability-note.md` (Section 2.3)

---

### CyberFundamentals (CyFun) — Belgium

- **Framework:** Belgian de facto implementation framework for NIS2
- **Based On:** NIST Cybersecurity Framework 2.0 (updated 2024)
- **Status:** Reference standard for NIS2 compliance verification in Belgium
- **Official Source:** https://www.cyberfundamentals.be/
- **Access:** Contact CCB or your national cybersecurity authority for detailed documentation

**CyFun 2025 Functions (6 Categories):**
1. **Govern (GV)** — Policies, roles, risk management
2. **Protect (PR)** — Access control, encryption, asset management
3. **Detect (DE)** — Monitoring, logging, incident detection
4. **Respond (RS)** — Incident response, containment, eradication
5. **Recover (RC)** — Recovery procedures, backup/restore, business continuity
6. **Adapt (AD)** — Continuous improvement, lessons learned

**This Audit's CyFun Mapping:**
- Protect: VLAN segmentation, SSH, RADIUS (PR.AC, PR.DS)
- Detect: Syslog logging (DE.AE)
- Respond: Incident response procedure (RS.CO)
- Recover: Backup design (RC.CO)
- Govern: Asset inventory, policies (GV) — out of scope

**Audit Mapping:** See `applicability-note.md` (Section 2.4)

---

### EU AI Act (Mention Only — Out of Scope)

- **Legal Instrument:** EU Regulation 2024/1689
- **Effective Date:** 2 February 2025 (phased implementation)
- **Scope:** Artificial Intelligence systems with "high risk"
- **Official Text:** https://eur-lex.europa.eu/eli/reg/2024/1689/oj
- **Relevance to NVIDIA Hub:** Uncertain
  - Hub mentions "Central-Compute-LLM" server (VLAN 70, 192.168.70.14)
  - **No documentation of what "LLM" means or what it does**
  - If it is a Large Language Model (Generative AI), AI Act may apply
  - **Flagged as risk in applicability-note.md; requires separate assessment**

**Recommendation:** Clarify LLM workload with NVIDIA. If Generative AI, conduct AI Act compliance audit separately.

---

### CRA (Cyber Resilience Act) — Not Applicable

- **Legal Instrument:** EU Regulation 2024/2930 (EN: Cybersecurity Certification)
- **Scope:** Manufacturers of ICT products
- **Why Not Applicable:** NVIDIA hub is a buyer/user of network equipment, not a manufacturer
- **Reference:** https://eur-lex.europa.eu/eli/reg/2024/2930/oj (for information)

---

### DORA (Digital Operational Resilience Act) — Not Applicable

- **Legal Instrument:** EU Regulation 2023/2775
- **Scope:** Financial services entities
- **Why Not Applicable:** NVIDIA hub is not a bank, insurer, or investment firm
- **Reference:** https://eur-lex.europa.eu/eli/reg/2023/2775/oj (for information)

---

## Technical Standards & References

### Cisco Network Device Documentation

- **Catalyst 3650 Switch:** https://www.cisco.com/c/en/us/products/switches/catalyst-3650-series/
  - CLI Reference: Search "Catalyst 3650 Command Reference"
  - SVI ACL configuration: https://www.cisco.com/c/en/us/support/switches/catalyst-3650-series/

- **Cisco ASA Firewall:** https://www.cisco.com/c/en/us/products/security/asa-firepower-services/
  - CLI Reference: https://www.cisco.com/c/en/us/support/security/asa-5500-x-series-next-generation-firewalls/

- **Cisco ISR4331 Router:** https://www.cisco.com/c/en/us/products/routers/isr-4331/
  - CLI Reference: Search "ISR4331 Command Reference"

### OSPF & ECMP

- **OSPF (Open Shortest Path First):**
  - RFC 2328: https://tools.ietf.org/html/rfc2328
  - Cisco OSPF overview: https://www.cisco.com/c/en/us/support/docs/ip/open-shortest-path-first-ospf/

- **ECMP (Equal-Cost Multi-Path Routing):**
  - NIST definition: https://csrc.nist.gov/glossary/term/ecmp
  - Cisco load balancing: https://www.cisco.com/c/en/us/support/docs/ip/border-gateway-protocol-bgp/

### SSH & Cryptography

- **SSH (Secure Shell) Protocol:**
  - RFC 4251–4254: https://tools.ietf.org/html/rfc4251
  - NIST SP 800-53 Rev. 5, AC-2 (Account Management): https://csrc.nist.gov/publications/detail/sp/800-53/rev-5

- **RSA Key Sizing:**
  - Recommendation: 2048-bit minimum (deprecated: 1024-bit)
  - NIST guidance: https://csrc.nist.gov/publications/detail/sp/800-175b/final

### VLAN & Network Segmentation

- **IEEE 802.1Q (VLAN Tagging):**
  - Official standard: https://www.ieee.org/
  - Cisco VLAN overview: https://www.cisco.com/c/en/us/support/docs/lan-switching/vlan/

- **802.1X (Port-Based Network Access Control):**
  - IEEE 802.1X: https://www.ieee.org/
  - Cisco 802.1X: https://www.cisco.com/c/en/us/support/docs/lan-switching/802-1x/

### Logging & Syslog

- **Syslog Protocol:**
  - RFC 5424 (latest): https://tools.ietf.org/html/rfc5424
  - RFC 3164 (legacy): https://tools.ietf.org/html/rfc3164

- **Cisco Syslog:**
  - Overview: https://www.cisco.com/c/en/us/support/docs/security/logging/

### RADIUS Authentication

- **RADIUS Protocol:**
  - RFC 2865: https://tools.ietf.org/html/rfc2865
  - Cisco RADIUS: https://www.cisco.com/c/en/us/support/docs/security/remote-access-dialin-user-service-radius/

---

## Packet Tracer Simulator

- **Download:** https://www.netacad.com/
  - Free educational version
  - Requires Cisco Learning Network account
  
- **System Requirements:**
  - Windows 10+, macOS, Linux
  - ~1 GB RAM minimum
  
- **Known Limitations:** See `Packet-tracer-limitations.docx` for 10 documented gaps

- **Forum/Support:**
  - Cisco Learning Network: https://learningnetwork.cisco.com/

---

## NIST Cybersecurity Framework (Reference)

- **NIST CSF 2.0 (February 2024):** https://csrc.nist.gov/publications/detail/cswp/02-23-01/final
  - Functions: Govern, Protect, Detect, Respond, Recover (5 domains + Govern)
  - CyFun 2025 is based on NIST CSF 2.0

- **NIST SP 800-53 (Rev. 5):** https://csrc.nist.gov/publications/detail/sp/800-53/rev-5
  - Detailed security controls (285 total)
  - Basis for many ISO 27001 controls

---

## Belgium-Specific Resources

### Cybersecurity Authority

- **Centre de Cybersécurité Belgique (CCB):**
  - Website: https://www.cert.be/
  - Email: cert@bosa.fgov.be
  - Incident Reporting: https://www.cert.be/nl/form/Rapport-un-incident

### Legal Framework

- **Belgian Federal Government Law Portal (Justel):**
  - NIS2 Belgium Act (26 April 2024): https://www.justel.be/
  - GDPR Belgium compliance: https://www.autoriteprotectiondonnees.be/ (ADPD/APD)

### Data Protection Authority

- **Autorité de Protection des Données (APD) / Gegevensbeschermingsautoriteit (GBA):**
  - Website: https://www.autoriteprotectiondonnees.be/
  - Multilingual (FR/NL)
  - GDPR guidance: https://www.autoriteprotectiondonnees.be/ressources/documents

---

## Industry Best Practices

### Network Security

- **SANS Institute:**
  - NMS Checklist: https://www.sans.org/
  - Top 25 Software Errors: https://www.cwe.mitre.org/top25/

- **MITRE ATT&CK Framework:**
  - Enterprise Matrix: https://attack.mitre.org/
  - Network-related tactics & techniques

### Incident Response

- **NIST SP 800-61 (Rev. 2) — Incident Handling Guide:**
  - https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final

### Secure Configuration

- **CIS Benchmarks:**
  - Cisco IOS: https://www.cisecurity.org/cis-benchmarks/
  - Network device hardening baselines

---

## Insurance & Risk Management

### Cyber Insurance

- **AXA Belgium:** https://www.axa.be/
  - Hardware & cyber risk policies

- **Hiscox Europe:** https://www.hiscox.com/
  - Professional liability & cyber risk

### Risk Assessment Methodologies

- **ISO 31000 (Risk Management):**
  - https://www.iso.org/standard/65694
  - Framework for defining Likelihood & Impact scales

---

## Audit Guidance

### ISO 19011 — Auditing Standards

- **Official Standard:** https://www.iso.org/standard/62806
- **Scope:** Guidance for auditing management systems (including ISO 27001)
- **Relevance:** This audit follows ISO 19011 principles (objectivity, evidence-based, independence)

### ISACA Standards

- **CISA (Certified Information Systems Auditor):** https://www.isaca.org/
- **COBIT (Control Objectives for Information & Related Technology):** https://www.isaca.org/resources/cobit

---

## Document Standards

### Markdown

- **Markdown Syntax:** https://daringfireball.net/projects/markdown/
- **CommonMark (Standard):** https://commonmark.org/
- **GitHub Markdown:** https://guides.github.com/features/mastering-markdown/

### Git & Version Control

- **Git Official:** https://git-scm.com/
- **GitHub:** https://github.com/
- **GitLab:** https://gitlab.com/

---

## Notes on Dated References

Some standards referenced in this audit may be updated after publication:

- **ISO 27001:** Check https://www.iso.org/standard/27001 for latest version
- **GDPR:** As of December 2023; monitor https://eur-lex.europa.eu/ for amendments
- **NIS2:** Effective October 2024; Belgium Act (April 2024) details national implementation
- **NIST CSF:** Latest version is 2.0 (February 2024); check https://csrc.nist.gov/

**Always verify current versions before citing in official audit reports.**

---

**Framework Versions:** ISO 27001:2022, GDPR (current), NIS2 (2022), CyFun 2025, NIST CSF 2.0
# GRC Audit: NVIDIA Regional R&D Hub Network Infrastructure

**Project ID:** NVIDIA-REG-RD-2026-1106  
**Audit Date:**  September 2026  
**Status:** FINAL  
**Client:** NVIDIA Corporation (Brussels)  
**Frameworks:** ISO 27001:2022, GDPR Article 32, NIS2 Directive, CyberFundamentals (CyFun)

---

## 👥 Audit Team: Control Freaks

| Role | Auditor | Responsibility |
|------|---------|---|
| **Technical Network Auditor** | Madumathi Singaraju | Packet Tracer review, VLANs, ACLs, firewall, DMZ, technical evidence collection |
| **Regulatory & Compliance Auditor** | Hanah Marroun | Applicability note, NIS2, GDPR, ISO 27001, CyFun framework validation |
| **Audit Coordinator, Risk & Reporting** | Sajjad Shahpoor | Logging, AAA, availability review; findings coordination, risk register, final reporting |

---

---

## Executive Summary

This repository contains a **corrected Governance, Risk, and Compliance (GRC) audit** of NVIDIA's Regional R&D Hub network infrastructure. The audit identified **11 findings across 15 security controls**, with **1 CRITICAL and 4 HIGH-risk gaps** that require remediation before production deployment.

### Key Findings Summary

| Priority | Count | Status | Timeline |
|----------|-------|--------|----------|
| **CRITICAL** | 1 | Centralized logging incomplete | Must fix before go-live |
| **HIGH** | 4 | ACLs/DMZ/SSH/RADIUS weak | Fix within 30 days |
| **MEDIUM** | 5 | Retention/backup/IR/vendor | Fix within 90 days |
| **LOW** | 1 | Dossier inconsistency | Accept/defer |

### Top 3 Priorities

1. **Enable Syslog on firewall + 7 network devices** (3–5 days, €0)
2. **Fix DMZ architecture** — add 3rd firewall interface (10–14 days, €5–10k)
3. **Deploy real RADIUS server + strengthen credentials** (30 days, labor only)

---

## 📋 Repository Structure

```
GRC-Audit/
├── README.md (this file)
├── .gitignore
│
├── deliverables/
│   ├── findings/
│   │   ├── F-01-Critical-Findings.md
│   │   │   └── 5 critical & high-risk findings with recommendations
│   │   │
│   │   ├── checklist.md
│   │   │   └── 15 controls assessment (Pass/Partial/Fail)
│   │   │
│   │   └── risk-register.md
│   │       └── All 11 findings with scores, owners, timeline
│
├── evidence/
│   ├── design-document/
│   │   ├── evidence-guide.md
│   │   │   └── How to cite & verify findings correctly
│   │   │
│   │   └── NOTES.md
│   │       └── External references & standards
│   │
│   └── supporting-docs/
│       ├── applicability-note.md
│       │   └── Regulatory mapping (ISO, GDPR, NIS2, CyFun)
│       │
│       ├── asset-inventory.md
│       │   └── Network devices, servers, workstations
│       │
│       └── scope-statement.md
│           └── In/out of scope, methodology, limitations
```

---

## 🔍 How to Use This Audit

### For Project Managers
1. Read this README (5 min)
2. Review: `deliverables/findings/risk-register.md` — All findings, scores, timeline
3. Plan: Resource the 5 remediation tracks; assign owners

### For Technical Teams
1. Review: `deliverables/findings/F-01-Critical-Findings.md` — Each finding has:
   - What failed (Observation)
   - Why it matters (Compliance impact)
   - How to fix (Recommendation + timeline)
2. Cross-check: `evidence/supporting-docs/` — Understand scope & evidence
3. Remediate: Follow owner + timeline in risk register
4. Validate: Re-test on production hardware

### For Security/Compliance Teams
1. Understand scope: `evidence/supporting-docs/applicability-note.md`
2. Know the controls: `deliverables/findings/checklist.md`
3. Track progress: Update risk register as remediation completes
4. Verify: Re-validate findings before go-live

### For Auditors/Reviewers
1. Methodology: `evidence/supporting-docs/scope-statement.md`
2. Evidence chain: Follow citations to source documents
3. Evidence verification: Use `evidence/design-document/evidence-guide.md`
4. Risk ratings: Check consistency in risk register

---

## ✅ Audit Statistics

### Coverage
- **Frameworks:** 4 (ISO 27001:2022, GDPR, NIS2, CyFun)
- **Controls:** 15 (across 5 areas)
- **Findings:** 11 (1 CRITICAL, 4 HIGH, 5 MEDIUM, 1 LOW)
- **Network Devices:** 12 (Spines, Leaves, routers, firewall)
- **Total Assets:** 82 (devices, servers, workstations)

### Control Assessment
| Result | Count | Controls |
|--------|-------|---------|
| ✅ Pass | 3 | VLAN design, guest isolation, dual spine |
| 🟡 Partial | 2 | ACLs configured but not enforced; DMZ weak |
| ❌ Fail | 10 | Logging, RADIUS, SSH, retention, backup, IR, redundancy, vendor SLA |

### Timeline
| Phase | Timeline | Priority |
|-------|----------|----------|
| **Before Go-Live** | 3–30 days | CRITICAL + HIGH (5 findings) |
| **Post-Launch** | 30–90 days | MEDIUM (5 findings) |
| **Deferred** | 6+ months | LOW (1 finding) |

---

## 🎯 Key Audit Decisions

### Control Framework (15 Controls, 5 Areas)

| Area | Controls | Pass | Partial | Fail | Highest Risk |
|------|----------|------|---------|------|---|
| **Network Segmentation** | 3 | 1 | 1 | 1 | ACLs not enforced (HIGH) |
| **Access Control** | 3 | 1 | 1 | 1 | RADIUS broken, SSH incomplete (HIGH) |
| **Logging & Monitoring** | 3 | 0 | 0 | 3 | No firewall/switch logs (CRITICAL) |
| **Data Protection** | 3 | 0 | 0 | 3 | No backup/IR/retention (MEDIUM) |
| **High Availability & Supply Chain** | 3 | 1 | 0 | 2 | Single edge router, no vendor SLA (MEDIUM) |
| **TOTAL** | **15** | **3** | **2** | **10** | **1 CRITICAL + 4 HIGH** |

### Risk Scale (Likelihood × Impact)

- **Likelihood 1–3:** Low (control exists) → Medium (partial) → High (no barrier)
- **Impact 1–3:** Low (limited) → Medium (one sector) → High (perimeter/core)
- **Score:** 1–2 = LOW | 3–4 = MEDIUM | 6 = HIGH | 9 = CRITICAL

---

## 📄 Document Descriptions

### `deliverables/findings/F-01-Critical-Findings.md`
**11 Findings (5 detailed below)**

1. **F-GAP-01: Logging Incomplete (CRITICAL)**
   - Firewall and switches not forwarding logs to Syslog server
   - Impact: Cannot detect incidents; violates NIS2 24h notification SLA
   - Fix: Enable Syslog on 8 devices (3–5 days, €0)

2. **F-GAP-02: ACLs Not Enforced (HIGH)**
   - Department ACLs configured but not actively enforcing traffic rules
   - Impact: Inter-VLAN isolation not verified
   - Fix: Verify ACL enforcement on production hardware (14 days)

3. **F-GAP-03: DMZ Topology Weak (HIGH)**
   - DMZ and internal networks on same switch; firewall doesn't inspect DMZ↔Internal traffic
   - Impact: Compromise of public-facing server can reach internal systems
   - Fix: Add 3rd firewall interface or re-architect (10–14 days, €5–10k)

4. **F-GAP-04: SSH Incomplete (HIGH)**
   - SSH configured on 3 of 8 Layer 3 devices; 5 lack SSH, exposing Telnet
   - Impact: Management traffic unencrypted; credentials at risk (GDPR Art. 32)
   - Fix: Enable SSH on remaining devices (7 days, €0)

5. **F-GAP-05: RADIUS Nonfunctional (HIGH)**
   - RADIUS server configuration fails; falls back to weak local credentials
   - Impact: Authentication weak; credentials reused (Cisco123, 8 chars, RSA 1024)
   - Fix: Deploy real RADIUS; upgrade credentials (30 days, labor only)

**+ 6 Medium/Low Findings** documented in full checklist and risk register.

### `deliverables/findings/checklist.md`
**15 Security Controls Assessment**

- Each control defined with pass condition
- Evidence cited from network design documents
- Result: Pass ✅ / Partial 🟡 / Fail ❌
- Traces to specific finding (F-GAP-01, etc.)

### `deliverables/findings/risk-register.md`
**All 11 Findings with**

- Observation (what is wrong)
- Evidence citation (where in dossier)
- Risk assessment (Likelihood × Impact score)
- Recommendation (owner, timeline, cost, acceptance criteria)
- Go-live criteria (what must be fixed before production)

### `evidence/supporting-docs/applicability-note.md`
**Regulatory Mapping**

- **ISO 27001:2022** — Information security controls (31% coverage: 15 of 48 controls)
- **GDPR Article 32** — Technical measures for personal data protection
- **NIS2 Directive** — Network & information security; NVIDIA = "Important Entity"
- **CyFun 2025** — Belgium's de facto NIS2 compliance framework

### `evidence/supporting-docs/asset-inventory.md`
**Complete Asset Listing**

- **Network Devices:** 2 Spines + 3 Leaves + 6 access switches + 1 firewall + 2 routers
- **Servers:** 9 (DNS, DHCP, RADIUS, compute, FTP, Syslog, iSCSI)
- **Workstations:** 48 standard + 10 GPU-accelerated
- **Data Flows:** Mapped by VLAN and sensitivity
- **Valuation:** €264k new / €160k refurbished

### `evidence/supporting-docs/scope-statement.md`
**Audit Scope & Methodology**

- **In-Scope:** Network architecture, segmentation, access control, logging, perimeter security, HA
- **Out-of-Scope:** Endpoints, applications, IAM platforms, incident response procedures, training
- **Limitations:** Packet Tracer simulator has 10 known gaps; production validation required
- **Assumptions:** Devices functional as configured, PT simulation accurate (except documented gaps)

### `evidence/design-document/evidence-guide.md`
**How to Cite & Verify Findings**

- Correct citation format (document name + section + quote)
- Red flags (evidence not found, contradictions, PT limitations)
- Verification checklist before submitting
- Document location map (all evidence sources)

### `evidence/design-document/NOTES.md`
**External References & Standards**

- ISO 27001:2022 (links, key clauses)
- GDPR (Articles 5, 32, 33, 34)
- NIS2 Directive (Article 21, Belgium Act 26 April 2024)
- CyFun 2025 (6 functions, Belgium compliance)
- Cisco network device documentation
- NIST CSF, RADIUS/SSH/Syslog RFCs
- Belgium-specific contacts (CCB, ADPD, CISA)

---

## 🏆 Key Takeaways

1. **Audit Findings Are About Gaps, Not Validations**
   - Finding: What control failed
   - Evidence: Where you found the gap
   - Risk: Why it matters
   - Recommendation: How to fix it

2. **Read the Dossier's Admissions**
   - The contractor documented its own limitations (Packet Tracer constraints, RADIUS issues, Syslog gaps, VLAN 1 not hardened)
   - Those admissions **become your findings**, not excuses

3. **Simulator ≠ Production**
   - Packet Tracer has 10 known limitations
   - Before go-live, **validate on real hardware**
   - SVI ACL enforcement, RADIUS, Syslog, 802.1X must be tested on production devices

4. **Consistency Is Critical**
   - Every finding links to a checklist item
   - Every rating follows the same scale
   - README, risk register, and findings all tell the same story
   - If they disagree, fix it before submitting


---
**Framework Versions:** ISO 27001:2022, GDPR (current), NIS2 (2022), CyFun 2025

For detailed guidance, see individual documents in `deliverables/` and `evidence/`.

---

## 🏆 About Control Freaks

This audit was conducted by **Control Freaks**, a specialized GRC audit team comprised of:

**Madumathi Singaraju** — Technical Network Auditor
- Deep expertise in network design and Cisco device configuration
- Led review of Packet Tracer simulation, VLAN segmentation, ACLs, firewall rules, and DMZ architecture
- Ensured all technical findings are grounded in actual configuration evidence

**Hanah Marroun** — Regulatory & Compliance Auditor
- Specialized knowledge in GDPR, ISO 27001:2022, NIS2 Directive, and CyberFundamentals framework
- Prepared the applicability note establishing legal foundation for audit scope
- Validated all framework references and compliance requirements

**Sajjad Shahpoor** — Audit Coordinator, Risk & Reporting
- Coordinated end-to-end audit execution and stakeholder communication
- Reviewed logging (Syslog), AAA (RADIUS), and availability (redundancy) controls
- Built the risk register with consistent ratings, established remediation timeline, and delivered final presentation and reporting materials

Together, Control Freaks delivered a comprehensive, evidence-based audit bridging technical reality and regulatory compliance.

---

*"An audit is only as good as the team executing it. Control Freaks brought technical rigor, compliance expertise, and coordinated execution to deliver this assessment."*
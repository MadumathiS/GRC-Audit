# GRC Audit: NVIDIA Regional R&D Hub Network Infrastructure

**Project ID:** NVIDIA-REG-RD-2026-1106  
**Audit Date:** September 2026  
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

## Executive Summary

This repository contains a **comprehensive Governance, Risk, and Compliance (GRC) audit** of NVIDIA's Regional R&D Hub network infrastructure. The audit identified **12 findings** — **1 CRITICAL and 5 HIGH-risk gaps** that require remediation before production deployment, plus **6 MEDIUM-risk and 0 LOW-risk items** requiring post-launch attention.

### Key Findings Summary

| Priority | Count | Status | Timeline |
|----------|-------|--------|----------|
| **CRITICAL** | 1 | Centralized logging incomplete | Must fix before go-live |
| **HIGH** | 5 | ACLs/DMZ/SSH/RADIUS/FTP weak | Fix within 30 days |
| **MEDIUM** | 6 | Retention/backup/IR/vendor/edge failover/dossier | Fix within 90 days |
| **LOW** | 0 | — | — |

### Top 3 Priorities (Before Go-Live)

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
│   ├── Final-Audit-Report.md
│   │   └── Consolidated final report: scope, applicability, all 12 findings, go-live gate
│   │
│   ├── NIS2-GDPR-Incident-Notification.md
│   │   └── Completed incident exercise: notification timelines, breach assessment
│   │
│   └── findings/
│       ├── Critical-Findings.md
│       │   └── 1 critical & 5 high-risk findings (F-GAP-01 to F-GAP-05, F-GAP-12) 6 medium-risk findings (F-GAP-06 to F-GAP-11)
│       ├── checklist.md
│       │   └── 15 controls assessment (Pass/Partial/Fail)
│       │
│       └── risk-register.md
│           └── All 12 findings with scores, owners, timelines
│
├── evidence/
│   ├── design-document/
│   │   ├── evidence-guide.md
│   │   └── NOTES.md
│   │
│   └── supporting-docs/
│       ├── applicability-note.md
│       ├── asset-inventory.md
│       └── scope-statement.md
```

---

## 🔍 How to Use This Audit

### For Project Managers
1. Read this README (5 min)
2. Read: `deliverables/Final-Audit-Report.md` — Consolidated report with priorities & go-live gate
3. Review: `deliverables/findings/risk-register.md` — All findings with timelines
4. Plan: Resource remediation tracks and assign owners

### For Technical Teams
1. Review: `deliverables/findings/F-01-Critical-Findings.md` — 5 high-priority findings
2. Review: `deliverables/findings/F-GAP-06-to-11-Additional-Findings.md` — 6 medium findings
3. Check: `deliverables/findings/F-GAP-12-FTP-Cleartext-Default-Credentials.md` — FTP issue
4. Follow: Owner + timeline in risk register for remediation

### For Security/Compliance Teams
1. Understand: `evidence/supporting-docs/applicability-note.md`
2. Know controls: `deliverables/findings/checklist.md`
3. Review incident readiness: `deliverables/NIS2-GDPR-Incident-Notification.md`
4. Track progress: Update risk register as remediation completes

### For Auditors/Reviewers
1. Methodology: `evidence/supporting-docs/scope-statement.md`
2. Evidence verification: `evidence/design-document/evidence-guide.md`
3. Risk consistency: `deliverables/findings/risk-register.md`

---

## ✅ Audit Statistics

### Coverage
- **Frameworks:** 4 (ISO 27001:2022, GDPR, NIS2, CyFun)
- **Controls:** 15 (across 5 areas: Network Segmentation, Access Control, Logging, Data Protection, HA/Supply Chain)
- **Findings:** 12 (1 CRITICAL, 5 HIGH, 6 MEDIUM, 0 LOW)
- **Network Devices:** 12 (2 Spines, 3 Leaves, 6 access switches, 1 firewall, 2 edge routers)
- **Total Assets:** 71 (14 devices, 9 servers, 48 workstations)

### Control Assessment
| Result | Count |
|--------|-------|
| ✅ Pass | 3 |
| 🟡 Partial | 2 |
| ❌ Fail | 10 |

### Timeline
| Phase | Timeline | Count |
|-------|----------|-------|
| Before Go-Live | 3–30 days | 6 findings (1 CRITICAL + 5 HIGH) |
| Post-Launch | 30–90 days | 6 findings (MEDIUM) |

---

## 🎯 Key Audit Decisions

### Control Framework (15 Controls, 5 Areas)

| Area | Controls | Pass | Partial | Fail | Highest Risk |
|------|----------|------|---------|------|---|
| **Network Segmentation** | 3 | 1 | 1 | 1 | ACLs not enforced (HIGH) |
| **Access Control** | 3 | 1 | 1 | 1 | RADIUS broken, SSH incomplete (HIGH) |
| **Logging & Monitoring** | 3 | 0 | 0 | 3 | No firewall/switch logs (CRITICAL) |
| **Data Protection** | 3 | 0 | 0 | 3 | No backup/IR/retention/data inventory (MEDIUM) |
| **HA & Supply Chain** | 3 | 1 | 0 | 2 | Single edge router/firewall, no vendor SLA (MEDIUM) |
| **TOTAL** | **15** | **3** | **2** | **10** | **1 CRITICAL + 5 HIGH** |

### Risk Scale
- **Score:** 1–2 = LOW | 3–4 = MEDIUM | 6 = HIGH | 9 = CRITICAL
- **Formula:** Likelihood (1–3) × Impact (1–3)

---

## 📄 Quick Reference

### Files by Audience

**Decision Makers:** README → Final-Audit-Report.md → risk-register.md  
**Technical Teams:** README → F-01-Critical-Findings.md → F-GAP-06-to-11-Additional-Findings.md → F-GAP-12  
**Compliance:** applicability-note.md → checklist.md → NIS2-GDPR-Incident-Notification.md  
**Auditors:** scope-statement.md → evidence-guide.md → risk-register.md

---

## 🏆 Key Takeaways

1. **Audit Findings Are About Gaps, Not Validations**
   - Finding: What control failed
   - Evidence: Where you found the gap
   - Risk: Why it matters
   - Recommendation: How to fix it

2. **Read the Dossier's Admissions**
   - The contractor documented its own limitations (Packet Tracer, RADIUS, Syslog)
   - Those admissions **become findings**, not excuses

3. **Simulator ≠ Production**
   - Packet Tracer has 10 known gaps
   - Before go-live, **validate on real hardware**
   - SVI ACL, RADIUS, Syslog, 802.1X must be tested on production devices

4. **Consistency Is Critical**
   - Every finding links to a checklist item
   - Every rating follows the same scale
   - README, risk register, and findings all tell the same story

---

## 🏆 About Control Freaks

**Madumathi Singaraju** — Technical Network Auditor  
Deep expertise in network design and Cisco device configuration. Led review of Packet Tracer simulation, VLAN segmentation, ACLs, firewall rules, and DMZ architecture. Ensured all technical findings are grounded in actual configuration evidence.

**Hanah Marroun** — Regulatory & Compliance Auditor  
Specialized knowledge in GDPR, ISO 27001:2022, NIS2 Directive, and CyberFundamentals framework. Prepared the applicability note establishing legal foundation for audit scope. Validated all framework references and compliance requirements.

**Sajjad Shahpoor** — Audit Coordinator, Risk & Reporting  
Coordinated end-to-end audit execution and stakeholder communication. Reviewed logging (Syslog), AAA (RADIUS), and availability (redundancy) controls. Built the risk register with consistent ratings and established remediation timeline.

---

## 📌 License
Educational / Academic Cisco Packet Tracer Simulation Project — For instructional and portfolio demonstration purposes only.

---

*"An audit is only as good as the team executing it. Control Freaks brought technical rigor, compliance expertise, and coordinated execution to deliver this assessment."*
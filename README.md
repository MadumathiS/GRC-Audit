# NVIDIA Regional R&D Hub — Compliance Audit

**Project Reference:** NVIDIA-REG-RD-2026-1106  
**Audit Type:** Design-Based Compliance Assessment  
**Audit Date:** September 2026  

---

## 📋 Project Overview

This repository contains a comprehensive **compliance audit** of the NVIDIA Regional R&D and Production Hub network design. The audit compares the design deliverables against regulatory requirements and security frameworks to identify gaps before the facility goes live.

### What We're Auditing

- **Client:** NVIDIA Corporation
- **Facility:** Regional R&D and Production Hub
- **Location:** Belgium
- **Scope:** Complete network design covering 48 workstations across 5 operational sectors
- **CapEx Constraint:** €300,000 ceiling
- **Assets:** Network infrastructure, servers, services, and employee data

### Audit Deliverables

1. ✅ **Audit Checklist** — 12-15 specific controls mapped to frameworks
2. ✅ **Findings** — Identified gaps with evidence citations
3. ✅ **Risk Register** — Prioritized findings with severity ratings
4. ✅ **NIS2 Incident Notification** — Regulatory exercise
5. ✅ **Team Presentation** — Executive summary and recommendations

### Applicable Regulations

| Regulation | Applies? | Reason |
|-----------|----------|--------|
| **GDPR** | ✅ YES | Employee personal data (names, IDs, salaries) |
| **NIS2** | ✅ YES | R&D critical sector, 48+ employees |
| **ISO/IEC 27001:2022** | ✅ YES | Reference framework for audit |
| **CyFun (CCB)** | ✅ YES | Belgian security framework |
| **Cyber Resilience Act** | ❌ NO | Not a product manufacturer |
| **DORA** | ❌ NO | Not a financial entity |
| **AI Act** | ❓ UNCERTAIN | Needs clarification on system use |

See `01-scope-and-applicability/applicability-note.md` for full analysis.

---

## 👥 Team

| Name | Role | Responsibilities | GitHub Handle |
|------|------|------------------|---|
| **[Team Lead Name]** | Lead Auditor | Overall coordination, regulatory mapping, consistency | @[handle] |
| **[Technical Name]** | Technical Auditor | Access control, network segmentation, logging | @[handle] |
| **[Process Name]** | Process Auditor | Governance, data protection, supply chain | @[handle] |
| **[Documentation Name]** | Documentation Lead | Findings write-up, evidence tracking, register | @[handle] |


---

## 🗂️ Repository Structure

```
Grc-Audit/
│
├── README.md                          # You are here
├── .gitignore                         # Git rules (ignore large files, secrets)
│
├── 01-scope-and-applicability/        # STEP 1: What are we auditing?
│   ├── asset-inventory.md             # All assets: VLANs, servers, data
│   ├── applicability-note.md          # Which regulations apply & why
│   └── scope-statement.md             # Audit boundaries & limitations
│
├── 02-audit-checklist/                # STEP 2: What will we check?
│   ├── checklist-final.md             # 12-15 specific checks
│   └── control-mapping.md             # How checks map to regulations
│
├── 03-audit-execution/                # STEP 3: What did we find?
│   ├── checklist-results.md           # All results (pass/fail/partial)
│   ├── findings/                      # Individual findings
│   │   ├── F-01-[title].md
│   │   ├── F-02-[title].md
│   │   └── [more findings...]
│   └── evidence-log.md                # Where evidence came from
│
├── 04-risk-and-notification/          # STEP 4: How serious are they?
│   ├── rating-scale.md                # Likelihood × Impact scale
│   ├── risk-register.md               # Findings with severity ratings
│   ├── nis2-incident-notification.md  # Regulatory exercise
│   └── risk-analysis.md               # Why we rated things certain ways
│
│
├── evidence/                          # Source materials (linked, not committed)
│   ├── EVIDENCE_GUIDE.md              # How to cite evidence
│   ├── RFQ/                           # Client requirements
│   ├── design-document/               # Design deliverables
│   └── NOTES.md                       # Links to actual evidence files
│
│
└── references/                        # Learning materials
    ├── security-concepts.md           # Quick reference
    ├── regulations-summary.md         # Key points
    └── audit-method-notes.md          # Methodology recap
```

---

## 🚀 How to Use This Repository

### First Time Setup

```bash
# Clone the repository
git clone https://github.com/MadumathiS/Grc-Audit.git
cd GRC-Audit

# Create your local branch
git checkout -b feature/[your-task]

# Verify you're on the right branch
git branch -a
```


---

## 📖 Important Concepts

### Evidence Traceability

**Every finding must point to specific evidence.** Examples:

- **Design Document:**  "Section: Security Measures, Page: 9"
- **Packet Tracer:** "Device: Core-SW-01, Interface: Gi0/1, Config: ACL-not-applied"
- **RFQ:** "§2.3 Technical Scope" or "Section: Requirements"

**Format:** `Document name, specific location, specific detail`

### Rating Scale

Findings are rated using **Likelihood × Impact**:

```
LIKELIHOOD: How probable is exploitation?
├─ 3 (High):   No controls prevent it; common technique; easy to exploit
├─ 2 (Medium): Some controls exist, but partial or can be bypassed
└─ 1 (Low):    Requires special conditions; insider access; rare scenario

IMPACT: How bad if it happens?
├─ 3 (High):   Affects sensitive data, operations, or business continuity
├─ 2 (Medium): Affects one sector or one service; moderate recovery time
└─ 1 (Low):    Limited effect; easily recovered; non-sensitive data

MULTIPLY TO GET SCORE:
9 = CRITICAL (fix immediately)
6 = HIGH (fix within 30 days)
3-4 = MEDIUM (fix within 90 days)
1-2 = LOW (nice to have; fix if resources available)
```

**Important:** This scale must be applied **consistently** throughout the audit. See `04-risk-and-notification/rating-scale.md` for full details and examples.

### Control Types

Not all controls do the same thing:

| Type | Purpose | Example |
|------|---------|---------|
| **Preventive** | Stop attacks before they happen | Firewalls, MFA, encryption |
| **Detective** | Identify attacks in progress or after | Logging, monitoring, SIEM |
| **Corrective** | Respond and recover from attacks | Incident response plans, backups |
| **Governance** | Document, own, and manage controls | Policies, procedures, training |

---

## 🔍 Key Regulations Explained

### GDPR — Personal Data Protection
**What it means:** Protect personal data of EU residents; report breaches within 72 hours  
**Applies here:** Employee records (names, national IDs, salaries)  
**Key deadline:** 72-hour breach notification to APD/GBA

### NIS2 — Critical Infrastructure Security
**What it means:** Critical organizations must implement security measures; report incidents quickly  
**Applies here:** R&D is a critical sector; hub has 48+ employees  
**Key deadlines:** 24h early warning → 72h notification → 1 month final report

### ISO/IEC 27001:2022 — Security Management
**What it means:** Framework with 93 controls for information security  
**Applies here:** Used as reference framework for all audit checks  
**Key:** 4 themes: Organizational, People, Physical, Technological

### CyFun (CCB) — Belgian Security Framework
**What it means:** Belgium's practical security checklist (3 levels: Basic, Important, Essential)  
**Applies here:** Belgium-specific guidance; maps to ISO 27001  
**Key:** Used for access control, segmentation, logging checks

For full details, see `references/regulations-summary.md` or read the main module documentation.

---

## ⚠️ Important Limitations & Disclaimers

### What This Audit Does

✅ Compares design document against regulatory requirements  
✅ Identifies gaps between design and configuration  
✅ Cites specific controls from recognized frameworks  
✅ Rates findings consistently using a documented scale  
✅ Provides actionable recommendations  

### What This Audit Does NOT Do

❌ Conduct a penetration test or hacking exercise  
❌ Verify that documented policies are followed in practice  
❌ Test a running production system  
❌ Provide legal advice (we are not lawyers)  
❌ Guarantee absence of vulnerabilities  

### Scope Boundaries

**In Scope:**
- Network design document (PDF)
- Packet Tracer simulation
- NVIDIA RFQ requirements
- Belgian/EU regulatory requirements

**Out of Scope:**
- Physical security of data center
- Actual employee security practices
- Vendor security assessments (only documented requirements)
- Backup/recovery testing (only documented procedures)

See `01-scope-and-applicability/scope-statement.md` for complete scope.

---

## 📚 References & Resources

### Framework Documentation

- **ISO/IEC 27001:2022** — https://www.iso.org/standard/27001
  - 93 controls in 4 themes: Organizational, People, Physical, Technological
  - Official reference for most audit checks

- **CyFun (CCB)** — https://atwork.safeonweb.be/cyberfundamentals-framework
  - Belgium's practical security framework
  - Aligned with NIST CSF 2.0 and ISO 27001

- **NIST CSF 2.0** — https://www.nist.gov/cyberframework
  - Reference architecture used as basis for all frameworks
  - Functions: Govern, Identify, Protect, Detect, Respond, Recover

### Regulatory Authorities

**Belgium:**
- **CCB** (Centre for Cybersecurity Belgium) — https://ccb.belgium.be/en
  - NIS2 oversight and enforcement
  - Incident reporting: https://notif.safeonweb.be

- **APD/GBA** (Data Protection Authority) — https://www.autoriteprotectiondonnees.be
  - GDPR enforcement
  - Breach notification: https://moncompte.autoriteprotectiondonnees.be

**EU:**
- **EDPB** (Data Protection Board) — https://www.edpb.europa.eu
- **ENISA** (Cybersecurity Agency) — https://www.enisa.europa.eu

### Module Documentation

- `01_SecurityConcepts.md` — CIA Triad, assets, threat modeling
- `02_Regulations.md` — Full regulatory requirements
- `03_AuditMethod.md` — Audit methodology and finding structure
- `04_FinalProject.md` — Project brief and requirements

---

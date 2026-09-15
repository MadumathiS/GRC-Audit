# GRC-Audit: NVIDIA Regional R&D Hub

**Project:** Network Security Audit & Compliance Assessment  
**Organization:** NVIDIA Corporation (Regional R&D Hub)  
**Service Provider:** Full Duplex (Network Implementation)  
**Audit Type:** Infrastructure Security Review (Packet Tracer Simulation)  
**Status:** Final Report  
**Date:** September 2026

---

## 📋 Quick Navigation

| Section | Purpose | Location |
|---------|---------|----------|
| **Audit Charter** | Project scope & compliance framework | `supporting-docs/` |
| **Checklist** | Control assessment summary | `deliverables/checklist.md` |
| **Findings** | Detailed control validations (3 findings) | `deliverables/findings/` |
| **Risk Register** | Risk assessment & mitigation status | `deliverables/risk-register.md` |
| **Evidence** | Network design, test reports, RFQ | `evidence/` |

---

## 🎯 Executive Summary

### Audit Scope

This audit validates the **NVIDIA Regional R&D Hub network infrastructure** against:
- **ISO/IEC 27001:2022** (Information Security Management)
- **GDPR Article 32** (Technical Security Measures)
- **NIS2 Directive** (Network & Information Security)
- **CyFun Framework** (Belgian Cyber Fundamentals)

### In-Scope Infrastructure

```
Network Architecture:
├── 2 Spine (Core) Switches (Layer 3, OSPF routing)
├── 3 Leaf Switches (Distribution layer, VLAN termination)
├── 6 Access Switches (Department connectivity)
├── 1 ASA Firewall (Perimeter security, DMZ protection)
├── 1 Edge Router (Internet gateway, Syslog forwarding)
├── 1 Syslog Server (Centralized event logging)
└── 48 Workstations + 10 GPU Systems (Endpoints)

Scope Areas:
- Network segmentation (9 VLANs, DMZ isolation)
- Access control (ACLs, port security, SSH-only management)
- Logging & monitoring (Syslog, RADIUS, event capture)
- Redundancy & high availability (OSPF, spanning tree)
```

### Audit Results

| Finding | Title | Status | Risk |
|---------|-------|--------|------|
| **F-01** | VLAN 1 Hardening | ✅ IMPLEMENTED | LOW |
| **F-02** | Firewall & Syslog | ✅ OPERATIONAL | LOW |
| **F-03** | Access Switch Logging | ✅ EFFECTIVE | LOW |

**Overall Assessment:** ✅ **CONTROLS EFFECTIVE** — No critical remediation required.

---

## 📁 Repository Structure

```
## 📁 Repository Structure

GRC-Audit/
├── README.md (this file)
├── .gitignore
│
├── deliverables/
│   ├── findings/
│   │   ├── F-01-VLAN-Segmentation-Validated.md
│   │   │   └── VLAN segmentation validated; 4 tests PASS
│   │   │
│   │   ├── F-02-Firewall-Authentication-Validated.md
│   │   │   └── ASA firewall & RADIUS validated; 6 tests PASS
│   │   │
│   │   ├── F-03-Logging-Monitoring-Validated.md
│   │   │   └── Syslog logging validated; 4 tests PASS
│   │   │
│   │   ├── checklist.md
│   │   │   └── Control assessment summary (25 controls evaluated)
│   │   │
│   │   └── risk-register.md
│   │       └── Risk assessment & mitigation strategies
│
├── evidence/
│   ├── design-document/
│   │   ├── Nvidia_network.pkt
│   │   │   └── Packet Tracer network simulation & configuration
│   │   │
│   │   ├── Final_NVIDIA_Network_Security_Project_Testing_Report.pdf
│   │   │   └── Comprehensive 51-page testing report (ALL TESTS PASS)
│   │   │       Covers: VLAN, Firewall, RADIUS, NAT, DMZ, Syslog, etc.
│   │   │
│   │   ├── NVIDIA_Project_Report.pdf
│   │   │   └── 60-page project overview & validation summary
│   │   │
│   │   ├── Network-Implementation-Report.pdf
│   │   │   └── Network design, VLAN layout, architecture decisions
│   │   │
│   │   ├── NVIDIA-Security-Configuration.docx
│   │   │   └── Firewall rules, ACLs, Syslog, RADIUS configuration
│   │   │
│   │   ├── NVIDIA-Device-Configuration.docx
│   │   │   └── Device-level settings, SSH, management VLAN
│   │   │
│   │   ├── NVIDIA-VLAN-Subnet-Worksheet.docx
│   │   │   └── IP addressing plan, VLAN allocation, gateways
│   │   │
│   │   ├── A-Well-Routed-Router.pdf
│   │   │   └── Router configuration reference guide
│   │   │
│   │   ├── why_leaf_and_spine.docx
│   │   │   └── Architecture justification, OSPF, ECMP benefits
│   │   │
│   │   ├── Packet-tracer-limitations.docx
│   │   │   └── Simulator constraints, production recommendations
│   │   │
│   │   ├── NVIDIA_RFQ_v2.pdf
│   │   │   └── Requirements baseline for audit scope
│   │   │
│   │   ├── Contract.pdf
│   │   │   └── Full Duplex service contract
│   │   │
│   │   ├── INSURANCE_CONTRACT___PROPOSAL.pdf
│   │   │   └── Hardware & cyber risk insurance coverage
│   │   │
│   │   ├── NVIDIA_Regional_R_D_Hub___Cost_Breakdown.pdf
│   │   │   └── Asset valuation & cost analysis
│   │   │
│   │   ├── evidence-guide.md
│   │   │   └── How to cite evidence in findings
│   │   │
│   │   └── NOTES.md
│   │       └── External references (Cisco, standards, regulations)
│   │
│   └── supporting-docs/
│       ├── applicability-note.md
│       │   └── Audit scope mapping to ISO 27001, GDPR, NIS2, CyFun
│       │
│       ├── asset-inventory.md
│       │   └── Network devices, workstations, GPU systems
│       │
│       └── scope-statement.md
│           └── In-scope areas, audit period, testing approach
```

---

## 🔍 How to Use This Repository

### For Audit Stakeholders

1. **Start Here:**
   - Read `supporting-docs/scope-statement.md` (what was audited)
   - Review `deliverables/checklist.md` (control summary)
   - Review `deliverables/risk-register.md` (risk assessment)

2. **Review Findings:**
   - Open `deliverables/findings/` directory
   - Read each finding (F-01, F-02, F-03)
   - Each finding references specific evidence with document names & section numbers

3. **Verify Evidence:**
   - Open `evidence/evidence-guide.md` to understand citation format
   - Follow citation references to specific files in `evidence/` directory
   - Locate evidence in PKT file, test reports, or RFQ documents

### For Auditors/Validators

1. **Validate Control Implementation:**
   ```
   For each finding:
   1. Read evidence section (what was tested)
   2. Open referenced PKT file or test report
   3. Verify that configuration/test result matches described evidence
   4. Confirm assessment is accurate
   ```

2. **Check Compliance Mapping:**
   - Each finding includes mapping to ISO 27001, GDPR, NIS2, CyFun
   - Cross-reference with `supporting-docs/applicability-note.md`

3. **Review Risk Assessment:**
   - Open `deliverables/risk-register.md`
   - Verify risk ratings match control effectiveness

### For Operations/Implementation Teams

1. **Understand Network Design:**
   - Read `evidence/design-document/Network-Implementation-Report.pdf`
   - Review `evidence/design-document/Nvidia_network.pkt` (topology)
   - Check `evidence/design-document/NVIDIA-VLAN-Subnet-Worksheet.docx` (IP plan)

2. **Implement Production Changes:**
   - Refer to `evidence/design-document/NVIDIA-Security-Configuration.docx`
   - Use Cisco documentation links in `evidence/NOTES.md`
   - Follow "Follow-up Actions" section in each finding

3. **Production Recommendations:**
   - Each finding includes "Auditor Notes" with production requirements
   - Review `evidence/Packet-tracer-limitations.docx` for simulator constraints
   - Implement additional controls noted as "Production Requirement"

---

## 📊 Control Assessment Summary

### Finding Breakdown

**F-01: VLAN 1 Hardening** (Best Practice)
- **Status:** ✅ IMPLEMENTED
- **Risk:** LOW
- **Evidence:** PKT file, network design document, architecture justification
- **Impact:** Prevents 802.1Q double-tagging VLAN hopping attacks
- **Compliance:** ISO 27001 A.13.1.3, NIST PR.AC-5

**F-02: Firewall & Syslog Logging** (Control Implementation)
- **Status:** ✅ OPERATIONAL (All 8 firewall test sections PASSED)
- **Risk:** LOW
- **Evidence:** 15+ test reports, ASA configuration, Syslog event logs
- **Impact:** Perimeter security, centralized event logging for incident detection
- **Compliance:** ISO 27001 A.12.4.1, GDPR Article 32, NIS2 Article 23

**F-03: Access Switch Logging** (Control Implementation)
- **Status:** ✅ EFFECTIVE (All port tests PASSED)
- **Risk:** LOW
- **Evidence:** Switch interface tests, spanning tree validation, event logs
- **Impact:** Department-level segmentation, unused port containment, event auditing
- **Compliance:** ISO 27001 A.13.1.3 + A.9.2.1, GDPR Article 32, NIS2

---


## 📋 Audit Metadata

| Attribute | Value |
|-----------|-------|
| **Audit Period** | Q3 2026 (Packet Tracer simulation) |
| **Testing Method** | Network simulation + design review |
| **Frameworks** | ISO 27001, GDPR, NIS2, CyFun |
| **Scope** | Network infrastructure (Spine-Leaf, DMZ, access layer) |
| **Total Controls Assessed** | 25 |
| **Findings Count** | 3 (all LOW risk) |
| **Test Reports Count** | 18 comprehensive test reports |
| **Evidence Files** | 25+ supporting documents |
| **Status** | ✅ FINAL - No critical remediation required |

---

## 🔐 Confidentiality Notice

This audit report contains sensitive information about NVIDIA's network infrastructure, security configurations, and compliance posture. 

**Handling Requirements:**
- Restricted to authorized audit stakeholders only
- Do not distribute externally without written permission
- Destroy or securely archive after retention period
- Report security issues immediately via SafeOnWeb (Belgium): https://notif.safeonweb.be

---

**For more information, see:**
- `deliverables/checklist.md` — Full control checklist
- `deliverables/findings/` — All findings with evidence
- `evidence/evidence-guide.md` — How to read the evidence
- `supporting-docs/scope-statement.md` — Audit scope details


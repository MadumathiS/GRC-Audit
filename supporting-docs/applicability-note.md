# Applicability Note — NVIDIA Regional R&D Hub

**Project:** NVIDIA Regional R&D and Production Hub  
**Project ID:** NVIDIA-REG-RD-2026-1106  
**Prepared By:**  

---

## Organization Profile

| Field | Value |
|-------|-------|
| **Organization Type** | Technology/Manufacturing (NVIDIA Regional Hub) |
| **Organization Size** | Large Enterprise (48 workstations + infrastructure) |
| **Primary Sector(s)** | AI/ML Research, Production, Development |
| **Location(s)** | Belgium (Brussels region) |
| **Data Types** | Employee data, R&D IP, Production data, Financial records |
| **Critical Functions** | AI research, LLM training, multimedia production, file transfer |
| **Employees** | 48 workstations deployed |

---

## Regulations Assessment

### 1. GDPR (General Data Protection Regulation)

| Criteria | Status | Analysis |
|----------|--------|----------|
| **Applies?** | ✅ **YES** | Belgium-based organization processing personal data |
| **Data Types** | ✅ Employee data (names, addresses, national IDs, salaries) stored on AAA Server (192.168.70.12) |
| **Scope** | ✅ Personal data of 48 employees + contractors in workstations, servers, and network logs |
| **Key Obligations** | Data protection, access control, breach notification (72 hours), data retention limits, DPA requirements |
| **Authority** | APD/GBA (Belgium Data Protection Authority) |
| **Breach Notification Portal** | https://moncompte.autoriteprotectiondonnees.be |
| **Evidence in Design** | Employee database stored on centralized AAA server; Syslog captures user activity |
| **Risk Rating** | **HIGH** — Personal data in centralized location with partial monitoring |

**Finding:** Employee data (national IDs, salaries) centralized on 192.168.70.12 with no documented backup/encryption requirements. Breach would trigger GDPR 72-hour notification to APD/GBA.

---

### 2. NIS2 (Network and Information Security Directive 2)

| Criteria | Status | Analysis |
|----------|--------|----------|
| **Applies?** | ✅ **YES** | Critical R&D infrastructure for NVIDIA |
| **Entity Type** | ✅ **Essential** (Critical sector organization) |
| **Sector** | Research & Development (Critical Information Infrastructure) |
| **Size Threshold** | ✅ Exceeds 250 employees; meets "essential" criteria |
| **Key Obligations** | Risk management, incident response, supply chain security, breach notification (24h/72h/1mo) |
| **Authority** | CCB (Centre for Cybersecurity Belgium) |
| **Incident Reporting Portal** | https://notif.safeonweb.be |
| **Evidence in Design** | Multi-layer security (VLAN, ACLs, firewall), centralized logging, DMZ design |
| **Risk Rating** | **CRITICAL** — R&D infrastructure requires 24-hour incident notification |

**Timelines:**
- **24 hours:** Early warning to CCB
- **72 hours:** Full incident notification to CCB
- **1 month:** Final report to CCB

**Finding:** Syslog monitoring partially implemented (Leaf-1 only); access switches not logging. This creates blind spots for incident detection and reporting.

---

### 3. ISO/IEC 27001:2022 (Information Security Management)

| Criteria | Status | Analysis |
|----------|--------|----------|
| **Applies?** | ✅ **YES** | Reference framework for audit |
| **Type** | ✅ **Reference Framework** (not certification requirement) |
| **Scope** | All network infrastructure, servers, workstations |
| **Controls Assessed** | 14 Annex A controls across 4 themes |
| **Themes** | Organizational, People, Physical, Technological |
| **Audit Standard** | ISO/IEC 27001:2022 |
| **Evidence in Design** | ACLs (A.8.2), SSH access (A.9.4), VLAN segmentation (A.8.1), logging (A.12.4) |
| **Risk Rating** | **HIGH** — Several controls only partially implemented |

**14 Primary Controls Being Assessed:**
1. A.5.1 — Policies for information security
2. A.6.1 — Organization of information security
3. A.8.1 — Access control
4. A.8.2 — User access management
5. A.8.3 — Access control to cryptography
6. A.8.6 — Access control for change management
7. A.9.1 — Network security perimeter (firewall, DMZ)
8. A.9.2 — Network segmentation (VLANs)
9. A.9.4 — Access control for remote work (SSH)
10. A.10.1 — Cryptography policy
11. A.12.4 — Logging and monitoring
12. A.12.6 — Management of technical vulnerabilities
13. A.13.1 — Information transfer policies
14. A.14.2 — System acceptance criteria

**Finding:** Controls A.12.4 (logging) and A.12.6 (vulnerability management) only partially implemented.

---

### 4. CyFun (Belgian Cybersecurity Framework)

| Criteria | Status | Analysis |
|----------|--------|----------|
| **Applies?** | ✅ **YES** | Belgium-specific requirement |
| **Level Required** | ✅ **Important** (R&D hub with data) |
| **Linked To** | NIS2 + ISO 27001 |
| **Framework Domains** | Governance, Data Protection, Access Control, Network Security, Incident Response |
| **Authority** | CCB (Centre for Cybersecurity Belgium) |
| **Evidence in Design** | ACL-based access control, VLAN segmentation, centralized logging, firewall |
| **Risk Rating** | **MEDIUM-HIGH** — Framework requirements mostly met with gaps |

**CyFun Domains Assessed:**
- **Governance:** Audit team, defined roles ✅
- **Data Protection:** Centralized servers, encrypted management (SSH) ✅
- **Access Control:** VLAN isolation, ACLs ✅ (with gaps)
- **Network Security:** Firewall, DMZ, segmentation ✅
- **Incident Response:** Syslog monitoring ⚠️ (partial)
- **Supply Chain:** Not assessed (out of scope)

**Finding:** CyFun "Important" level mostly implemented; monitoring gaps present.

---

### 5. Cyber Resilience Act (CRA)

| Criteria | Status | Analysis |
|----------|--------|----------|
| **Applies?** | ❌ **NO** | |
| **Reason** | NVIDIA designs/uses equipment but does not manufacture products for EU market |
| **Scope** | Does not apply to network infrastructure; applies to manufactured products |

---

### 6. DORA (Digital Operational Resilience Act)

| Criteria | Status | Analysis |
|----------|--------|----------|
| **Applies?** | ❌ **NO** | |
| **Reason** | Not a financial institution; NVIDIA Regional Hub is R&D/manufacturing |
| **Note** | Would apply if this were a bank or financial services entity |

---

### 7. AI Act

| Criteria | Status | Analysis |
|----------|--------|----------|
| **Applies?** | ❓ **UNCERTAIN** | |
| **Uses AI Systems?** | ✅ **YES** — Compute Server (192.168.70.21) for "AI/ML workload simulation" |
| **Risk Classification** | ✅ **High Risk** (AI for research/training) |
| **Requires:** | Transparency, human oversight, risk assessment |
| **Assessment** | Beyond scope of network audit; flagged for separate AI governance review |
| **Recommendation** | Conduct separate AI governance assessment |

**Finding:** Network audit notes AI compute server but AI governance assessment required separately. Recommend reviewing AI Act compliance as Phase 2 work.

---

## Summary Table

| Regulation | Applies | Level/Type | Key Obligations | Authority | Status |
|-----------|---------|-----------|-----------------|-----------|--------|
| **GDPR** | ✅ YES | Critical | 72h breach notification, data protection, DPA | APD/GBA | Must Comply |
| **NIS2** | ✅ YES | Essential | 24h/72h/1mo incident notification, risk mgmt | CCB | Must Comply |
| **ISO 27001** | ✅ YES | Reference | 14 controls assessed | International | Reference |
| **CyFun** | ✅ YES | Important | Domain-based framework | CCB | Must Comply |
| **CRA** | ❌ NO | N/A | Product manufacturing | EU | Not Applicable |
| **DORA** | ❌ NO | N/A | Financial services | EU | Not Applicable |
| **AI Act** | ❓ UNCERTAIN | High Risk | Separate assessment | EU | Flag for Review |

---

## Compliance Obligations Summary

### GDPR Obligations
- ✅ Personal data inventory (employee data identified)
- ⚠️ Data Protection Impact Assessment (required)
- ⚠️ Data Processing Agreement with NVIDIA (required)
- ⚠️ 72-hour breach notification to APD/GBA (capability needed)
- ⚠️ Data subject rights procedures (required)

**GDPR Contacts:**
- APD/GBA Portal: https://moncompte.autoriteprotectiondonnees.be
- Email: contact@autoriteprotectiondonnees.be

### NIS2 Obligations
- ✅ Risk management program (partially implemented)
- ✅ Incident response procedures (partially documented)
- ⚠️ 24-hour early warning system (CCB)
- ⚠️ 72-hour full incident notification (CCB)
- ⚠️ 1-month final incident report (CCB)
- ⚠️ Supply chain security assessment (required)
- ⚠️ Third-party risk assessment (required)

**NIS2 Contacts:**
- CCB Incident Portal: https://notif.safeonweb.be
- CCB Website: https://ccb.belgium.be/en

### ISO 27001 Obligations
- ✅ 14 primary controls documented and assessed
- ⚠️ Annual control review and update
- ⚠️ Control effectiveness testing
- ⚠️ Evidence retention (3+ years recommended)

### CyFun Obligations
- ✅ Framework domains assessed
- ⚠️ "Important" level controls must be implemented
- ⚠️ Annual review and update
- ⚠️ Documentation of control implementations

---

## Identified Gaps

| Regulation | Gap | Priority | Owner |
|-----------|-----|----------|-------|
| GDPR | Data Protection Impact Assessment not documented | **Critical** | Security Team |
| NIS2 | 24-hour incident notification process not documented | **Critical** | Security Team |
| ISO 27001 A.12.4 | Logging incomplete (access switches not monitored) | **High** | Network Team |
| ISO 27001 A.12.6 | Vulnerability management process not documented | **High** | Security Team |
| CyFun | Incident response testing not documented | **Medium** | Security Team |
| AI Act | Separate AI governance assessment not completed | **Medium** | Security Team |

---

## Recommendations

### Phase 1 (Immediate - Next 30 Days)
1. ✅ Complete Data Protection Impact Assessment (GDPR)
2. ✅ Document NIS2 incident notification procedures
3. ✅ Integrate access switch logging into centralized Syslog
4. ✅ Document vulnerability management process

### Phase 2 (Next 90 Days)
1. ✅ Conduct AI Act governance assessment
2. ✅ Perform incident response testing
3. ✅ Complete supply chain security assessment
4. ✅ Test 72-hour breach notification procedures

### Phase 3 (Strategic - 6-12 Months)
1. ✅ Pursue ISO 27001 certification (if required by NVIDIA)
2. ✅ Implement CyFun "Essential" level controls (if business grows)
3. ✅ Establish annual regulatory review process

---

## Conclusion

The NVIDIA Regional R&D Hub network is subject to **4 primary regulations** (GDPR, NIS2, ISO 27001, CyFun) and operates in a **high-compliance environment**. The network design demonstrates good foundational security; however, **monitoring and documentation gaps** create compliance risks for GDPR and NIS2 breach notifications.

**Audit Scope:** This GRC audit assesses compliance with GDPR, NIS2, ISO 27001, and CyFun using the network design documents and security configuration as the evidence baseline.

---

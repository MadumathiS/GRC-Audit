# Scope Statement — NVIDIA Regional R&D Hub GRC Audit

**Project:** NVIDIA Regional R&D and Production Hub  
**Project ID:** NVIDIA-REG-RD-2026-1106  
**Audit Type:** Design-Based Compliance Assessment  
**Prepared By:** 

---

## Executive Summary

This GRC audit is a **design-based compliance assessment** of the NVIDIA Regional R&D Hub network against regulatory frameworks (GDPR, NIS2, ISO 27001, CyFun). The audit examines the network design documentation, security configuration, device settings, and testing reports to identify compliance gaps and security weaknesses. This is **not a penetration test or operational audit** — it does not test live systems or verify that policies are actually followed in practice.

---

## Audit Scope

### What IS Included (In Scope)

**Network Infrastructure:**
- 2 Spine Layer 3 switches (core routing)
- 3 Leaf Layer 3 switches (distribution)
- 6 Access switches (department connectivity)
- 1 Edge Router (Internet gateway)
- 1 Firewall (perimeter security)
- 9 Infrastructure servers (DNS, DHCP, AAA, Syslog, storage, compute)
- 48 Workstations (across 6 departments)
- 10 VLANs (logical segmentation)
- 1 DMZ (public-facing services)

**Security Controls Assessed:**
- ✅ Network segmentation (VLAN design and configuration)
- ✅ Access control (ACLs, SSH, firewall rules)
- ✅ Logging and monitoring (Syslog, event capture)
- ✅ DMZ isolation (firewall and ACL rules)
- ✅ Redundancy and high availability (OSPF, ECMP, dual Spines)
- ✅ Authentication (SSH, AAA/RADIUS)
- ✅ Management security (encryption, access control)

**Data Assets:**
- ✅ Employee personal data (names, national IDs, salaries)
- ✅ R&D intellectual property
- ✅ Production artifacts
- ✅ Financial records
- ✅ System logs and audit trails

**Regulations/Standards Assessed:**
- ✅ GDPR (General Data Protection Regulation)
- ✅ NIS2 (Network and Information Security Directive 2)
- ✅ ISO/IEC 27001:2022 (Information Security Management)
- ✅ CyFun (Belgian Cybersecurity Framework)

**Documentation Reviewed:**
- ✅ Network design document (PDF)
- ✅ Security configuration report (DOCX)
- ✅ Device configuration documentation (DOCX)
- ✅ VLAN and subnet worksheet (DOCX)
- ✅ Network implementation report (PDF)
- ✅ Testing report (PDF)
- ✅ Packet Tracer simulation file (.pkt)
- ✅ Cost breakdown and equipment list
- ✅ Insurance and contract documents

---

### What IS NOT Included (Out of Scope)

**Systems & Testing:**
- ❌ Penetration testing or active hacking
- ❌ Testing live production systems
- ❌ Verification that staff actually follow policies
- ❌ Performance testing or capacity assessment
- ❌ Backup/recovery testing
- ❌ Disaster recovery validation

**Beyond Network Scope:**
- ❌ Physical security of data center
- ❌ Vendor/third-party security assessments
- ❌ End-user security awareness training
- ❌ Business continuity planning
- ❌ Legal contract review
- ❌ Insurance policy analysis

**Regulatory Scope Exclusions:**
- ❌ Cyber Resilience Act (CRA) — Not a product manufacturer
- ❌ DORA — Not a financial institution
- ❌ AI Act governance — Separate assessment required

---

## Scope Boundaries

### Geographic Scope
**Location:** Belgium (Brussels region)  
**Applies to:** NVIDIA Regional R&D and Production Hub only  

### Temporal Scope
**Design Date:** July 1, 2026  
**Audit Date:** September 15, 2026  
**Assessment Period:** Design baseline to current date  

### Organizational Scope
**Organization:** NVIDIA Corporation (Regional Hub)  
**Employees:** 48 workstations deployed  
**Contractors:** Not separately assessed  

---

## Audit Methodology

### Assessment Approach

**1. Document Review** (Completed)
- Network design document review
- Security configuration analysis
- Device configuration verification
- Testing report review
- Regulatory requirement mapping

**2. Design Verification** (Completed)
- Network topology validation against design
- VLAN configuration against requirements
- ACL rules against security policy
- Firewall rules against requirements
- Server configuration against requirements

**3. Compliance Mapping** (Completed)
- GDPR data protection requirements
- NIS2 security obligation requirements
- ISO 27001 control assessment
- CyFun framework domain assessment

**4. Gap Identification** (Completed)
- Missing controls identification
- Partial control implementation
- Configuration weaknesses
- Monitoring gaps

**5. Risk Rating** (In Progress)
- Likelihood assessment
- Impact assessment
- Severity calculation (Likelihood × Impact)

---

## Evidence Sources

### Primary Evidence
- Network design document (PDF)
- Security configuration report (Word doc)
- Device configuration documentation (Word doc)
- VLAN worksheet (Word doc)
- Network implementation report (PDF)
- Testing report (PDF)
- Packet Tracer simulation file

### Secondary Evidence
- Cost breakdown document
- Insurance contract (Hiscox)
- Network implementation contract (Full Duplex)
- Architecture diagrams (PNG images)
- Floorplan diagram (PNG images)
- Physical diagram (JPG image)

### Not Available/Out of Scope
- Actual device configurations (would need network access)
- Running logs or audit trails
- Staff interview results
- Policy documents
- Procedure manuals

---

## Limitations & Disclaimers

### What This Audit Does

✅ **Compare design** against regulatory requirements and standards  
✅ **Identify gaps** between design and compliance requirements  
✅ **Rate findings** by severity using consistent methodology  
✅ **Provide evidence citations** for all findings  
✅ **Recommend remediation** for identified gaps  
✅ **Document findings** in audit-standard format  

### What This Audit Does NOT Do

❌ **Conduct penetration testing** — No active hacking or vulnerability scanning  
❌ **Test live systems** — Design assessment only  
❌ **Verify actual practices** — Documentation-based assessment  
❌ **Provide legal advice** — Findings are technical; legal review needed  
❌ **Guarantee security** — Absence of findings ≠ absence of vulnerabilities  
❌ **Assess vendors** — Third-party security not assessed  
❌ **Test disaster recovery** — Only design reviewed  

### Known Constraints

**Access Limitations:**
- Cannot access actual running devices
- Cannot verify actual configurations match design
- Cannot test actual incident response procedures
- Cannot verify staff training/compliance

**Documentation Limitations:**
- Some device-level details from Packet Tracer simulation (not full production specs)
- No operational procedures documentation available
- No security policies provided (referenced but not supplied)

**Timeline Limitations:**
- One-time assessment; does not track ongoing compliance
- No continuous monitoring assessment
- No trend analysis available

**Scope Limitations:**
- Network only (not facility, not organizational)
- Design-based (not operational)
- Assessment-only (not remediation or testing)

---

## Key Assumptions

1. **Network design document is accurate** — Design matches intended production architecture
2. **Configuration descriptions are complete** — Security config doc captures all deployed controls
3. **Testing report results are valid** — Tests conducted correctly; results are accurate
4. **Regulatory environment stable** — No major regulatory changes since design date (July 2026)
5. **Design represents current state** — No undocumented changes have been made
6. **Scope boundaries clear** — Regional hub is the only system being assessed

---

## Stakeholders & Communication

### Project Sponsors
- **NVIDIA Project Sponsor:** [Contact Name/Email]
- **Contractor (Full Duplex):** [Contact Name/Email]

### Audit Team
- **GRC Lead:** [Audit Lead Name]
- **Technical Auditor:** [Name]
- **Documentation Lead:** [Name]

### Regulatory Contacts
- **GDPR Authority (APD/GBA):** https://moncompte.autoriteprotectiondonnees.be
- **NIS2 Authority (CCB):** https://notif.safeonweb.be
- **Insurance (Hiscox):** [Policy reference number]

---
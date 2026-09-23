# Scope Statement — NVIDIA Regional R&D Hub GRC Audit

**Project ID:** NVIDIA-REG-RD-2026-1106  
**Audit Period:** September 2026  
**Audit Scope:** Network Infrastructure (Layers 1–4; Layer 7 excluded)  
**Status:** FINAL

---

## 1. AUDIT OBJECTIVES

This audit evaluates whether the NVIDIA Regional R&D Hub network infrastructure complies with four regulatory frameworks:

1. **ISO 27001:2022** — Information Security Management System (15 of 48 controls evaluated)
2. **GDPR Article 32** — Technical measures for personal data protection
3. **NIS2 Directive Article 21** — Network & Information Security requirements
4. **CyberFundamentals (CyFun)** — Belgium's de facto NIS2 implementation framework

The audit seeks to identify **gaps between required controls and actual implementation**, rate each gap by risk, and recommend remediation with timeline and owner.

---

## 2. IN-SCOPE AREAS

### 2.1 Network Architecture (✅ Fully Evaluated)

- **Spine-Leaf topology:** Redundancy, OSPF ECMP routing, 2-hop design
- **VLAN segmentation:** 10 VLANs across 3 Leaf switches, per-department isolation
- **Access layer:** 6 access switches, per-sector connectivity
- **Trunk configuration:** VLAN allowed lists, native VLAN hardening
- **Point-to-point links:** IP addressing, OSPF adjacencies

### 2.2 Network Segmentation (✅ Fully Evaluated)

- **Inter-VLAN access control:** ACLs on Layer 3 interfaces (SVI ingress)
- **DMZ isolation:** VLAN 80 from internal subnets (VLAN 10–60, 70, 90)
- **Guest isolation:** VLAN 99 restricted to internet-only access
- **Broadcast domain separation:** Trunk pruning, VLAN tagging verification
- **Dynamic ARP Inspection (DAI):** Not evaluated (not deployed in PT)

### 2.3 Access Control — Management (✅ Evaluated)

- **SSH configuration:** Version 2, crypto keys, key exchange algorithms (where deployed)
- **VTY/console access:** Authentication, privilege levels
- **SSH coverage:** Presence/absence on all Layer 3 devices (Spines, Leaves, Routers, ASA)
- **Telnet:** Documented presence or absence
- **Local credentials:** Username, password policy (if documented), RSA key length

**Note:** Does NOT evaluate organizational access control (IAM, LDAP, SAML, MFA platforms — out of scope).

### 2.4 Centralized Authentication & AAA (✅ Evaluated)

- **RADIUS configuration:** Server address, port, shared key, timeout/retry
- **AAA chain:** Authentication source priority (RADIUS → local fallback)
- **802.1X port authentication:** Configuration on access switches (if present)
- **Local fallback credentials:** Strength assessment (length, complexity, uniqueness)
- **Multi-factor authentication:** Presence or absence

**Note:** Does NOT evaluate application-level authentication (web login, SSH key-based auth, OAuth).

### 2.5 Logging & Monitoring (✅ Evaluated)

- **Syslog server:** IP address, UDP port, protocol
- **Device logging:** Device-level configuration (which devices forward logs)
- **Log sources:** Syslog facility levels (local0–local7)
- **Firewall logging:** ASA ACL hits, NAT events (if configured)
- **Authentication logging:** Failed login attempts, privilege escalation
- **Log aggregation:** Central syslog server reachability

**Note:** Does NOT evaluate log analysis, SIEM correlation, or forensic retention.

### 2.6 Data Protection in Transit (✅ Partially Evaluated)

- **Encryption for management:** SSH (yes/no per device)
- **Encryption for data-plane traffic:** IPsec, TLS between internal systems (not configured)
- **Cleartext protocols:** HTTP, FTP, Telnet presence/absence

**Note:** Does NOT evaluate data-at-rest encryption (storage layer, not network scope).

### 2.7 Perimeter Security (✅ Evaluated)

- **Firewall rules:** ACL configurations (outside-in, inside-out)
- **NAT/PAT configuration:** Dynamic vs. static translation rules
- **DMZ traffic inspection:** Firewall positioned between DMZ and internal
- **Internet gateway:** Redundancy, failover (if configured)
- **Firewall logging:** Device-level log forwarding (if supported)

### 2.8 High Availability & Redundancy (✅ Evaluated)

- **Dual spines:** Presence, ECMP load-balancing
- **Link redundancy:** Point-to-point connections per spine
- **Internet gateway redundancy:** Backup router availability
- **Server redundancy:** Single vs. multiple instances (DNS, DHCP, RADIUS, Syslog)
- **Backup & recovery:** Design documentation, restore testing

### 2.9 Supply Chain & Vendor Security (✅ Evaluated)

- **Vendor contract:** Security clauses, incident response SLA, audit rights
- **Hardware sourcing:** New vs. refurbished, supplier due diligence
- **Lifecycle management:** Warranty, support terms, end-of-life procedures
- **Insurance:** Coverage for hardware + cyber risk

---

## 3. OUT-OF-SCOPE AREAS

### 3.1 Endpoint Security (❌ Out of Scope)

- ❌ Workstation firewalls (Windows Defender, iptables, macOS firewall)
- ❌ Antivirus/anti-malware software
- ❌ Endpoint Detection & Response (EDR) agents
- ❌ USB port restrictions, full-disk encryption per workstation
- ❌ Web filtering/proxy (client-side)

**Reason:** This audit focuses on network infrastructure (Layers 1–4), not endpoint security (Layer 5+). Workstations are assumed to follow basic hardening; detailed workstation inventory is limited to count & GPU capability.

### 3.2 Application Security (❌ Out of Scope)

- ❌ Web application firewall (WAF) rules
- ❌ OWASP Top 10 controls on web servers
- ❌ SQL injection / XSS prevention
- ❌ API authentication (OAuth, API keys, JWT)
- ❌ Database encryption, access control
- ❌ Application-level logging (Apache, Django, etc.)

**Reason:** Application security is Layer 7 and above. This audit is Layers 1–4.

### 3.3 Identity & Access Management (IAM) (❌ Out of Scope)

- ❌ User provisioning/deprovisioning workflows
- ❌ LDAP/Active Directory integration
- ❌ Single Sign-On (SSO) platforms
- ❌ Multi-factor authentication (MFA) systems (e.g., Okta, Duo)
- ❌ Role-based access control (RBAC) policies per application
- ❌ Session management (timeouts, concurrent login limits)

**Reason:** IAM is a distinct domain; this audit assumes AAA infrastructure (RADIUS) exists and evaluates its basic configuration only.

### 3.4 Incident Response & Procedures (❌ Out of Scope)

- ❌ Incident response plan / runbook (organizational procedure)
- ❌ Breach notification workflows
- ❌ Forensics & evidence collection procedures
- ❌ Crisis communication / stakeholder notification
- ❌ Business continuity / disaster recovery (BCP/DRP) plans

**Reason:** This audit evaluates **technical capabilities** (logging, monitoring, backup design). **Organizational procedures** (incident response runbooks, communication plans) are out of scope but are essential for compliance (flagged as FINDING F-GAP-08: MEDIUM).

### 3.5 Security Awareness & Training (❌ Out of Scope)

- ❌ Phishing simulation campaigns
- ❌ Security policy acknowledgments
- ❌ Role-specific training (e.g., data handling for researchers)
- ❌ Security incident case studies / lessons learned

**Reason:** This is a people/process domain, not infrastructure.

### 3.6 Vulnerability Management (❌ Out of Scope)

- ❌ Vulnerability scanning results (Nessus, Qualys, OpenVAS)
- ❌ Patch management timelines
- ❌ Known CVE exposure assessment
- ❌ Penetration testing results

**Reason:** Vulnerability assessment is a separate engagement. This audit assumes all devices are patched and functional as configured.

### 3.7 Third-Party Security Assessments (❌ Out of Scope)

- ❌ Vendor security certifications (ISO 27001, SOC 2, FedRAMP)
- ❌ Supplier audit reports (questionnaires, assessments)
- ❌ Cloud service provider (CSP) security postures (AWS, Azure, GCP)
- ❌ Data center / colocation facility security

**Reason:** Vendor security is evaluated at the contract level only (FINDING F-GAP-10: MEDIUM — no security audit rights in contract).

### 3.8 Artificial Intelligence & Machine Learning (⚠️ Partially Out of Scope)

- ❌ AI system governance (model training, data provenance, bias testing)
- ❌ LLM safety & alignment (if Central-Compute-LLM is an LLM)
- ❌ AI Act Annex III high-risk use cases
- ❌ Automated decision-making systems

**Reason:** The dossier mentions a "Central-Compute-LLM" server but provides no documentation of what "LLM" means or what workload it runs. If it is a Large Language Model for training or inference, it may fall under the EU AI Act. **Flagged as risk in applicability-note.md; requires separate AI governance audit.**

**Recommendation:** Clarify with NVIDIA what the LLM workload is. If confirmed as Gen-AI, conduct separate AI Act gap analysis.

---

## 4. AUDIT METHODOLOGY

### 4.1 Control Framework & Selection

**Framework:** ISO 27001:2022 (primary) + GDPR Art. 32 + NIS2 Art. 21 + CyFun 2025 (secondary)

**Control Selection:** 15 controls across 5 areas

| Area | Controls | Source |
|---|---|---|
| **Network Segmentation** | 3 | ISO A.8.22, A.8.20 |
| **Access Control** | 3 | ISO A.8.5, A.8.2, NIS2 21(2)(i) |
| **Logging & Monitoring** | 3 | ISO A.8.15, A.8.16, NIS2 21(2)(b) |
| **Data Protection** | 3 | ISO A.8.13, GDPR Art. 32, NIS2 21(2)(c) |
| **High Availability & Supply Chain** | 3 | ISO A.8.14, A.5.19, NIS2 21(2)(d) |

**Rationale:** 15 controls represent 31% of ISO 27001 (48 controls). Selection prioritizes network infrastructure (in-scope) and highest-risk areas (logging, authentication, segmentation).

### 4.2 Control Evaluation Method

**Step 1: Define Pass Condition**
- State what "passing" means for this control based on the framework
- Example: "All Layer 3 devices (8 total) forward authentication logs to central Syslog server at UDP 514"

**Step 2: Identify Evidence Sources**
- Network design documents (PDFs, DOCX)
- Device configuration files (Cisco CLI, ASA config)
- Packet Tracer simulation (.pkt file)
- Test report (51-page testing document)

**Step 3: Run the Check**
- Open evidence documents and search for configuration statements
- Cross-reference to device CLI (where accessible)
- Note simulator limitations (Packet Tracer gaps)

**Step 4: Determine Result**
- ✅ **PASS:** Control implemented and verified; tests confirm functionality
- 🟡 **PARTIAL:** Control partially implemented; some devices/aspects working, others failing
- ❌ **FAIL:** Control not implemented, broken, or intentionally excluded

**Step 5: Trace to Finding**
- If FAIL or PARTIAL: Create finding document (F-GAP-01.md, etc.)
- Observation: What is wrong (facts only, no opinion)
- Evidence: Where in the dossier the gap appears (section reference)
- Risk: Why it matters (compliance impact, operational impact)
- Recommendation: Who should fix it, how long, what it costs

### 4.3 Evidence Sources

| Document | Type | Pages | Use In Audit |
|---|---|---|---|
| `Nvidia_network.pkt` | Packet Tracer simulation | Binary | Topology, device config verification (attempted; may be encrypted) |
| `Final_NVIDIA_Network_Security_Project_Testing_Report.pdf` | Test report | 51 | Validation of VLAN, firewall, logging, RADIUS tests |
| `NVIDIA_Project_Report.pdf` | Design overview | 60 | Architecture justification, project phases |
| `Network-Implementation-Report.pdf` | Technical doc | — | VLAN layout, device allocations, design rationale |
| `NVIDIA-Security-Configuration.docx` | Configuration reference | — | Firewall rules, ACLs, AAA setup, Syslog config |
| `NVIDIA-Device-Configuration.docx` | CLI reference | — | Device-level config (SSH, OSPF, SVI, etc.) |
| `NVIDIA-VLAN-Subnet-Worksheet.docx` | IP plan | — | VLAN addressing, DHCP pools, static IPs |
| `A-Well-Routed-Router.pdf` | Router guide | — | Router CLI reference |
| `why_leaf_and_spine.docx` | Architecture doc | — | Spine-Leaf justification, ECMP, redundancy |
| `Packet-tracer-limitations.docx` | Constraints doc | — | PT simulator gaps (CRITICAL for audit validity) |
| `NVIDIA_RFQ_v2.pdf` | Requirements | — | RFQ baseline (what was contracted) |
| `Contract.pdf` | Service agreement | — | Timeline, payment, warranties, incident SLA |
| `INSURANCE_CONTRACT___PROPOSAL.pdf` | Insurance proposal | — | Coverage limits, cyber risk requirements |
| `NVIDIA_Regional_R_D_Hub___Cost_Breakdown.pdf` | Cost analysis | — | Asset valuation, hardware sourcing |

### 4.4 Evidence Quality Assurance

- ✅ All findings cite specific section numbers (e.g., Security Config §2.2)
- ✅ Page numbers included where applicable
- ✅ Quotes verified to exist in source (not fabricated)
- ✅ Contradictions flagged (e.g., "Syslog only Leaf-1" vs. "all devices forward logs")
- ✅ Simulator limitations acknowledged (PT not production-equivalent)

---

## 5. LIMITATIONS & CAVEATS

### 5.1 Packet Tracer Simulation Limitations

The dossier documents **10 known PT simulator constraints** that affect audit findings:

| # | Feature | PT Status | Production Behavior | Audit Impact |
|---|---|---|---|---|
| 1 | SVI ACL enforcement (Catalyst 3650) | NOT enforced (except GUEST-ACL) | Fully supported | F-GAP-02: Enforcement assumption may differ in production |
| 2 | RADIUS server configuration | Commands rejected | Fully supported | F-GAP-05: Cannot validate RADIUS on PT; must test on real hardware |
| 3 | 802.1X port-control | Not persistent | Fully supported | Out of scope (not evaluated) |
| 4 | Syslog on Catalyst switches | Commands rejected | Fully supported | F-GAP-01: Cannot enable Syslog on switches in PT |
| 5 | Syslog on ASA firewall | Commands rejected | Fully supported | F-GAP-01: Cannot enable firewall Syslog in PT |
| 6 | iSCSI protocol | Not simulated | Network-level only | F-GAP-07: iSCSI backup not simulated; untested |
| 7 | AI/LLM compute workload | Not simulated | N/A (application layer) | Out of scope |
| 8 | Dynamic ARP Inspection (DAI) | Not supported | Must implement in production | Not evaluated (would require separate PT extension) |
| 9 | Advanced ASA features (IPS/DPI/URL filtering) | Limited | Fully supported | Not evaluated (firewall configured as basic stateful) |
| 10 | Control Plane Policing (CoPP) | Not deployable | Must implement in production | Not evaluated |

**Implication:** Treat Packet Tracer results as **proof-of-configuration (what was configured), not proof-of-security (whether it actually works).**

**Validation Requirement:** Before production go-live, all findings must be re-validated on real hardware (Catalyst 3650, ASA, ISR devices).

### 5.2 Dossier Internal Contradictions

The evidence itself contains inconsistencies:

| Contradiction | Source 1 | Source 2 | Audit Impact |
|---|---|---|---|
| **Syslog server IP** | VLAN Worksheet: 192.168.70.16 | Testing Report: 203.0.113.6 | Unclear which is correct; 203.0.113.6 is firewall outside IP, likely error |
| **RADIUS status** | Testing Report §9.2: "PASS" | Security Config §7.4: "does not function" | Contradictory; audit trusts Security Config admission |
| **VLAN 1 hardening** | Security Config §11: "Never performed… identified gap" | Audit checklist (team): "VLAN 1 removed, VLAN 999 native" | Dossier contradicts original team submission (reason for correction) |
| **VLAN count** | Multiple references: "9 VLANs" | Some: "10 VLANs" (including VLAN 1) | Minor; impact on VLAN 1 hardening gap |
| **ACL test result** | Testing Report: "PASS" | Security Config §1.4: "not enforced in PT" | Audit trusts Security Config limitation note |

**Resolution:** These contradictions are flagged in findings and should be resolved with the contractor before production acceptance.

### 5.3 Incomplete Documentation

Some areas lack supporting evidence:

| Area | Finding | Status |
|---|---|---|
| **Access Control Matrix** | RFQ §3 asks for matrix defining inter-dept traffic rules | Not found in dossier |
| **Performance testing** | RFQ §3 asks for throughput/latency validation | Not found in evidence |
| **Backup procedures** | No backup design, restore procedures, or recovery time objectives (RTO/RPO) documented | F-GAP-07: MEDIUM |
| **Incident response plan** | No runbook for breach investigation, containment, eradication, recovery | F-GAP-08: MEDIUM |
| **Data inventory** | No list of what data is stored where (GDPR Art. 30 requirement) | Out of audit scope (organizational) |

**Audit Approach:** For missing documentation, findings state what **should** be there and recommend development.

### 5.4 Time Constraint

This audit covers **15 controls** (31% of ISO 27001's 48 controls). Notable omissions:

- Governance controls (A.5.1–5.23): Not evaluated
- Physical security (A.7.1–7.15): Not evaluated
- Cryptography (A.6.1–6.2): Partially evaluated (SSH only)
- Supplier management (A.5.19–5.21): Partially evaluated (contract only)
- Compliance management (A.5.36–5.37): Not evaluated

**Implication:** This audit provides a snapshot of network security, not comprehensive compliance certification.

---

## 6. ASSUMPTIONS

1. **Devices are functional as configured.** The audit assumes all Cisco IOS, ASA, and server configurations are correct and deployed correctly. Typos in configuration are NOT assumed.

2. **Packet Tracer accurately represents device behavior except where documented.** PT simulator gaps (#1–10 above) are known; other behavior is assumed to match production hardware.

3. **Test report is accurate.** The 51-page testing document represents the contractor's honest reporting of test results (not independent third-party verification).

4. **Network is static.** The audit treats the network as a static design as of 21 September 2026. Post-deployment changes are NOT in scope.

5. **Workstations follow basic hardening.** The audit does not assess individual workstations; assumes they are patched, firewalled, and anti-malware-protected.

6. **Personnel are trusted.** The audit does not evaluate insider threats or physical security (e.g., rogue admin, physical access to switch).

7. **Internet connection is available.** The audit assumes reliable internet uplink to 203.0.113.0/30; does not evaluate ISP SLA or upstream security.

---

## 7. EXCLUSIONS (Things NOT Evaluated)

- ❌ Previous versions of the network (this is a new deployment)
- ❌ Vendor compliance certifications (we do not audit Cisco, Dell, or contractor's certifications)
- ❌ Physical security (locks, cameras, badge access to server room)
- ❌ Disaster recovery procedures (outside audit scope)
- ❌ Cost-benefit analysis of recommendations (audit proposes fixes; NVIDIA decides)
- ❌ Compliance with NVIDIA's internal policies (only ISO/GDPR/NIS2/CyFun)

---

## 8. SUCCESS CRITERIA (How to Know the Audit Was Done Right)

✅ **Every finding cites specific evidence.** (Section §, page #, quote)  
✅ **Risk ratings applied uniformly.** (Likelihood × Impact scale defined before rating)  
✅ **Checklist result traces to finding.** (FAIL → Finding; PASS → no finding)  
✅ **README summary aligns with risk register.** (No contradictions)  
✅ **Limitations acknowledged.** (PT gaps, dossier contradictions documented)  
✅ **Findings include recommendation.** (Owner, timeline, cost for each)  
✅ **Confidence level stated.** (HIGH if hardware tested; MEDIUM if PT-only; LOW if uncertain)  

---

## 9. NEXT STEPS AFTER AUDIT COMPLETION

### Phase 1: Review & Acceptance (Days 1–7)

- [ ] NVIDIA reviews findings and accepts risk ratings
- [ ] Contractor responds to each finding with remediation plan
- [ ] Legal/Compliance reviews NIS2/GDPR implications

### Phase 2: Production Validation (Days 8–14)

- [ ] Real hardware testing (Catalyst 3650, ASA, ISR devices)
- [ ] ACL enforcement verified on production switches
- [ ] RADIUS server deployed and tested on real authenticator
- [ ] Syslog forwarding confirmed on firewall and switches

### Phase 3: Remediation (Days 15–30)

- [ ] CRITICAL findings (F-GAP-01) fixed
- [ ] HIGH findings (F-GAP-02 through F-GAP-05) in progress
- [ ] Go-live blocked until above complete

### Phase 4: Post-Deployment (Days 31–90)

- [ ] MEDIUM findings remediated per timeline
- [ ] Incident response procedure drafted and tested
- [ ] Backup/recovery procedures validated
- [ ] Supplier contract updated with security clauses

---

**Scope Statement Status:** FINAL  
**Audit Period:**  September 2026  
**Framework Version:** ISO 27001:2022, GDPR as of Dec 2023, NIS2 as of Dec 2024, CyFun 2025

---

*"An audit's scope defines what was examined and what was not. Clear scope boundaries protect both the auditor's findings and the client's expectations. This scope statement shows that we evaluated network security controls, documented simulator limitations, and flagged areas beyond this audit's jurisdiction but critical for compliance (incident response, backup, AI governance)."*
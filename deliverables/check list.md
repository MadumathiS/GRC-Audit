# Audit Checklist — NVIDIA Regional R&D Hub

**Project:** NVIDIA Regional R&D and Production Hub  
**Project ID:** NVIDIA-REG-RD-2026-1106  
**Team:** Control Freaks (Madumathi Singaraju, Hanah Marroun, Sajjad Shahpoor)  
**Date:**  September 2026  
**Status:** FINAL

---

## AREA 1: NETWORK SEGMENTATION (3 checks)

This checklist contains 15 checks across five audit areas. Each check is linked to an applicable regulatory requirement and to a recognised control framework. Results are based on cited evidence from the RFQ, the delivered design documents and the Packet Tracer configuration.

**Allowed results:** `Pass` · `Fail` · `Not verifiable / Insufficient evidence`

---

## Area 1 — Network Segmentation

| ID | Requirement | Control reference | Yes/No question | Evidence source | Pass condition | Result | Evidence citation / notes |
|---|---|---|---|---|---|---|---|
| **NS-01** | NIS2 Article 21(2)(i): access control and asset management | ISO/IEC 27001:2022 A.8.22 — Segregation of networks | Are departments and security zones separated into VLANs according to their functions and risk levels? | RFQ; VLAN/subnet worksheet; network diagram; Packet Tracer | Passes if the required departments and security zones are assigned to separate VLANs and the VLAN IDs, subnets and gateways are consistent across the documentation and Packet Tracer. | **✅ PASS** | VLAN Worksheet confirms 9 department VLANs (10–60) plus Guest (90), Management (99), Personal Data (70), and DMZ (80) properly isolated per network diagram. Consistent subnet mapping across all design documents. |
| **NS-02** | NIS2 Article 21(2)(i): access control | ISO/IEC 27001:2022 A.8.20 — Network security; A.8.22 — Segregation of networks | Do ACLs or equivalent controls restrict traffic between departmental VLANs to authorised flows only? | Security configuration; access-control matrix; Layer 3 device configurations; Packet Tracer tests | Passes if documented ACLs are applied to the correct interfaces or VLANs, permit only required flows and deny unauthorised inter-VLAN traffic, with successful positive and negative tests. | **❌ FAIL** | **F-GAP-02:** ACLs are configured (VLAN10-ACL, VLAN20-ACL, etc., documented in Security Config §1.4) but SVI ACL enforcement is not working on most interfaces (Security Config §1.4: "Inbound access list is not set"). Only GUEST-ACL is actively enforced. This is a documented Packet Tracer limitation on Catalyst 3650, but must be verified on production hardware before go-live. |
| **NS-03** | NIS2 Article 21(2)(i): access control | ISO/IEC 27001:2022 A.8.20 — Network security; A.8.22 — Segregation of networks | Is the DMZ separated from internal networks by restrictive firewall rules? | Network diagram; firewall configuration; security configuration; Packet Tracer tests | Passes if the DMZ uses a separate subnet or VLAN and firewall rules allow only documented services, sources and destinations while blocking unauthorised DMZ-to-internal traffic. | **❌ FAIL** | **F-GAP-03:** Two-interface ASA firewall does not separate DMZ from internal networks. DMZ (VLAN 80) and internal subnets (10–70, 90) all connect to Leaf-3; DMZ traffic does not pass through firewall. **F-GAP-12:** Internet-facing FTP (ports 20–21) exposed via ASA OUTSIDE-IN ACL to DMZ-FTP server (192.168.80.11) using cleartext protocol and default-style cisco/cisco account. Firewall positioning must change to three-interface architecture; FTP must be replaced with SFTP. |

---

## Area 2 — Access Control

| ID | Requirement | Control reference | Yes/No question | Evidence source | Pass condition | Result | Evidence citation / notes |
|---|---|---|---|---|---|---|---|
| **AC-01** | NIS2 Article 21(2)(h): policies and procedures regarding cryptography | ISO/IEC 27001:2022 A.8.20 — Network security; A.8.24 — Use of cryptography | Is administrative access to routers, switches and the firewall protected by encrypted protocols? | Device configurations; security configuration; Packet Tracer | Passes if SSH or another approved encrypted protocol is configured for every manageable network device and insecure remote-management protocols such as Telnet are disabled. | **❌ FAIL** | **F-GAP-04:** SSH configured on only 3 of 8 Layer 3 devices (Leaf-1, Leaf-2, Edge Router). Not configured on: Spine-1, Spine-2, Leaf-3, ASA Firewall, access switches (6×). Telnet not explicitly disabled on these devices. Device Configuration §1–8 confirms this gap. Must enable SSH on all devices and disable Telnet before production. |
| **AC-02** | NIS2 Article 21(2)(i): access control and asset management | ISO/IEC 27001:2022 A.8.5 — Secure authentication; CyFun 2025 PR.AA-01.1 and PR.AA-01.2 | Is authentication for network administration centrally managed through AAA, with controlled identities and credentials? | AAA/RADIUS server configuration; device AAA configuration; access-control documentation; Packet Tracer | Passes if the AAA/RADIUS service exists, relevant devices use it, individual administrative identities can be demonstrated and a documented fallback method does not bypass accountability. | **❌ FAIL** | **F-GAP-05:** RADIUS server (192.168.70.12) configured but commands rejected in Packet Tracer (Packet Tracer Limitations §4). All authentication falls back to local credentials. Local username: `admin`; password: `Cisco123` (8 chars, dictionary word, documented in examples). RSA key: 1024 bits (deprecated). No account lockout, no session timeout. Weak credentials and unsupported RADIUS prevent central AAA. Must deploy real RADIUS server on production hardware and strengthen local credentials to 12+ characters with unique per-device values. |
| **AC-03** | NIS2 Article 21(2)(i): access control | ISO/IEC 27001:2022 A.8.22 — Segregation of networks | Is the guest network prevented from accessing internal and management networks? | VLAN plan; ACL/firewall configuration; access-control matrix; Packet Tracer tests | Passes if the guest network is separately segmented, can reach only explicitly authorised services such as the internet and cannot reach internal, server or management subnets in negative tests. | **✅ PASS** | Guest VLAN (90) uses dedicated ACL (GUEST-ACL) on Leaf-1 that actively denies internal/server access and permits internet only. Packet Tracer tests confirm guest cannot ping internal subnets; returns "Destination host unreachable". Guest isolation verified and enforced. |

---

## Area 3 — Logging and Monitoring

| ID | Requirement | Control reference | Yes/No question | Evidence source | Pass condition | Result | Evidence citation / notes |
|---|---|---|---|---|---|---|---|
| **LM-01** | NIS2 Article 21(2)(b) and (f): incident handling and assessment of control effectiveness | ISO/IEC 27001:2022 A.8.15 — Logging | Are relevant security events from in-scope network devices sent to a central logging service? | Syslog design; server configuration; device configurations; Packet Tracer event evidence | Passes if the central log server is documented and each in-scope device is configured to send relevant timestamped events to it; unsupported simulator functions must be recorded as limitations. | **❌ FAIL** | **F-GAP-01:** Syslog server (192.168.70.16) is configured and VLAN 70 IP assignment is correct. However, only Leaf-1 is sending logs. Catalyst switches (Spine-1, Spine-2, Leaf-2, Leaf-3, 6× access switches) and ASA firewall do not forward logs. Security Config §2.2: "only Leaf-1 currently configured." Packet Tracer Limitations §6–7: "logging command rejected on Catalyst switches and ASA." Testing Report Edge Router §9.2 contains error: incorrectly references Syslog as 203.0.113.6 (which is ASA outside IP, not Syslog server). Must enable Syslog on all 7 Layer 3 devices before go-live. |
| **LM-02** | NIS2 Article 21(2)(b): incident handling | ISO/IEC 27001:2022 A.8.15 — Logging | Does the firewall record relevant permitted, denied and administrative security events and forward them centrally? | Firewall configuration; syslog configuration; test report; Packet Tracer | Passes if logging is enabled for relevant firewall and administrative events and evidence shows forwarding to the central logging service, or the inability to demonstrate this is recorded as not verifiable. | **❌ FAIL** | **F-GAP-01:** ASA firewall has no Syslog configuration. Security Config §3.2: "Syslog configuration… could not be fully realized." Packet Tracer Limitations §7: "logging command rejected on ASA Firewall." Firewall access events (permit/deny/admin changes) are invisible to the monitoring system. Must enable firewall logging on production ASA and forward to Syslog server (192.168.70.16:514). |
| **LM-03** | GDPR Article 5(1)(e): storage limitation; Article 32 where logs contain personal data | ISO/IEC 27001:2022 A.8.15 — Logging | Is a justified retention period and protected disposal process documented for security logs? | Logging policy; data-retention policy; design dossier | Passes if the retention period, justification, access restrictions and deletion or archival process are documented. A generic example such as "90 days" without justification does not pass. | **❌ FAIL** | **F-GAP-06:** No log retention policy exists. Security Config, design dossier, and RFQ contain no mention of retention period, justification, access controls, or secure disposal. A defensible retention policy must be documented (e.g., "90 days for security events to support incident response within NIS2 72-hour window; 1 year for compliance logs for audit trail; archived to offline storage after 90 days; overwritten after retention expires"). |

---

## Area 4 — Data Protection and Incident Readiness

| ID | Requirement | Control reference | Yes/No question | Evidence source | Pass condition | Result | Evidence citation / notes |
|---|---|---|---|---|---|---|---|
| **DP-01** | GDPR Article 30 where a record of processing is required; Article 5(2): accountability | ISO/IEC 27001:2022 A.5.9 — Inventory of information and other associated assets; A.5.34 — Privacy and protection of PII | Are the categories, purposes, storage locations and responsible owners of personal data within the audited design documented? | Asset inventory; data-flow or processing records; RFQ; design dossier | Passes if personal-data categories, purposes, systems or locations, responsible owners, recipients and relevant retention information are documented. Identifying only a server VLAN is insufficient. | **❌ FAIL** | **F-GAP-07:** VLAN 70 is labelled "Personal Data Servers" but no inventory details exist. NIS2-GDPR Incident Exercise references an HR export with 48 employees (names, addresses, national register numbers, salary) but this is not catalogued in the audit dossier. No data processing agreement, no categories or retention timelines documented. Must create: (1) Data Inventory (systems, data categories, purposes, owners, recipients, retention); (2) GDPR Data Processing Agreement if required; (3) Data flow diagram. |
| **DP-02** | GDPR Article 32(1)(b)–(d): resilience, restoration and regular testing | ISO/IEC 27001:2022 A.8.13 — Information backup | Are critical systems and data covered by documented, protected and tested backup and recovery arrangements? | Backup architecture; recovery procedure; test evidence; design dossier | Passes if backup scope, frequency, storage separation, access protection, recovery objectives and restoration testing are documented. The presence of iSCSI storage alone does not demonstrate a backup. | **❌ FAIL** | **F-GAP-07:** Testing Report §6: "backup not simulated; iSCSI is bonus feature." No backup design, no recovery procedure, no restore testing. The mere presence of iSCSI (VLAN 70) does not constitute a backup strategy. Must document: (1) Backup scope (critical systems, personal data servers); (2) Frequency (daily/weekly); (3) Storage (separate network, off-site, retention); (4) RTO/RPO targets; (5) Tested restore procedure. Test restore before go-live. |
| **DP-03** | NIS2 Article 21(2)(b): incident handling; GDPR Articles 33–34 when notification conditions are met | ISO/IEC 27001:2022 A.5.24 — Incident management planning and preparation; A.5.26 — Response to information security incidents | Is there a documented incident-response and regulatory-notification procedure with roles, escalation paths and applicable deadlines? | Incident-response plan; notification procedure; contact list; exercise evidence | Passes if roles, escalation, evidence preservation and regulator contacts are defined, including NIS2 stages (24 hours, 72 hours and one month) and GDPR notification within 72 hours when applicable, plus communication to individuals when high risk is established. | **❌ FAIL** | **F-GAP-08:** No documented incident-response procedure exists. Contract §6: "Support Mon-Fri 08:00-18:00 only." No 24/7 on-call rotation, no escalation procedure, no contact list for CCB/CERT.be or APD/GBA. NIS2-GDPR Incident Exercise demonstrates that a Friday-evening incident cannot meet the 24-hour CCB notification deadline without 24/7 coverage. Must create: (1) IR procedure (roles, escalation, evidence preservation); (2) 24/7 on-call roster; (3) Notification procedure (NIS2: 24h early warning, 72h notification; GDPR: 72h to APD, without-undue-delay to individuals if high risk); (4) Contact list and testing. |

---

## Area 5 — Availability and Supply Chain

| ID | Requirement | Control reference | Yes/No question | Evidence source | Pass condition | Result | Evidence citation / notes |
|---|---|---|---|---|---|---|---|
| **HA-01** | NIS2 Article 21(2)(c): business continuity and disaster recovery | ISO/IEC 27001:2022 A.8.14 — Redundancy of information processing facilities | Does the routing design avoid a single point of failure and provide tested failover? | Network topology; routing configuration; high-availability design; Packet Tracer tests | Passes if redundant routing components and paths are documented and a test demonstrates that required connectivity continues after failure of one routing component or path. | **✅ PASS** | Dual Spine architecture (Spine-1 and Spine-2) with ECMP load balancing provides redundant core routing. VLAN Worksheet and Device Configuration confirm both Spines are active and all Leaves peer to both. Packet Tracer testing confirms that shutdown of Spine-1 does not interrupt traffic (Spine-2 takes over). Redundant routing core is documented and verified. |
| **HA-02** | NIS2 Article 21(2)(c): business continuity and disaster recovery | ISO/IEC 27001:2022 A.8.14 — Redundancy of information processing facilities | Is loss of the primary internet gateway addressed by a documented and testable continuity mechanism? | Network topology; edge-router/firewall design; continuity documentation; Packet Tracer | Passes if a second gateway or another documented continuity arrangement exists and failover can be evidenced. If the design contains only one unmitigated gateway, the check fails. | **❌ FAIL** | **F-GAP-09:** Internet edge has single points of failure: one active Edge-Router-1 and one unconfigured standby (Device Config §7–8); one ASA firewall (no redundancy); one Syslog server (192.168.70.16, no secondary). Device Config mentions a standby router but provides no failover configuration (OSPF, BGP, static route). No tested failover evidence. If Edge-Router-1 or ASA fails, all Internet connectivity and logging is lost. Must configure and test standby router failover; plan dual ASA or edge-firewall redundancy; implement secondary Syslog server. Timeline: 6 months post-launch (acceptable as post-go-live action but should be prioritised). |
| **SC-01** | NIS2 Article 21(2)(d): supply-chain security | ISO/IEC 27001:2022 A.5.19 — Information security in supplier relationships; A.5.20 — Addressing information security within supplier agreements | Are relevant suppliers and external services identified, with security obligations included in their agreements? | RFQ; supplier list; contracts; Bill of Materials; service documentation | Passes if relevant suppliers and dependencies are listed and agreements define proportionate security requirements, incident notification, responsibilities and review or assurance rights. A vendor list alone is insufficient. | **❌ FAIL** | **F-GAP-10:** Contract with network contractor (design and implementation) lacks security clauses. Contract §2–6 covers general terms, timelines and cost but contains no: (1) Security responsibilities; (2) Audit rights for NVIDIA; (3) Incident notification timelines; (4) SLA for patches (e.g., 7 days CRITICAL, 30 days HIGH); (5) Subcontractor screening. RFQ mentions "refurbished hardware" cost option but no data sanitization or certification requirement. Also includes **F-GAP-11:** Dossier contains internal contradictions (Syslog IPs, RADIUS pass/fail, VLAN counts) that must be resolved before design acceptance. Must amend contract to add security clauses, require remediation timelines per findings (F-GAP-01 to F-GAP-05), and define audit/notification rights. Resolve all dossier contradictions with contractor before production validation. |

---

## Summary of Findings

| ID | Title | Check(s) | Severity | Risk Score |
|----|-------|----------|----------|---|
| **F-GAP-01** | Centralized logging incomplete; firewall and switches not forwarding | LM-01, LM-02 | CRITICAL | 3×3 = 9 |
| **F-GAP-02** | Department ACLs configured but not enforced | NS-02 | HIGH | 2×3 = 6 |
| **F-GAP-03** | DMZ isolation depends on unenforced ACL; two-interface firewall limits control | NS-03 | HIGH | 2×3 = 6 |
| **F-GAP-04** | SSH management not deployed to 5 of 8 Layer 3 devices | AC-01 | HIGH | 2×3 = 6 |
| **F-GAP-05** | RADIUS non-functional; local credentials only; weak key material | AC-02 | HIGH | 2×3 = 6 |
| **F-GAP-06** | No log retention policy documented | LM-03 | MEDIUM | 2×2 = 4 |
| **F-GAP-07** | No backup design, data inventory, or restore testing | DP-01, DP-02 | MEDIUM | 2×2 = 4 |
| **F-GAP-08** | No 24/7 incident response; support only Mon-Fri 08:00–18:00 | DP-03 | MEDIUM | 2×2 = 4 |
| **F-GAP-09** | Single points of failure at Internet edge (1 router, 1 firewall, 1 Syslog server) | HA-02 | MEDIUM | 1×3 = 3 |
| **F-GAP-10** | No security requirements in supplier contract; refurbished hardware option | SC-01 | MEDIUM | 2×2 = 4 |
| **F-GAP-11** | Dossier internally inconsistent (Syslog IPs, RADIUS status, VLAN counts, logging claims) | SC-01, QA | MEDIUM | 2×2 = 4 |
| **F-GAP-12** | Internet-facing FTP uses cleartext transfer and default-style cisco/cisco account | NS-03 | HIGH | 3×2 = 6 |

---

## Checklist Results Summary

| Area | Checks | ✅ Pass | ❌ Fail | ⚠️ Not Verifiable |
|---|---:|---:|---:|---:|
| **Area 1: Network Segmentation** | 3 | 1 | 2 | 0 |
| **Area 2: Access Control** | 3 | 1 | 2 | 0 |
| **Area 3: Logging and Monitoring** | 3 | 0 | 3 | 0 |
| **Area 4: Data Protection and Incident Readiness** | 3 | 0 | 3 | 0 |
| **Area 5: Availability and Supply Chain** | 3 | 1 | 2 | 0 |
| **TOTAL** | **15** | **3** | **10** | **0** |

---

## Audit Note

A blank result means that the check has not yet been executed. A control must not be marked `Pass` solely because it is described in a document: the cited evidence must satisfy the complete pass condition. When Packet Tracer cannot reproduce a production feature, the limitation is recorded and `Fail` is assigned if the feature is essential for go-live (e.g., SVI ACL enforcement must be verified on production hardware). Where verification is genuinely impossible due to simulator limitations, `Not verifiable / Insufficient evidence` may be used; however, in this audit, all limitations have been documented and failures assigned based on the requirement to validate on production hardware before deployment.

---

## Key Audit Conclusions

1. **3 of 15 controls pass.** These are well-designed but do not guarantee overall security posture.

2. **10 of 15 controls fail.** The failures are not due to missing understanding but to incomplete or missing implementations:
   - **Logging incomplete** (1 device of 8 logging) — easily fixable
   - **Access controls configured but not enforced** — Packet Tracer bug, must test on hardware
   - **Critical services lack encryption or centralised authentication** — straightforward configuration
   - **Gaps in incident response, backup and redundancy** — require planning and documentation

3. **Go-Live Gate:** 6 findings must be fixed before production (1 CRITICAL + 5 HIGH + F-GAP-11 dossier consistency + F-GAP-12 FTP).

4. **Residual Risk (Post-Launch):** 5 MEDIUM findings can be addressed within 30–90 days if formally accepted by NVIDIA management.

---

**Prepared by:** Control Freaks  
**Technical Auditor:** Madumathi Singaraju  
**Regulatory Auditor:** Hanah Marroun  
**Audit Coordinator, Risk & Reporting:** Sajjad Shahpoor

**Date:** September 2026
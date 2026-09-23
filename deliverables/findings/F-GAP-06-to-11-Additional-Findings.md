# Additional Findings (F-GAP-06 to F-GAP-11) — NVIDIA Regional R&D Hub

**Project ID:** NVIDIA-REG-RD-2026-1106  
**Status:** Medium-risk findings from audit checklist

---

## F-GAP-06: No Log Retention Policy Documented

**Related Check:** LM-03 (log retention policy)

**Observation:**  
Security logs are configured on Leaf-1 and should be configured on the firewall and other devices (per F-GAP-01 remediation). However, there is no documented policy defining:
- How long logs must be retained (minimum period, justification)
- Where logs are archived or deleted
- Who has access to logs
- How logs are securely disposed

**Evidence:**
- Security Configuration: No retention policy document referenced
- VLAN Worksheet, Device Configuration: No mention of log retention
- Dossier gaps: No retention schedule found

**Compliance Reference:**
- GDPR Article 5(1)(e) — Storage limitation (logs must not be kept longer than necessary)
- ISO 27001:2022 A.8.15 — Logging (retention requirements)
- NIS2 Directive Article 21(2)(b) — Incident handling capability (requires audit trail)

**Risk Rating:** **MEDIUM** (Likelihood 2 × Impact 2 = 4)

**Recommendation:**
1. Define a justified retention period (e.g., 90 days minimum for security events, 1 year for compliance logs)
2. Document access controls (who can view, download, delete logs)
3. Specify secure deletion method (overwrite, secure erase)
4. Implement log rotation/archival in Syslog server configuration
5. Document and approve before go-live

**Timeline:** 30 days  
**Owner:** Security/Compliance team

---

## F-GAP-07: Personal-Data Inventory and Backup Design Not Documented

**Related Checks:** DP-01 (personal-data inventory), DP-02 (backup & restore)

**Observation:**  
The dossier references "Personal Data Servers" (VLAN 70) and mentions an HR export, but provides no:
1. **Data Inventory:** Which systems store what data? What categories (names, IDs, email, salary)?
2. **Processing Agreement:** Who is the controller, processor, or joint controller?
3. **Backup Design:** The testing report states "backup not simulated"; iSCSI is mentioned as "Bonus" but not as a backup strategy.
4. **Restore Testing:** No evidence of a tested restore procedure.

**Evidence:**
- VLAN Worksheet: "Personal Data Servers" listed; no detail on what data they contain
- Testing Report: "backup not simulated; iSCSI is bonus feature"
- NIS2-GDPR Incident Exercise: HR export with 48 employees mentioned; but not in audit dossier scope

**Compliance Reference:**
- GDPR Article 30 — Records of processing activities
- GDPR Article 32(1)(b)–(d) — Resilience, restoration, testing
- ISO 27001:2022 A.5.9 — Inventory of information
- ISO 27001:2022 A.8.13 — Information backup
- NIS2 Directive Article 21(2)(c) — Business continuity

**Risk Rating:** **MEDIUM** (Likelihood 2 × Impact 2 = 4)

**Recommendation:**
1. **Data Inventory:** Identify all systems storing personal data, categories, retention periods, and owners.
2. **Backup Design:** Define backup scope (which servers), frequency (daily/weekly), storage location (separate network/off-site), and retention.
3. **Restore Testing:** Conduct and document a test restore from backup to verify recovery time objective (RTO) and recovery point objective (RPO).
4. **Documentation:** Create a data processing agreement (if required by GDPR) and backup/recovery procedure.

**Timeline:** 45 days (can be post-deployment if tested before go-live)  
**Owner:** Data Protection Officer / Operations team

---

## F-GAP-08: No 24/7 Incident Response Procedure; Support Only Mon–Fri 08:00–18:00

**Related Check:** DP-03 (incident response)

**Observation:**  
The contract specifies support hours as Monday–Friday, 08:00–18:00. This means:
- A security incident discovered on Friday evening has no on-call response until Monday.
- NIS2 requires notification to CCB within 24 hours of awareness.
- GDPR requires notification to APD within 72 hours of breach discovery.
- No documented incident response procedure exists for after-hours incidents.

**Evidence:**
- Contract §6: "Support Mon-Fri 08:00-18:00 only"
- No 24/7 IR plan, on-call roster, or escalation procedure in dossier
- Incident Exercise (NIS2-GDPR) shows incident discovered at 07:55; cannot meet 24h CCB deadline without 24/7 capability

**Compliance Reference:**
- NIS2 Directive Article 21(2)(b) — Incident detection and response within 24 hours
- GDPR Article 33 — Personal-data breach notification within 72 hours
- ISO 27001:2022 A.5.24 — Incident management planning
- ISO 27001:2022 A.5.26 — Response to information security incidents

**Risk Rating:** **MEDIUM** (Likelihood 2 × Impact 2 = 4)

**Recommendation:**
1. **Draft IR procedure:** Define roles (incident manager, technical lead, legal, DPO), escalation paths, evidence preservation.
2. **On-call coverage:** Establish 24/7 rotation for security incidents (minimum: weekend and evening coverage).
3. **Notification process:** Document NIS2 (24h to CCB), GDPR (72h to APD, 72h to employees if high risk).
4. **Contact list:** Compile emergency contacts for CCB, CERT.be, APD, external forensics provider.
5. **Testing:** Conduct a tabletop exercise or full simulation before go-live.

**Timeline:** 60 days (can be parallel to go-live)  
**Owner:** Security/Compliance team

---

## F-GAP-09: Single Points of Failure at Internet Edge (1 Router, 1 Firewall, 1 Syslog Server)

**Related Check:** HA-02 (internet gateway redundancy)

**Observation:**  
The network design has:
- **1 primary Internet edge router** (Edge-Router-1): If it fails, all Internet connectivity is lost.
- **1 ASA firewall:** If it fails, internal traffic cannot reach Internet (and vice versa).
- **1 Syslog server** (192.168.70.16): If it fails, logging stops (impacts F-GAP-01 remediation).

The dossier mentions an unconfigured "standby router" but provides no evidence of failover configuration (OSPF, BGP, or static route backup).

**Evidence:**
- Device Configuration §7–8: One active edge router; one "unconfigured standby"
- Security Configuration §2.2: Single Syslog server listed
- No redundancy or failover configuration documented

**Compliance Reference:**
- ISO 27001:2022 A.8.14 — Redundancy of information processing facilities
- NIS2 Directive Article 21(2)(c) — Business continuity and disaster recovery

**Risk Rating:** **MEDIUM** (Likelihood 1 × Impact 3 = 3)
- *Likelihood 1:* Failover is planned; documentation gap, not complete absence.
- *Impact 3:* Internet outage affects entire hub; business-critical production and R&D VLANs offline.

**Recommendation:**
1. **Edge router failover:** Configure the standby router with OSPF/ECMP or static routing to take over if primary fails. Test failover (shutdown primary, confirm standby takes traffic).
2. **Firewall redundancy:** Plan dual ASA with state synchronization OR implement edge router failover before firewall (reduces single point of failure risk).
3. **Syslog redundancy:** Implement secondary Syslog server with log forwarding from primary (or use a centralized log aggregator).
4. **Documentation:** Create a failover runbook with RTO/RPO targets.

**Timeline:** 6 months (post-launch acceptable; not a go-live blocker)  
**Owner:** Network team

---

## F-GAP-10: No Security Requirements in Supplier Contract

**Related Check:** SC-01 (vendor security)

**Observation:**  
The contract with the network contractor (for design and implementation) does not include:
- Security responsibilities (who ensures what is secure)
- Audit rights (NVIDIA's right to audit contractor's security practices)
- Incident notification requirements (timeline for reporting security issues)
- SLA for security updates or patches
- Subcontractor screening requirements

The RFQ mentions a "refurbished hardware" cost option but does not specify whether refurbished equipment has been sanitized or certified.

**Evidence:**
- Contract §2–6: General terms, no security clauses
- Cost Breakdown: "Refurbished hardware option" mentioned; no data sanitization or certification noted

**Compliance Reference:**
- ISO 27001:2022 A.5.19 — Information security in supplier relationships
- ISO 27001:2022 A.5.20 — Addressing information security within supplier agreements
- NIS2 Directive Article 21(2)(d) — Supply-chain security

**Risk Rating:** **MEDIUM** (Likelihood 2 × Impact 2 = 4)

**Recommendation:**
1. **Amend contract:** Add security clauses covering:
   - Contractor's obligation to implement findings (F-GAP-01 to F-GAP-05) per remediation timeline
   - Right for NVIDIA to audit contractor's security practices
   - Incident notification: contractor must notify NVIDIA of any security incidents within 24 hours
   - SLA for critical patches: 7 days for CRITICAL, 30 days for HIGH
2. **Refurbished hardware:** Require certificate of data sanitization and functional testing before acceptance
3. **Subcontractors:** Require contractor to vet and audit subcontractors under the same terms

**Timeline:** Before contract acceptance or go-live  
**Owner:** Procurement/Legal team

---

## F-GAP-11: Dossier Internally Inconsistent (Contradicting Claims, Wrong IPs, Unclear Requirements)

**Related Check:** All (quality assurance)

**Observation:**  
The audit dossier contains internal contradictions:

| Contradiction | Location 1 | Location 2 | Impact |
|---|---|---|---|
| Syslog server IP | VLAN Worksheet: 192.168.70.16 | Testing Report Edge Router §9.2: 203.0.113.6 (firewall outside IP) | Unclear which is correct; F-GAP-01 remediation depends on this |
| RADIUS status | Testing Report §9.2: "PASS" | Security Config §7.4: "does not function" | Breaks ACL/AAA evidence chain |
| Department VLAN count | VLAN Worksheet: "9 VLANs defined" | Some sections: "10 VLANs" (VLAN 1 hardening confusion) | Asset inventory discrepancy |
| "All devices log" | Dossier summary | Security Config §2.2: "only Leaf-1 logs" | Contradicts own evidence |

**Evidence:**
- VLAN Worksheet vs. Testing Report: Different Syslog IPs
- Security Configuration §7.4 vs. Testing Report §9.2: RADIUS pass/fail conflict
- Multiple VLAN count references: 9 vs. 10

**Compliance Reference:**
- Audit quality assurance (consistency, accuracy)
- ISO 19011 — Auditing standards (evidence must be verifiable and consistent)

**Risk Rating:** **MEDIUM** (Likelihood 2 × Impact 2 = 4)
- *Likelihood 2:* Dossier was drafted by multiple authors; inconsistency is common.
- *Impact 2:* Audit team and client must spend time reconciling contradictions before validation.

**Recommendation:**
1. **Reconcile:** Identify which statement is correct for each contradiction.
2. **Validate:** Cross-check all IP addresses, device names, VLAN IDs against the Packet Tracer `.pkt` file and network design diagram.
3. **Correct:** Update all documents to use the verified values consistently.
4. **Sign-off:** Ensure technical auditor (Madumathi) and contractor confirm the corrected dossier before production validation.

**Timeline:** Before design acceptance  
**Owner:** Contractor (correction); Audit team (verification)

---

## Summary

| Finding | Rating | Score | Timeline | Go-Live Blocker? |
|---------|--------|-------|----------|---|
| F-GAP-06 (Log retention) | MEDIUM | 4 | 30 days | No |
| F-GAP-07 (Data inventory & backup) | MEDIUM | 4 | 45 days | No* |
| F-GAP-08 (Incident response) | MEDIUM | 4 | 60 days | No* |
| F-GAP-09 (Edge failover) | MEDIUM | 3 | 6 months | No |
| F-GAP-10 (Vendor SLA) | MEDIUM | 4 | Before signature | No |
| F-GAP-11 (Dossier consistency) | MEDIUM | 4 | Before acceptance | **YES** |

*F-GAP-07 & F-GAP-08 should be addressed before go-live or formally accepted as residual risk per risk register.
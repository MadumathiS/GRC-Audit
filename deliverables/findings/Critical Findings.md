# All Audit Findings (F-GAP-01 to F-GAP-12) — NVIDIA Regional R&D Hub

**Project ID:** NVIDIA-REG-RD-2026-1106  
**Audit Date:** September 2026  
**Status:** Final  
**Total Findings:** 12 (1 CRITICAL + 5 HIGH + 6 MEDIUM)

---

## 📊 Quick Reference by Severity

| Severity | Count | Findings | Go-Live Blocker? |
|----------|-------|----------|---|
| **CRITICAL** | 1 | F-GAP-01 | ✅ YES |
| **HIGH** | 5 | F-GAP-02, F-GAP-03, F-GAP-04, F-GAP-05, F-GAP-12 | ✅ YES |
| **MEDIUM** | 6 | F-GAP-06, F-GAP-07, F-GAP-08, F-GAP-09, F-GAP-10, F-GAP-11 | ⚠️ No (except F-GAP-11) |

---

# CRITICAL FINDINGS (Score 9)

## F-GAP-01: Centralized Logging Incomplete; Firewall and Switches Not Monitored

**Related Checks:** LM-01 (centralized logging), LM-02 (firewall logging)  
**Risk Rating:** **CRITICAL** (Likelihood 3 × Impact 3 = 9)

**Observation:**  
Logging configuration is incomplete. Only Leaf-1 forwards events to the Syslog server (192.168.70.16). The ASA firewall does not send logs (logging commands rejected in simulation). Catalyst switches are not configured for Syslog (command not supported in Packet Tracer). As a result:
- Firewall access attempts are not audited.
- Switch configuration changes and port security events are not logged.
- DMZ attacks (if any occur) cannot be detected at the firewall level.

**Evidence:**
- Security Configuration §2.2: "only Leaf-1 currently configured"
- Security Configuration §3.2: "Syslog configuration… could not be fully realized"
- Packet Tracer Limitations §6–7: "logging command rejected… on Catalyst switches… on ASA Firewall"
- Testing Report: Edge Router test references Syslog IP 203.0.113.6 (which is the firewall's outside interface, not the Syslog server)

**Compliance Reference:**
- ISO 27001:2022 A.8.15 Logging (evidence of user actions)
- ISO 27001:2022 A.8.16 Monitoring of activities
- GDPR Article 32 (technical measures to detect breaches)
- NIS2 Directive Article 21(2)(b) (incident detection and response capability)

**Risk Assessment:**
- *Likelihood 3:* No control prevents this. Logging can be deployed on any device; the contractor simply did not.
- *Impact 3:* Perimeter (firewall) and core (switches) events completely invisible. Incident detection depends on customer complaints, not proactive monitoring.

**Recommendation:**
1. Deploy Syslog agent on the ASA firewall and all 8 Layer 3 switches (2 Spines + 3 Leaves + Edge Router + ASA = 7 devices total sending to 192.168.70.16:514).
2. Implement before production go-live.
3. Verify: `show logging` on ASA; `show run | inc logging` on Catalyst devices; confirm logs arrive at Syslog server with timestamps.

**Timeline:** 3–5 days  
**Owner:** Network/Security team  
**Cost:** €0 (included in current contract)  
**Go-Live Blocker:** ✅ **YES** — Must demonstrate all devices logging before production.

---

# HIGH FINDINGS (Score 6)

## F-GAP-02: Department ACLs Configured but Not Enforced

**Related Check:** NS-02 (ACL enforcement)  
**Risk Rating:** **HIGH** (Likelihood 2 × Impact 3 = 6)

**Observation:**  
Access Control Lists (ACLs) are created on Leaf-1 to segregate departments:
- VLAN10-ACL, VLAN20-ACL, …, VLAN60-ACL deny inter-department traffic; permit access to servers and DMZ.
- GUEST-ACL blocks guest access to internal networks.
- DMZ-ISOLATION blocks DMZ→internal traffic.

However, ACL enforcement on SVI (Switch Virtual Interface) interfaces is inconsistent:
- `show access-lists` confirms all ACL lines are present and correct.
- `show ip interface vlan X` shows "Inbound access list is not set" despite the `ip access-group` command being accepted.
- The GUEST-ACL is the exception: it **is** enforced and tested; guest pings to internal networks return "Destination host unreachable".
- All other SVI ACLs (VLANs 10–60, DMZ) are **not** enforced in the simulation.

**Evidence:**
- Security Configuration §1.4 (SVI ACL enforcement gap): "Inbound access list is not set"
- Security Configuration §1.4 (Packet Tracer limitation): "documented Catalyst 3650 SVI ACL enforcement limitation"
- Security Configuration §3.3 (DMZ ACL): "Configured correctly and verified… but not enforced"
- Packet Tracer Limitations §1–3: "PT Enforced: No" for department and DMZ ACLs; "Exception: GUEST-ACL is actively enforcing"

**Compliance Reference:**
- ISO 27001:2022 A.8.20 Network segregation controls
- ISO 27001:2022 A.8.22 Segregation of networks

**Risk Assessment:**
- *Likelihood 2:* The control is documented and partially works (guest isolation). But enforcement is broken for 6 of 7 ACLs.
- *Impact 3:* Lateral movement between departments is not blocked. A compromised Production workstation can reach IT or Study networks.

**Recommendation:**
1. Verify on real Catalyst 3650 hardware that SVI ACL enforcement works (it should; this is a Packet Tracer simulator bug, not a device issue).
2. If hardware testing shows the same gap, move ACL enforcement to the firewall (external ACLs on the DMZ interface) or implement a VLAN access control protocol (e.g., 802.1AE MAC security).
3. Decision required before production: accept hardware-based workaround or re-architect.

**Timeline:** Must validate before go-live  
**Owner:** Network team (testing); Contractor (architecture review)  
**Cost:** No cost if hardware works; ~€3–5k + 10 days if workaround required  
**Go-Live Blocker:** ✅ **YES** — Must verify on production hardware.

---

## F-GAP-03: DMZ Isolation Depends on Non-Enforced ACL; Two-Interface Firewall Limits Segmentation

**Related Check:** NS-03 (DMZ isolation)  
**Risk Rating:** **HIGH** (Likelihood 2 × Impact 3 = 6)

**Observation:**  
The DMZ is separated from internal networks via:
1. VLAN 80 (Layer 2 isolation on Leaf-3).
2. DMZ-ISOLATION ACL denying DMZ→internal traffic (not enforced, per F-GAP-02).
3. ASA firewall with two interfaces: outside (203.0.113.0/30) and inside (10.0.3.0/30).

However, the firewall is **not positioned between the DMZ and internal networks**. Instead:
- Leaf-3 connects to the ASA's inside interface.
- All internal traffic (VLANs 10–60, 70, 90) flows through Leaf-3.
- DMZ (VLAN 80) also sits on Leaf-3 and can reach internal subnets without passing through the firewall.

Result: DMZ↔Internal traffic is **not inspected**. Only Internet↔DMZ traffic is firewalled.

**Evidence:**
- Security Configuration §10 (Network Architecture): "two-interface ASA… all internal and DMZ subnets converge on Leaf-3 SVI"
- Device Configuration §5 (Leaf-3): "Gig1/0/3 → ASA Firewall Gig1/2 (inside)"
- VLAN Worksheet: DMZ (VLAN 80) and internal VLANs (10–70, 90) all on Leaf-3
- Packet Tracer Limitations §3: DMZ-ISOLATION ACL not enforced

**Compliance Reference:**
- ISO 27001:2022 A.8.20 Network segregation
- ISO 27001:2022 A.8.22 Segregation of networks (DMZ from internal)
- NIS2 Directive Article 21 (network security, defense in depth)

**Risk Assessment:**
- *Likelihood 2:* The DMZ is on the same switch (Leaf-3) as the routing to internal networks. A VLAN hop or ACL bypass allows direct DMZ→internal access.
- *Impact 3:* If a DMZ server (e.g., web server) is compromised, attacker gains direct access to all internal subnets (servers, IT, production).

**Recommendation:**
1. Place the ASA with a dedicated DMZ interface (e.g., Gig1/3) between Leaf-3 (DMZ) and the internal network, creating a three-interface firewall topology: outside, DMZ, inside.
2. Alternative (lower cost): Use Leaf-3 as a trunk-only device and place DMZ on a separate access switch with firewall in-line.
3. Confirm DMZ traffic must pass through firewall (protocol analyzer test).

**Timeline:** 10–14 days  
**Owner:** Contractor (re-architecture); NVIDIA (budget approval)  
**Cost:** ~€5–10k for three-interface ASA upgrade or additional switching hardware  
**Go-Live Blocker:** ✅ **YES** — Must demonstrate firewall between DMZ and internal networks.

---

## F-GAP-04: SSH Management Not Deployed to 5 of 8 Layer 3 Devices

**Related Check:** AC-01 (encrypted management)  
**Risk Rating:** **HIGH** (Likelihood 2 × Impact 3 = 6)

**Observation:**  
SSH is configured on only 3 of 8 Layer 3 devices:
- ✅ Leaf-1 (COMPUTE): SSH enabled (Device Config §3.7)
- ✅ Leaf-2 (SERVICES): SSH enabled (Device Config §4)
- ✅ Edge Router: SSH enabled (Device Config §7)
- ❌ Spine-1: NO SSH config
- ❌ Spine-2: NO SSH config
- ❌ Leaf-3 (EDGE): NO SSH config
- ❌ ASA Firewall: NO SSH config
- ❌ Access switches (6×): NO SSH/VTY config

Telnet is not explicitly disabled on the devices without SSH, so it remains the default management protocol.

**Evidence:**
- Device Configuration §1 (Spine-1): No mention of SSH, VTY, or crypto key generation.
- Device Configuration §2 (Spine-2): Same—no SSH configuration.
- Device Configuration §5 (Leaf-3): No SSH configuration.
- Device Configuration §8 (ASA Firewall): No SSH or VTY lines configured.
- Device Configuration §6 (Access Switches): "No SSH/VTY configuration" noted.

**Compliance Reference:**
- ISO 27001:2022 A.8.5 Access to networks (secure authentication, encrypted channels)
- GDPR Article 32 (encryption of data in transit)
- NIS2 Directive Article 21(2)(h) (cryptographic techniques)

**Risk Assessment:**
- *Likelihood 2:* Telnet is a known-vulnerable protocol (plaintext credentials). An attacker on the network can capture passwords.
- *Impact 3:* Compromised network device credentials can lead to network re-configuration, route hijacking, or complete infrastructure takeover.

**Recommendation:**
1. Enable SSH version 2 on all 8 Layer 3 devices. Disable Telnet (`line vty 0 15 transport input ssh`).
2. Use the Leaf-1/Leaf-2 SSH setup as a template for the Spines and Leaf-3.
3. Test SSH login from management VLAN (VLAN 99) after configuration.
4. Confirm Telnet is disabled: `show ip ssh` or equivalent on each device.

**Timeline:** 2–3 hours configuration + testing  
**Owner:** Network team  
**Cost:** €0 (included in support contract)  
**Go-Live Blocker:** ✅ **YES** — Must demonstrate SSH on all Layer 3 devices.

---

## F-GAP-05: RADIUS Authentication Non-Functional; Local Credentials as Fallback; Weak Key Material

**Related Check:** AC-02 (centralized authentication)  
**Risk Rating:** **HIGH** (Likelihood 2 × Impact 3 = 6)

**Observation:**  
The AAA (Authentication, Authorization, and Accounting) framework is configured with RADIUS as the primary method and local usernames as fallback. However:

1. **RADIUS does not work:** The `radius-server host 192.168.70.12 auth-port 1645 key Cisco123` command is rejected on Leaf-1 with "Invalid input detected" (Packet Tracer limitation). As a result, the RADIUS pool is empty and all authentication falls back to local usernames.

2. **Local credentials are weak:**
   - Username: `admin` (default).
   - Password: `Cisco123` (8 characters, dictionary word, same as the RADIUS key in the configuration examples).
   - RSA key: 1024 bits (deprecated; industry standard is 2048+).
   - No account lockout, no session timeout on console.

3. **Only 3 of 8 devices have SSH/AAA configured** (see F-GAP-04), so the Spines and Leaf-3 have no documented authentication at all.

**Evidence:**
- Security Configuration §7.4: "RADIUS… does not function. Local fallback is the only working authentication path."
- Packet Tracer Limitations §4: "radius-server host command rejected… not supported in PT."
- Device Configuration §3.7 (Leaf-1 SSH): "username admin privilege 15 secret Cisco123; …crypto key generate rsa 1024"
- Device Configuration §8 (ASA): No username or crypto config shown.

**Compliance Reference:**
- ISO 27001:2022 A.5.17 Authentication information (strength of authentication)
- ISO 27001:2022 A.8.2 Privileged access (MFA where appropriate)
- ISO 27001:2022 A.8.5 Access control (encryption of credentials in transit)
- GDPR Article 32(1)(b) (ensuring ongoing confidentiality of credentials)
- NIS2 Directive Article 21(2)(i) (strong authentication for privileged users)

**Risk Assessment:**
- *Likelihood 2:* Weak local credentials can be cracked (8 chars, dictionary word, same value used in code examples).
- *Impact 3:* Full administrative access to the network infrastructure.

**Recommendation:**
1. Deploy a real RADIUS server (not PT simulation) and test authentication chain (device → RADIUS → user database).
2. Enforce strong local credentials as backup only:
   - Minimum 12 characters.
   - Mix of upper/lowercase, digits, special characters.
   - Unique per device (no copy-paste).
   - Different from any default or documentation example.
3. Update RSA key generation to 2048+ bits.
4. Consider Multi-Factor Authentication (MFA) for privileged users (SSH certificate + RADIUS token).

**Timeline:** 30 days  
**Owner:** Security team (credential policy); Network team (implementation)  
**Cost:** RADIUS server hardware/license already budgeted; MFA would require additional ~€5–10k  
**Go-Live Blocker:** ✅ **YES** — RADIUS must be operational; local credentials must be strong.

---

## F-GAP-12: Internet-Facing FTP Service Uses Cleartext Transfer and a Default-Style Account

**Related area:** Data Protection / Perimeter Security (contract §2.3 requires "Secure FTP/SFTP services")  
**Risk Rating:** **HIGH** (Likelihood 3 × Impact 2 = 6)

**Status:** Additional finding, identified during evidence review of the Testing Report screenshots (pp. 28–29, 45). Not one of the original 15 checklist controls — added here as a supplementary finding.

**Observation:**
The ASA firewall's outside-facing ACL permits FTP control and data traffic from the Internet directly to the DMZ-FTP server (192.168.80.11):

```
access-list OUTSIDE-IN extended permit tcp any host 192.168.80.11 eq ftp
access-list OUTSIDE-IN extended permit tcp any host 192.168.80.11 eq 20
```

FTP is a cleartext protocol: usernames, passwords and file contents are sent unencrypted. The FTP server's own user table shows an account named `cisco` with password `cisco`, holding **Read, Write, Delete, Rename and List** permissions, alongside a second account `nvidia_ftp` / `cisco` with Read, Write and List. A login test in the Testing Report succeeds using the `cisco` account. This account is reachable from the public Internet through the ACL above.

**Evidence:**
- `evidence/screenshots/ASA-OUTSIDE-IN-ACL-permits-FTP.jpg` — ASA `show access-list` output, confirming `ftp` (21) and `20` are permitted to 192.168.80.11 from any source.
- `evidence/screenshots/FTP-Server-User-Table-cisco-cisco-account.jpg` — FTP server Services tab, showing the `cisco` / `cisco` account with RWDNL permissions.
- `evidence/screenshots/FTP-Login-Using-cisco-Account.jpg` — a successful FTP login using the `cisco` account.
- Contract, §2.3 (Security Implementation): the contractor is required to deploy "Secure FTP/SFTP services."
- Packet Tracer Limitations, §10 (SFTP Service): "SFTP was unavailable in Packet Tracer server services. FTP was used as a substitute for file transfer simulation… SFTP would replace FTP for DMZ-FTP… instead of two unencrypted ports (20, 21)." — the dossier itself records this as a simulator substitution, not a deliberate design choice.

**Compliance Reference:**
- ISO/IEC 27001:2022 A.8.24 (Use of cryptography)
- ISO/IEC 27001:2022 A.5.17 (Authentication information)
- GDPR Article 32(1)(a) (encryption as an appropriate technical measure)
- NIS2 Directive Article 21(2)(h) (cryptography)

**Risk Assessment:**
- *Likelihood 3:* The service is reachable directly from the Internet, uses a cleartext protocol, and one of its two accounts has a default-style, easily-guessed username. No control stands in the way of a credential-guessing or traffic-interception attempt.
- *Impact 2:* Limited to the DMZ-FTP service and whatever it holds, but the `cisco` account's delete and rename rights mean a successful login can destroy or replace files, not just read them.

**Recommendation:**
1. Replace FTP with SFTP (or FTPS) on the production DMZ-FTP server, as the contract already requires.
2. Remove the `cisco` account; issue each legitimate user a named account with a unique credential; remove delete/rename rights from any account that only needs to upload.
3. Update the ASA `OUTSIDE-IN` ACL to permit only the chosen secure port, and remove the `ftp` and `20` lines.

**Timeline:** Before go-live (no dependencies on other findings)  
**Owner:** Network/Security team for ACL; Contractor for SFTP enablement  
**Cost:** €0 (SFTP supported on real hardware; only Packet Tracer lacks it)  
**Go-Live Blocker:** ✅ **YES** — FTP must be replaced with SFTP; cisco account must be removed.

---

# MEDIUM FINDINGS (Score 3–4)

## F-GAP-06: No Log Retention Policy Documented

**Related Check:** LM-03 (log retention policy)  
**Risk Rating:** **MEDIUM** (Likelihood 2 × Impact 2 = 4)

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

**Risk Assessment:**
- *Likelihood 2:* A documented retention policy is a standard operational control.
- *Impact 2:* Without a policy, logs may be over-retained (violating GDPR) or deleted prematurely (breaking incident investigations).

**Recommendation:**
1. Define a justified retention period (e.g., 90 days minimum for security events, 1 year for compliance logs)
2. Document access controls (who can view, download, delete logs)
3. Specify secure deletion method (overwrite, secure erase)
4. Implement log rotation/archival in Syslog server configuration
5. Document and approve before go-live

**Timeline:** 30 days  
**Owner:** Security/Compliance team  
**Cost:** €0  
**Go-Live Blocker:** ⚠️ No (but should be addressed before go-live)

---

## F-GAP-07: Personal-Data Inventory and Backup Design Not Documented

**Related Checks:** DP-01 (personal-data inventory), DP-02 (backup & restore)  
**Risk Rating:** **MEDIUM** (Likelihood 2 × Impact 2 = 4)

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

**Risk Assessment:**
- *Likelihood 2:* Backup is standard practice; the gap is documentation, not absence of a backup system.
- *Impact 2:* Without a documented backup/restore design, the organization cannot reliably recover from data loss or verify recovery times.

**Recommendation:**
1. **Data Inventory:** Identify all systems storing personal data, categories, retention periods, and owners.
2. **Backup Design:** Define backup scope (which servers), frequency (daily/weekly), storage location (separate network/off-site), and retention.
3. **Restore Testing:** Conduct and document a test restore from backup to verify recovery time objective (RTO) and recovery point objective (RPO).
4. **Documentation:** Create a data processing agreement (if required by GDPR) and backup/recovery procedure.

**Timeline:** 45 days (can be post-deployment if tested before go-live)  
**Owner:** Data Protection Officer / Operations team  
**Cost:** €0–€5k depending on backup solution  
**Go-Live Blocker:** ⚠️ No (but recommended before production)

---

## F-GAP-08: No 24/7 Incident Response Procedure; Support Only Mon–Fri 08:00–18:00

**Related Check:** DP-03 (incident response)  
**Risk Rating:** **MEDIUM** (Likelihood 2 × Impact 2 = 4)

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

**Risk Assessment:**
- *Likelihood 2:* Incidents do not follow business hours; the gap is preparedness.
- *Impact 2:* Delayed incident response and breach notification can extend damage and violate regulatory deadlines.

**Recommendation:**
1. **Draft IR procedure:** Define roles (incident manager, technical lead, legal, DPO), escalation paths, evidence preservation.
2. **On-call coverage:** Establish 24/7 rotation for security incidents (minimum: weekend and evening coverage).
3. **Notification process:** Document NIS2 (24h to CCB), GDPR (72h to APD, 72h to employees if high risk).
4. **Contact list:** Compile emergency contacts for CCB, CERT.be, APD, external forensics provider.
5. **Testing:** Conduct a tabletop exercise or full simulation before go-live.

**Timeline:** 60 days (can be parallel to go-live)  
**Owner:** Security/Compliance team  
**Cost:** €0–€10k depending on on-call platform  
**Go-Live Blocker:** ⚠️ No (but strongly recommended before production)

---

## F-GAP-09: Single Points of Failure at Internet Edge (1 Router, 1 Firewall, 1 Syslog Server)

**Related Check:** HA-02 (internet gateway redundancy)  
**Risk Rating:** **MEDIUM** (Likelihood 1 × Impact 3 = 3)

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

**Risk Assessment:**
- *Likelihood 1:* Failover is planned; documentation gap, not complete absence.
- *Impact 3:* Internet outage affects entire hub; business-critical production and R&D VLANs offline.

**Recommendation:**
1. **Edge router failover:** Configure the standby router with OSPF/ECMP or static routing to take over if primary fails. Test failover (shutdown primary, confirm standby takes traffic).
2. **Firewall redundancy:** Plan dual ASA with state synchronization OR implement edge router failover before firewall (reduces single point of failure risk).
3. **Syslog redundancy:** Implement secondary Syslog server with log forwarding from primary (or use a centralized log aggregator).
4. **Documentation:** Create a failover runbook with RTO/RPO targets.

**Timeline:** 6 months (post-launch acceptable; not a go-live blocker)  
**Owner:** Network team  
**Cost:** €3–5k for router/firewall redundancy  
**Go-Live Blocker:** ⚠️ No (post-launch acceptable)

---

## F-GAP-10: No Security Requirements in Supplier Contract

**Related Check:** SC-01 (vendor security)  
**Risk Rating:** **MEDIUM** (Likelihood 2 × Impact 2 = 4)

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

**Risk Assessment:**
- *Likelihood 2:* Contract amendments are routine; no technical blocker.
- *Impact 2:* Without security clauses, remediation timelines and audit rights are not legally binding.

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
**Cost:** €0  
**Go-Live Blocker:** ✅ Yes (before contract signature; can be addressed in parallel with technical remediation)

---

## F-GAP-11: Dossier Internally Inconsistent (Contradicting Claims, Wrong IPs, Unclear Requirements)

**Related Check:** All (quality assurance)  
**Risk Rating:** **MEDIUM** (Likelihood 2 × Impact 2 = 4)

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

**Risk Assessment:**
- *Likelihood 2:* Dossier was drafted by multiple authors; inconsistency is common.
- *Impact 2:* Audit team and client must spend time reconciling contradictions before validation.

**Recommendation:**
1. **Reconcile:** Identify which statement is correct for each contradiction.
2. **Validate:** Cross-check all IP addresses, device names, VLAN IDs against the Packet Tracer `.pkt` file and network design diagram.
3. **Correct:** Update all documents to use the verified values consistently.
4. **Sign-off:** Ensure technical auditor (Madumathi) and contractor confirm the corrected dossier before production validation.

**Timeline:** Before design acceptance  
**Owner:** Contractor (correction); Audit team (verification)  
**Cost:** €0  
**Go-Live Blocker:** ✅ **YES** — Dossier must be internally consistent before production validation.

---

# Summary Table: All 12 Findings

| Finding | Related Checks | Severity | Score | Likelihood | Impact | Timeline | Go-Live Blocker? | Owner |
|---------|---|---|---|---|---|---|---|---|
| **F-GAP-01** Centralized logging incomplete | LM-01, LM-02 | CRITICAL | 9 | 3 | 3 | 3–5 days | ✅ YES | Network/Security |
| **F-GAP-02** ACLs not enforced | NS-02 | HIGH | 6 | 2 | 3 | Before go-live | ✅ YES | Network team |
| **F-GAP-03** DMZ topology weak | NS-03 | HIGH | 6 | 2 | 3 | 10–14 days | ✅ YES | Contractor/NVIDIA |
| **F-GAP-04** SSH incomplete | AC-01 | HIGH | 6 | 2 | 3 | 2–3 hours | ✅ YES | Network team |
| **F-GAP-05** RADIUS + weak creds | AC-02 | HIGH | 6 | 2 | 3 | 30 days | ✅ YES | Security team |
| **F-GAP-06** No log retention policy | LM-03 | MEDIUM | 4 | 2 | 2 | 30 days | ⚠️ No | Security/Compliance |
| **F-GAP-07** Data inventory & backup | DP-01, DP-02 | MEDIUM | 4 | 2 | 2 | 45 days | ⚠️ No* | DPO / Operations |
| **F-GAP-08** No 24/7 IR procedure | DP-03 | MEDIUM | 4 | 2 | 2 | 60 days | ⚠️ No* | Security/Compliance |
| **F-GAP-09** Single points of failure | HA-02 | MEDIUM | 3 | 1 | 3 | 6 months | ⚠️ No | Network team |
| **F-GAP-10** No vendor SLA | SC-01 | MEDIUM | 4 | 2 | 2 | Before signature | ⚠️ Yes* | Procurement/Legal |
| **F-GAP-11** Dossier inconsistent | QA | MEDIUM | 4 | 2 | 2 | Before acceptance | ✅ YES | Contractor/Audit |
| **F-GAP-12** FTP cleartext + cisco account | Data Protection | HIGH | 6 | 3 | 2 | Before go-live | ✅ YES | Network/Security |

*F-GAP-07 & F-GAP-08 should be addressed before go-live or formally accepted as residual risk per risk register.  
*F-GAP-10 can be addressed in parallel to technical remediation.

---

## Conclusion

**Before Production Go-Live:** 6 findings must be addressed (1 CRITICAL + 5 HIGH + F-GAP-11 dossier consistency):
1. **F-GAP-01:** Enable Syslog on all devices
2. **F-GAP-02:** Verify ACL enforcement on real hardware
3. **F-GAP-03:** Implement three-interface DMZ firewall
4. **F-GAP-04:** Deploy SSH on all Layer 3 devices
5. **F-GAP-05:** Operational RADIUS server + strong credentials
6. **F-GAP-11:** Resolve dossier contradictions
7. **F-GAP-12:** Replace FTP with SFTP; remove cisco account

**After Go-Live (within 90 days):** 5 MEDIUM findings can be addressed per risk-acceptance timeline.

**Total Remediation Cost:** €8–20k (primarily DMZ firewall upgrade; rest are configuration/labor).  
**Total Remediation Timeline:** 14–30 days critical path (F-GAP-03 gate); others parallel.

---

*Prepared by Control Freaks — Madumathi Singaraju (Technical), Hanah Marroun (Regulatory), Sajjad Shahpoor (Coordination & Reporting)*